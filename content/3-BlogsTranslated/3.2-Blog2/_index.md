---
title: "Blog 2"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
--- 

# Introducing Local Device Emulator for Verbatim Circuits on Amazon Braket

by Mao Lin, Ammar Ali, Cody Wang, and Altan Nagji | on 26 AUG 2025 | in [Amazon Braket](https://aws.amazon.com/braket/), [Quantum Technologies](https://aws.amazon.com/blogs/quantum-computing/category/quantum-technologies/) | [Permalink](https://aws.amazon.com/blogs/quantum-computing/introducing-local-device-emulator-for-verbatim-circuits-on-amazon-braket/)

In this post, we introduce the **local device emulator** in **Amazon Braket**, a feature that lets you emulate **verbatim circuits** using calibration data taken from a real quantum processing unit (QPU). With this emulator, you can quickly check whether a circuit is compatible with a device and estimate how it behaves under realistic hardware noise before submitting a task to the QPU.

We show how to:

- Instantiate an emulator from either a live AWS device or explicit device properties.  
- Validate verbatim circuits locally and detect hardware-compatibility issues early.  
- Compare the results from a noiseless simulator, the local device emulator, and a real QPU.  
- Use these capabilities to design **noise-aware algorithms** and error-mitigation workflows.

## Overview

A **verbatim circuit** in Amazon Braket is a circuit that uses the **native gate set** of a QPU and is executed without any additional compiler passes. This gives you fine-grained control over the gates that run on hardware, but it also means your circuit must strictly adhere to the device constraints:

- Only gates supported by the device can be used.  
- Qubits must be referenced with the correct indices.  
- Two-qubit gates may only be applied to qubits that are connected in the hardware topology.

Without dedicated tooling, developers often have to send a task to the QPU just to discover that a circuit is incompatible, or spend a lot of time hand-crafting a noise model.

The local device emulator tackles both issues by:

1. **Validating** verbatim circuits against device calibration data (gate set, qubit indices, connectivity, and so on).  
2. Building a **depolarizing noise model** driven by this calibration data, and running the resulting noisy circuit on a local **density-matrix simulator**.

This lets you iterate on circuit design and noise-aware algorithms without having to run every experiment directly on quantum hardware.

## Instantiating a local device emulator

You can create a local device emulator in two ways.

### 1. From a live AWS device

First, obtain a handle to the device and then call `emulator()`:

```python
from braket.aws import AwsDevice
from braket.devices import Devices

ankaa3 = AwsDevice(Devices.Rigetti.Ankaa3)
ankaa3_emulator = ankaa3.emulator()
```

Here, the emulator is configured using the most recent **calibration data** automatically pulled from the Rigetti **Ankaa-3** QPU.

### 2. From explicit device properties

Alternatively, you can build an emulator directly from device properties that you already have (for example, a JSON snapshot you saved earlier):

```python
from braket.emulation.local_emulator import LocalEmulator

ankaa3_properties_json = ankaa3.properties.json()
ankaa3_emulator = LocalEmulator.from_json(ankaa3_properties_json)
```

- The **first approach** is ideal when you want to work against the **current calibration state** of a device.  
- The **second approach** is useful when you want to emulate from **historical calibration data** or experiment with custom device parameters.

## Validating verbatim circuits

Once an emulator is available, you can use it to verify that a verbatim circuit obeys all device constraints. The emulator checks that:

1. All **qubit indices** used in the circuit exist on the device.  
2. Only **native gates** supported by the device are present.  
3. Any two-qubit gates act only on qubits that are **connected** in the device’s topology graph.

For the examples below, we again target the Rigetti Ankaa-3 device, whose qubits are numbered 0–83 and arranged in a fixed connectivity pattern.

![anh](/images/image001-1.png)

### Example 1 – Invalid qubit index

```python
from braket.circuits import Circuit

invalid_circuit = Circuit().add_verbatim_box(
    Circuit().rx(84, 0)  # Ankaa-3 only has qubits 0–83
)

ankaa3_emulator.run(invalid_circuit, shots=10)
```

In this case, the circuit refers to qubit **84**, which does not exist. The emulator reports the mismatch before any task is sent to hardware.

### Example 2 – Non-native gate

```python
invalid_circuit = Circuit().add_verbatim_box(
    Circuit().h(0)  # H is not a native gate on Ankaa-3
)

ankaa3_emulator.run(invalid_circuit, shots=10)
```

Here, the **Hadamard (H)** gate is rejected because it is not part of Ankaa-3’s native gate set, which uses gates such as **RX**, **RZ**, and **iSWAP**.

### Example 3 – Invalid connectivity

```python
invalid_circuit = Circuit().add_verbatim_box(
    Circuit().iswap(0, 2)  # qubits 0 and 2 are not neighbors
)

ankaa3_emulator.run(invalid_circuit, shots=10)
```

This circuit attempts to apply an `iswap` gate between qubits that are not connected in the device topology, so the emulator flags a connectivity violation.

Running these checks locally helps you catch common structural issues early and avoids unnecessary QPU submissions.

## Comparing emulator results with hardware

After a circuit passes validation, you can use the local device emulator to approximate how it behaves when executed on the real device.

Internally, the emulator:

1. Constructs a **noisy circuit** by inserting depolarizing noise channels derived from calibration data after each gate.  
2. Simulates this noisy circuit with a local **density-matrix** simulator.  
3. Returns measurement counts that approximate what you would see on hardware.

Consider the following valid verbatim circuit on Ankaa-3:

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

We can now compare three different runs of this circuit:

```python
from braket.devices import LocalSimulator

# 1. Noiseless state-vector simulation
sv_task = LocalSimulator().run(valid_circuit, shots=1000)
sv_counts = sv_task.result().measurement_counts

# 2. Noisy local emulation
emulator_task = ankaa3_emulator.run(valid_circuit, shots=1000)
emulator_counts = emulator_task.result().measurement_counts

# 3. Execution on the Ankaa-3 QPU
hardware_task = ankaa3.run(valid_circuit, shots=1000)
hardware_counts = hardware_task.result().measurement_counts
```

You can inspect the noisy circuit that the emulator actually simulates:

```python
noisy_circuit = ankaa3_emulator.transform(valid_circuit)
print(noisy_circuit)
```

![anh](/images/image002-1.png)

The transformed circuit contains additional noise operations – for example, depolarizing channels after one- and two-qubit gates and bit-flip channels before measurements. This gives a more realistic, device-specific noise model than a noiseless simulation.

To compare results quantitatively, the blog defines a **fidelity** between two distributions of bit-string outcomes and uses it to measure how similar different runs are. In a representative experiment:

- The fidelity between **hardware** and **noiseless** simulation is high.  
- The fidelity between **hardware** and the **local device emulator** is even higher.

![anh](/images/image003-2.png)

This indicates that the local device emulator provides outcome distributions that are closer to those of the real QPU than a noiseless simulator does, making it a powerful tool for designing and tuning **noise-resilient quantum algorithms**.

A typical workflow is:

1. Build and validate a verbatim circuit with the emulator.  
2. Iterate on parameters and structure, observing behavior under the device-specific noise model.  
3. Run the most promising candidates on the actual QPU.

## Conclusion

The local device emulator in Amazon Braket gives you a practical way to:

- Verify that **verbatim circuits** respect device constraints.  
- Emulate device-specific **noise** based on calibration data.  
- Compare **noiseless**, **noisy emulated**, and **hardware** results within a single development loop.

By shifting much of the debugging and tuning process to your local environment, you can reduce the number of hardware runs, shorten development cycles, and build more robust **noise-aware quantum applications**.

To learn more and experiment with local device emulation:

- See the [developer guide](https://docs.aws.amazon.com/braket/latest/developerguide/braket-local-emulator.html).  
- Explore the [example notebook](https://github.com/aws/amazon-braket-examples) that demonstrates the feature in detail.

---

![anh](/images/MaoLin.jpg)

### Mao Lin

Mao Lin is a Scientist on the Amazon Braket team. His research background is in theoretical condensed matter physics, with a focus on topological phases of matter. He received his PhD in Physics from the University of Illinois and his BS in Physics from the National University of Singapore.

![anh](/images/image009.jpg)

### Ammar Ali

Ammar Ali was an Applied Scientist Intern at Amazon Braket. He is a PhD candidate at Purdue University, where he works on quantum simulations of condensed-matter systems. Outside of work, he enjoys building custom keyboards and playing racket sports.

![anh](/images/image013.jpg)

### Cody Wang

Cody Wang is a Software Development Engineer at Amazon Braket. He works on open-source libraries for writing and running quantum programs, as well as integrations with popular quantum-computing frameworks. Cody holds a BASc from the University of Toronto and is a math enthusiast, avid pianist, and sushi fan.

![anh](/images/image008.jpg)

### Altan Nagji

Altan Nagji is a Software Development Engineer at Amazon Braket. He holds Master’s and Bachelor’s degrees in Mathematics and Computer Science from the University of Texas at Austin. His interests span quantum complexity theory, algorithmic game theory, and beyond. In his free time, Altan enjoys hiking, cooking, and exploring topics across theoretical computer science.
