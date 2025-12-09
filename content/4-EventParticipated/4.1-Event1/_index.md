---
title: "Event 1"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## Location & Time

- **Location:** Bitexco Financial Tower  
- **Date:** 15/11/2025  

---

## Event Objectives

- Provide a **hands-on, practical** introduction to AI/ML/GenAI capabilities on AWS, with a strong focus on **Amazon Bedrock**.
- Help participants understand how to **choose the right Foundation Model** (Claude, Llama, Titan, …) for different types of use cases.
- Introduce key **prompt engineering** techniques (few-shot, structured prompts, reasoning patterns) to improve answer quality.
- Explain the **RAG architecture** and how to leverage a **Knowledge Base** to increase factual accuracy.
- Get familiar with **Bedrock Agents** for multi-step workflows with integrated tools/APIs.
- Emphasize how to apply **Guardrails** to control content and policies, and to keep GenAI applications “safe”.
- Watch a live demo of a complete **GenAI chatbot** built end-to-end with Amazon Bedrock.

---

## Speakers

- **Hoàng Kha**  
- **Hữu Nghị**  
- **Hoàng Anh**

---

## Key Highlights

### 1. Welcome & Introduction

- **Check-in & networking:**  
  Participants register, check in, and have quick conversations with other builders and attendees working with AWS.
- **Agenda overview:**  
  Walk through the morning schedule, learning goals, and what participants can expect to take away from the workshop.
- **Ice-breaker activity:**  
  A short warm-up so everyone can share expectations and make the atmosphere more relaxed.
- **Speaker introduction:**  
  The three speakers briefly share their background and perspectives in the AI/ML/GenAI on AWS space.

### 2. Generative AI with Amazon Bedrock

- **Foundation Models:**  
  Explain what Claude, Llama, Titan are, how they differ, and how to pick a model based on needs: internal support chatbot, text summarization, content classification, reasoning-heavy tasks, etc.
- **Prompt engineering:**  
  Practice different prompt styles:
  - Structured prompts (context → instruction → constraints).
  - Few-shot prompts (with sample examples).
  - Techniques to phrase questions so that the model “understands what you want” more clearly.
- **Reasoning patterns:**  
  Demonstrate how to ask the model to reason step by step, or break a large problem into sub-tasks, to get more consistent results on complex tasks.

### 3. RAG, Agents & Guardrails

- **RAG (Retrieval-Augmented Generation):**  
  - Separate out the retrieval step from a knowledge base.  
  - Add the right context before asking the model to generate an answer.  
  - Emphasize that retrieval is critical if you want correct answers based on internal knowledge.
- **Bedrock Agents:**  
  - Create “agents” that can call tools (APIs, search, data lookup).  
  - Very useful for multi-step workflows like business assistants or process automation.
- **Guardrails:**  
  - Configure protective layers to filter sensitive content and restrict unwanted topics.  
  - Ensure GenAI applications comply with company policies and regulations.

### 4. Live Demo

- Walkthrough of building a **GenAI chatbot** on Bedrock:
  - Choose a model.
  - Design the prompt.
  - Connect to a Knowledge Base using RAG.
  - Add basic Guardrails.
  - Test a Q&A scenario similar to a real enterprise environment.

---

## Key Takeaways

### 1. Design Mindset

- **Start from the use case:**  
  First clarify the problem (FAQ bot, document search assistant, process automation, …), then work backwards to choose the right model and architecture.
- **Grounding is critical:**  
  For systems using internal knowledge, accuracy depends heavily on how you retrieve and select context, not just on “writing a longer prompt”.
- **Safety is not an afterthought:**  
  Guardrails and policies should be part of the design from the beginning, instead of being patched on right before going to production.

### 2. Technical Architecture

- **Model selection:**  
  Balance quality, cost, latency, and task type (chat, reasoning, summarization, etc.).
- **Prompt engineering:**  
  Use structured prompts plus clear examples to reduce ambiguity and make prompts easier to reuse across scenarios.
- **RAG workflow:**  
  - Retrieve relevant documents.  
  - Inject context.  
  - Generate the answer.  
  - (Optional) log sources/context for traceability.
- **Agents & tools:**  
  When workflows require multiple steps or external API calls/services, Agents help automate the flow instead of hardcoding all the logic.
- **Guardrails:**  
  Serve as a “safety gate” to reduce content risk, especially critical when the GenAI app is user-facing.

### 3. Applying to Work

- **Prototype a small Bedrock chatbot:**  
  Start with a narrow knowledge base (FAQs, internal policies) to evaluate answer quality.
- **Build a “prompt library”:**  
  Collect and standardize good prompts for common tasks: summarization, classification, Q&A, information extraction.
- **Gradually add RAG:**  
  When higher accuracy is needed, move from “prompt-only” to RAG so answers are grounded in real, up-to-date data.
- **Use Agents intentionally:**  
  Only introduce Agents when workflows truly need multiple steps and tool calls—to avoid unnecessary complexity.
- **Set up guardrails & logging:**  
  Define policies, log prompts/responses, and monitor them for continuous improvement.

---

## Event Experience

Attending **“AI/ML/GenAI on AWS”** helped me:

- Clearly see how **Amazon Bedrock** can be used to build GenAI applications that actually run in production, not just demos.
- Go from foundational topics (model selection, prompt design) to more advanced patterns like:
  - RAG  
  - Agents  
  - Guardrails
- The live demo tied all the concepts together into a complete **end-to-end chatbot workflow**, which can be adapted to real internal-knowledge use cases inside an organization.

---

## Event Photos

![anh](/images/event1.png) 