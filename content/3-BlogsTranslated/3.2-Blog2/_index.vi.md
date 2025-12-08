---
title: "Blog 2"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Giới thiệu Local Device Emulator cho Verbatim Circuits trên Amazon Braket

by Mao Lin, Ammar Ali, Cody Wang, and Altan Nagji | vào ngày 26 tháng 8 năm 2025 | trong [Amazon Braket](https://aws.amazon.com/braket/), [Quantum Technologies](https://aws.amazon.com/blogs/quantum-computing/category/quantum-technologies/) | [Liên kết cố định](https://aws.amazon.com/blogs/quantum-computing/introducing-local-device-emulator-for-verbatim-circuits-on-amazon-braket/)

![anh](/images/C47-1.png)

Trong bài viết này, chúng tôi giới thiệu **local device emulator** trên **Amazon Braket** – một tính năng cho phép bạn mô phỏng (**emulate**) các **verbatim circuits** bằng cách sử dụng calibration data lấy từ một quantum processing unit (QPU) thực. Nhờ emulator này, bạn có thể nhanh chóng kiểm tra circuit có tương thích với thiết bị hay không và ước lượng hành vi của nó dưới tác động của nhiễu phần cứng thực tế, trước khi gửi task lên QPU.

Trong bài, chúng tôi sẽ minh họa cách:

- Khởi tạo một emulator từ một AWS device đang hoạt động hoặc từ explicit device properties.  
- Validate verbatim circuits tại môi trường local và phát hiện sớm các lỗi tương thích phần cứng.  
- So sánh kết quả giữa noiseless simulator, local device emulator và QPU thật.  
- Tận dụng các khả năng này để thiết kế **noise-aware algorithms** và các workflow giảm sai số (error mitigation).

## Tổng quan

Một **verbatim circuit** trong Amazon Braket là circuit sử dụng **native gate set** của QPU và được thực thi mà không qua thêm compiler pass nào. Cách này cho bạn quyền kiểm soát ở mức rất thấp đối với những gate chạy trên phần cứng, nhưng đồng thời cũng yêu cầu circuit phải tuân thủ nghiêm ngặt các ràng buộc của thiết bị:

- Chỉ được dùng các gate mà thiết bị hỗ trợ.  
- Qubit phải được tham chiếu bằng đúng chỉ số (index).  
- Các two-qubit gate chỉ có thể được áp dụng cho các qubit **có kết nối** trong topology của phần cứng.

Nếu không có công cụ hỗ trợ, developer thường phải gửi task lên QPU chỉ để phát hiện ra circuit không tương thích, hoặc tốn nhiều thời gian tự xây dựng noise model.

Local device emulator giải quyết cả hai vấn đề này bằng cách:

1. **Validating** verbatim circuits dựa trên calibration data của thiết bị (gate set, chỉ số qubit, connectivity, v.v.).  
2. Xây dựng một **depolarizing noise model** từ calibration data đó và chạy noisy circuit tương ứng trên local **density-matrix simulator**.

Nhờ vậy, bạn có thể lặp (iterate) trên thiết kế circuit và các thuật toán noise-aware mà không phải chạy mọi thử nghiệm trực tiếp trên phần cứng lượng tử.

## Khởi tạo local device emulator

Bạn có thể tạo một local device emulator theo hai cách.

### 1. Từ một AWS device đang hoạt động

Đầu tiên, lấy handle đến device và gọi `emulator()`:

```python
from braket.aws import AwsDevice
from braket.devices import Devices

ankaa3 = AwsDevice(Devices.Rigetti.Ankaa3)
ankaa3_emulator = ankaa3.emulator()
```

Ở đây, emulator được cấu hình bằng **calibration data mới nhất** được lấy tự động từ QPU **Rigetti Ankaa-3**.

### 2. Từ explicit device properties

Ngoài ra, bạn có thể xây dựng emulator trực tiếp từ device properties mà bạn đã có (ví dụ, một JSON snapshot được lưu từ trước):

```python
from braket.emulation.local_emulator import LocalEmulator

ankaa3_properties_json = ankaa3.properties.json()
ankaa3_emulator = LocalEmulator.from_json(ankaa3_properties_json)
```

- Cách **thứ nhất** phù hợp khi bạn muốn làm việc với **trạng thái hiệu chuẩn hiện tại** của thiết bị.  
- Cách **thứ hai** hữu ích khi bạn muốn mô phỏng dựa trên **calibration data lịch sử** hoặc thử nghiệm với các tham số thiết bị tùy chỉnh.

## Validate verbatim circuits

Sau khi có emulator, bạn có thể dùng nó để đảm bảo một verbatim circuit tuân thủ mọi ràng buộc của thiết bị. Emulator sẽ kiểm tra:

1. Tất cả **qubit index** được sử dụng trong circuit đều tồn tại trên device.  
2. Circuit chỉ dùng **native gates** được thiết bị hỗ trợ.  
3. Mọi two-qubit gate chỉ tác động lên các qubit **có kết nối** trong topology graph của thiết bị.

Trong các ví dụ bên dưới, chúng ta tiếp tục dùng thiết bị Rigetti Ankaa-3, với qubit đánh số từ 0 đến 83 và được sắp xếp theo một connectivity cố định.

![anh](/images/C47-2.png)

### Ví dụ 1 – Qubit index không hợp lệ

```python
from braket.circuits import Circuit

invalid_circuit = Circuit().add_verbatim_box(
    Circuit().rx(84, 0)  # Ankaa-3 chỉ có qubits 0–83
)

ankaa3_emulator.run(invalid_circuit, shots=10)
```

Trong trường hợp này, circuit tham chiếu đến qubit **84**, vốn không tồn tại. Emulator phát hiện sai lệch này trước khi bất kỳ task nào được gửi lên phần cứng.

### Ví dụ 2 – Gate không thuộc native gate set

```python
invalid_circuit = Circuit().add_verbatim_box(
    Circuit().h(0)  # H không phải native gate trên Ankaa-3
)

ankaa3_emulator.run(invalid_circuit, shots=10)
```

Ở đây, gate **Hadamard (H)** bị từ chối vì nó không thuộc native gate set của Ankaa-3, vốn sử dụng các gate như **RX**, **RZ** và **iSWAP**.

### Ví dụ 3 – Kết nối không hợp lệ

```python
invalid_circuit = Circuit().add_verbatim_box(
    Circuit().iswap(0, 2)  # qubits 0 và 2 không phải neighbor
)

ankaa3_emulator.run(invalid_circuit, shots=10)
```

Circuit này cố gắng áp dụng `iswap` cho cặp qubit không có kết nối trong topology của thiết bị, nên emulator sẽ báo lỗi vi phạm connectivity.

Việc chạy các bước kiểm tra này ở môi trường local giúp bạn bắt được sớm các lỗi cấu trúc thường gặp và tránh những lần gửi task không cần thiết lên QPU.

## So sánh kết quả emulator với phần cứng

Sau khi circuit vượt qua bước validate, bạn có thể sử dụng local device emulator để xấp xỉ hành vi của circuit khi chạy trên thiết bị thật.

Bên trong, emulator sẽ:

1. Xây dựng một **noisy circuit** bằng cách chèn các depolarizing noise channel (suy ra từ calibration data) sau mỗi gate.  
2. Mô phỏng noisy circuit đó bằng local **density-matrix simulator**.  
3. Trả về measurement counts xấp xỉ những gì bạn sẽ quan sát được trên phần cứng.

Xem xét ví dụ về một valid verbatim circuit trên Ankaa-3:

```python
from numpy import pi
from braket.circuits import Circuit

valid_circuit = Circuit().add_verbatim_box(
    Circuit()
    .rx(0,  pi / 2)
    .rz(1,  pi)
    .iswap(0, 1)
    .rx(2, -pi / 2)
    .rz(3,  pi / 2)
    .iswap(2, 3)
    .iswap(1, 2)
    .rx(0, -pi / 2)
    .rz(3, -pi)
)
```

Giờ chúng ta có thể so sánh ba lần chạy khác nhau của circuit này:

```python
from braket.devices import LocalSimulator

# 1. Noiseless state-vector simulation
sv_task = LocalSimulator().run(valid_circuit, shots=1000)
sv_counts = sv_task.result().measurement_counts

# 2. Noisy local emulation
emulator_task = ankaa3_emulator.run(valid_circuit, shots=1000)
emulator_counts = emulator_task.result().measurement_counts

# 3. Chạy trên QPU Ankaa-3 thật
hardware_task = ankaa3.run(valid_circuit, shots=1000)
hardware_counts = hardware_task.result().measurement_counts
```

Bạn có thể xem noisy circuit mà emulator thực sự mô phỏng:

```python
noisy_circuit = ankaa3_emulator.transform(valid_circuit)
print(noisy_circuit)
```

![anh](/images/C47-3.png)

Circuit sau khi transform sẽ chứa thêm các thao tác noise – ví dụ, depolarizing channel sau các one-qubit và two-qubit gate, cùng các bit-flip channel trước thao tác đo. Điều này mang lại một noise model mang tính đặc thù thiết bị (device-specific) và thực tế hơn nhiều so với mô phỏng noiseless.

Để so sánh định lượng, bài blog định nghĩa một thước đo **fidelity** giữa hai phân phối bit-string và dùng nó để đo mức độ tương đồng giữa các lần chạy. Trong một thí nghiệm điển hình:

- Fidelity giữa **hardware** và mô phỏng **noiseless** là khá cao.  
- Fidelity giữa **hardware** và **local device emulator** còn cao hơn nữa.

![anh](/images/C47-4.png)

Điều này cho thấy local device emulator tạo ra phân phối kết quả gần với QPU thật hơn so với noiseless simulator, từ đó trở thành công cụ mạnh mẽ để thiết kế và hiệu chỉnh các **noise-resilient quantum algorithms**.

Một workflow điển hình sẽ là:

1. Xây dựng và validate verbatim circuit bằng emulator.  
2. Lặp trên tham số và cấu trúc circuit, quan sát hành vi dưới noise model đặc thù thiết bị.  
3. Chỉ chạy những ứng viên nhiều tiềm năng nhất trên QPU thật.

## Kết luận

Local device emulator trong Amazon Braket mang tới cho bạn một cách tiếp cận thực tiễn để:

- Kiểm chứng rằng **verbatim circuits** tuân thủ các ràng buộc của thiết bị.  
- Mô phỏng **noise** đặc thù từng device dựa trên calibration data.  
- So sánh kết quả giữa **noiseless**, **noisy emulated** và **hardware** trong cùng một vòng lặp phát triển.

Bằng cách chuyển phần lớn quá trình debug và tinh chỉnh sang môi trường local, bạn có thể giảm số lần phải chạy trên phần cứng, rút ngắn thời gian phát triển và xây dựng được các **noise-aware quantum applications** vững chắc hơn.

Để tìm hiểu thêm và thực hành với local device emulation:

- Xem [developer guide](https://docs.aws.amazon.com/braket/latest/developerguide/braket-local-emulator.html).  
- Thử [notebook ví dụ](https://github.com/aws/amazon-braket-examples) minh họa chi tiết tính năng này.

---

![anh](/images/C47-5.png)

### Mao Lin

Mao Lin là Scientist trong đội Amazon Braket. Nền tảng nghiên cứu của anh là vật lý chất ngưng tụ lý thuyết, với trọng tâm là các topological phases of matter. Anh nhận bằng Tiến sĩ Vật lý tại University of Illinois và bằng Cử nhân Vật lý tại National University of Singapore.

![anh](/images/C47-6.png)

### Ammar Ali

Ammar Ali là Applied Scientist Intern tại Amazon Braket. Anh hiện là nghiên cứu sinh Tiến sĩ tại Purdue University, nơi anh nghiên cứu các bài toán quantum simulation cho hệ chất ngưng tụ. Ngoài công việc, anh thích tự lắp bàn phím cơ và chơi các môn thể thao dùng vợt.

![anh](/images/C47-7.png)

### Cody Wang

Cody Wang là Software Development Engineer tại Amazon Braket. Anh làm việc trên các thư viện mã nguồn mở dùng để viết và chạy quantum programs, cũng như các integration với những framework quantum computing phổ biến. Cody tốt nghiệp BASc tại University of Toronto, yêu thích toán học, chơi piano và đặc biệt là sushi.

![anh](/images/C47-8.png)

### Altan Nagji

Altan Nagji là Software Development Engineer tại Amazon Braket. Anh có bằng Thạc sĩ và Cử nhân Toán học & Khoa học Máy tính từ University of Texas at Austin. Mối quan tâm của anh trải rộng từ quantum complexity theory tới algorithmic game theory. Thời gian rảnh, Altan thích đi hiking, nấu ăn và khám phá thêm các chủ đề trong theoretical computer science.
