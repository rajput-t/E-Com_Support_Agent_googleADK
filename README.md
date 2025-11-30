# E-Com Support Agent

### An AI Assistant for Product and Order Inquiries

---

## Problem Statement

In e-commerce, customers expect **instant, accurate answers** to their questions — from product availability and delivery timelines to return policies. Traditional chatbots often fail to meet these expectations. They rely on **predefined scripts**, cannot handle unstructured queries, and lack contextual memory.

For example, when a user asks, *“Is the iPhone 15 Pro in stock?”* or *“What’s the status of order ORD123?”*, rule-based bots often fail to retrieve accurate, real-time data. This leads to frustration, long wait times, and lost trust.

Meanwhile, generic large language model (LLM) chatbots, though conversational, tend to **hallucinate** — making up data or giving inconsistent factual responses.

This highlights a core challenge:

> How can we build a conversational system that combines **LLM intelligence** with **factual accuracy and structured action**?

That’s where **agents** come in.

---

## Why Agents?

Agents are intelligent systems that **think, act, and remember**. Unlike simple chatbots, agents can decide what to do next, execute functions, and integrate with tools or APIs.

Using **Google’s Agent Development Kit (ADK)**, the E-Com Support Agent leverages the power of Gemini models with structured logic, enabling it to:

* **Understand intent** using an LLM
* **Invoke custom tools** (functions) for real-time tasks
* **Access and update memory** to maintain context
* **Return accurate, dynamic responses** instead of static templates

Agents provide the flexibility to scale across multiple workflows — making them perfect for domains like e-commerce, where queries involve structured information, reasoning, and integration with other systems (inventory, shipping, returns).

---

## What I Created

### Overview

The **E-Com Support Agent** is a conversational AI assistant that handles product questions, order inquiries, and return policies for an online electronics retailer.

It uses **Gemini 2.5 Flash Lite** (for speed and cost-efficiency) and the **Google ADK framework** to integrate multiple tools, memory, and observability into one cohesive system.

### Architecture

```
+------------------------------------------------------+
|                  E-Com Support Agent                 |
|------------------------------------------------------|
| Model: Gemini 2.5 Flash Lite                         |
| Tools:                                                |
|   • get_product_info()                                |
|   • get_order_status()                                |
|   • get_return_policy()                               |
|   • load_memory()                                     |
|------------------------------------------------------|
| Memory: InMemoryMemoryService                         |
| Session: InMemorySessionService                       |
| Observability: LoggingPlugin                          |
+------------------------------------------------------+
```

### Key Tools

* **get_product_info(product_name):** Retrieves details like price, stock, and specs.
* **get_order_status(order_id):** Simulates order tracking.
* **get_return_policy(country):** Provides localized return information.
* **load_memory():** Recalls past user data like country or preferences.

When a customer asks a question, the agent uses Gemini to understand intent, selects the right tool, executes it, and formulates a natural reply.

---

## Demo

**Example Conversation:**

```
User > Hi, what’s your return policy for India?
Agent > For India: You can return most items within 10 days of delivery. Pickup is arranged for eligible locations.

User > Tell me about the MacBook Pro 14.
Agent > The MacBook Pro 14 costs $1,999 and has an M3 Pro chip, 18GB RAM, and 512GB SSD. It’s in stock (22 units).

User > What’s the status of my order ORD123?
Agent > Your order ORD123 has been shipped with DHL and will arrive in 3–5 business days.
```

Each response involves structured reasoning — not static replies. The agent invokes tools for product or order data and combines that with conversational output.

---

## The Build

### 1. **Setup and Environment**

Installed dependencies:

```bash
pip install google-adk opentelemetry-instrumentation-google-genai
```

Configured the `GOOGLE_API_KEY` and environment variables for Gemini access.

### 2. **Agent Definition**

Built a Python agent using ADK’s `LlmAgent` class, defining tools and system instructions.

```python
root_agent = LlmAgent(
    model="gemini-2.5-flash-lite",
    instruction="You are a helpful support assistant.",
    tools=[get_product_info, get_order_status, get_return_policy]
)
```

### 3. **Memory and Session Management**

Used `InMemorySessionService` and `InMemoryMemoryService` to preserve context and recall facts within and across interactions.

### 4. **Observability**

Integrated ADK’s `LoggingPlugin` to trace every function call and model interaction, allowing debugging and performance monitoring.

### 5. **Testing and Evaluation**

Created `integration.evalset.json` and `test_config.json` to test correctness.
Executed:

```bash
adk eval ecom_support_agent ecom_support_agent/integration.evalset.json
```

Results:
Tool trajectory accuracy: 1.0
Response similarity: ≥ 0.8

### 6. **Deployment Ready Structure**

Prepared the directory with `.agent_engine_config.json` and environment files for deployment to **Vertex AI Agent Engine**, Google’s managed runtime for production agents.

---

## Evaluation

The agent was tested on 20 sample queries spanning product, order, and policy tasks.
It consistently selected the correct tool and produced clear, contextually appropriate responses.

| Metric                   | Description          | Score |
| ------------------------ | -------------------- | ----- |
| Tool trajectory accuracy | Correct use of tools | 1.0   |
| Response match           | Semantic correctness | 0.9   |
| Context retention        | Session consistency  | High  |

---

## Technologies Used

* **Google ADK (Agent Development Kit)** – Agent creation framework
* **Gemini 2.5 Flash Lite** – Low-latency, cost-efficient LLM
* **Python** – Tool implementation and orchestration
* **Vertex AI Agent Engine (optional)** – Deployment target
* **ADK Evaluation Suite** – Agent testing framework
* **OpenTelemetry Logging** – Observability and metrics

---

## Architecture Flow

```
User Query  →  LLM (Gemini 2.5)  →  Tool Selection  
            →  Tool Execution (Python)  →  Memory Update  
            →  Final Response Generation  
```

Example:
“Is the iPhone 15 Pro in stock?” → Gemini interprets → `get_product_info("iPhone 15 Pro")` → returns structured data → human-readable answer.

---

## If I Had More Time

If expanded, the next steps would include:

1. **API Integration:** Connect tools to live systems like Shopify or FedEx APIs for real-time data.
2. **Persistent Memory:** Use Vertex AI Memory Bank to remember user preferences (currency, language).
3. **Multi-Agent System:** Add specialized sub-agents (e.g., Shipping Agent, Payment Agent) using the A2A protocol.
4. **Web Interface:** Build a chat UI using Streamlit or Flask for interactive demos.
5. **Advanced Observability:** Add tracing dashboards via Vertex Monitoring to track user engagement and latency.

---

## Conclusion

The **E-Com Support Agent** showcases a practical, fully functional application of **agentic AI principles** using Google’s ADK and Gemini models. It integrates **reasoning**, **tool execution**, **memory**, and **observability** — forming a blueprint for intelligent, scalable business automation.

It moves beyond simple Q&A into actionable intelligence — capable of performing tasks, recalling past interactions, and delivering consistent, accurate customer experiences.

With its modular design and deployment readiness, this project demonstrates how AI agents can transform customer service from reactive helpdesks to proactive, intelligent assistants.
