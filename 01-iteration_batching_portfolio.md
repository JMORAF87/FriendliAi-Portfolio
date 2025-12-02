# Understanding Iteration Batching for LLM Inference

**Study Notes by Jorge Mora**

This explainer breaks down iteration batching (also known as continuous batching) from an infrastructure engineering perspective. It covers the core problem it solves, how the approach works, and why it matters for real world GPU costs and latency in production LLM serving.

## The Problem with Conventional Batching

Traditional LLM inference systems use rigid batching: once a batch starts executing, its membership cannot change until every request in that batch completes. This creates several inefficiencies:

- **Stuck fast requests**: When one request finishes early, it cannot exit the batch and return results to the client. Instead, it waits idle while slower requests continue processing.
- **Delayed new arrivals**: Newly submitted requests must wait in a queue until the entire current batch finishes, creating unnecessary latency and poor queue management.
- **Wasted GPU cycles**: The GPU sits partially idle, processing only the slowest remaining requests instead of immediately moving on to fresh work.

These constraints compound during variable latency workloads, where request duration depends on output length, complexity, or other factors.

## Iteration Batching: Dynamic Batch Reshaping

Iteration batching (continuous batching) solves this by updating the batch composition at every decoding iteration rather than locking it for the entire request lifecycle.

**How it works:**
- Requests that finish can immediately leave the batch and return results to the client
- New requests can be injected into the batch on the fly without waiting for the current group to complete
- The scheduler makes batching decisions at each generation step, not just at the start of the batch

This approach, pioneered by FriendliAI and protected by patent, is built into Friendli Inference and supports both LLMs and multimodal models.

## Key Benefits

**Throughput**: Achieves significantly higher request and token throughput under the same latency constraints compared to conventional batching often tens of times higher in real world scenarios.

**GPU Utilization**: Eliminates idle cycles from "straggler" requests. By continuously injecting new work, iteration batching keeps GPU compute utilization high, directly improving tokens per second and tokens per dollar metrics.

**Latency Predictability**: Completed requests don't get stuck waiting for slower ones. This delivers faster and more predictable response times for end users, even during traffic spikes.

**Flexible Deployment**: Available via Friendli Container (for your own infrastructure) and Friendli Dedicated Endpoints (managed service), so teams can integrate iteration batching into existing serving stacks.

## Key Concepts

| Term | Definition |
|------|-----------|
| **LLM** | A neural network model trained on large text corpora, capable of generating and understanding human like language. |
| **LLM Inference Serving** | The process of running trained LLMs in production to handle live user requests and return generated outputs. |
| **Batching** | Processing multiple requests together on a GPU instead of sequentially, to amortize compute costs. |
| **Conventional Batching** | A fixed batch approach where the set of requests is decided before execution and cannot change until all requests finish. |
| **Iteration Batching (Continuous Batching)** | A dynamic batching strategy that updates the set of requests in the batch at each decoding iteration, allowing requests to enter and exit while the model runs. |
| **Iteration-Level Scheduling** | The scheduling policy that decides which requests are processed together at every generation step, rather than at the start of a full request. |
| **Throughput** | The volume of work completed per unit time, measured in tokens per second or requests per second. |
| **Latency** | The time elapsed between a user sending a request and receiving the first (or final) token of the response. |
| **GPU Utilization** | The percentage of a GPU's compute capacity actively used during inference, versus sitting idle. |
| **Tokens per Second (TPS)** | A performance metric measuring how many tokens the system generates per second across one or more GPUs. |
| **Tokens per Dollar** | An efficiency metric capturing how many tokens you can generate for a given cost, accounting for GPU pricing and utilization. |
| **Friendli Inference** | FriendliAI's optimized inference engine for serving LLM and multimodal models with high speed and cost efficiency. |
| **Friendli Container** | A containerized deployment option for Friendli Inference that runs in your own infrastructure. |
| **Friendli Dedicated Endpoints** | A managed service from FriendliAI hosting dedicated model endpoints without requiring you to manage GPU infrastructure. |
| **Multimodal Models** | Models capable of processing and generating across multiple data types (e.g., text, images, audio). |
| **LoRA Adapters** | Lightweight fine tuning modules (Low Rank Adaptation) that customize a base model using small parameter updates. |

## In Summary

Efficient LLM serving isn't just about fast GPUs, it's about keeping them consistently busy across variable request arrivals and durations. Conventional batching locks requests together, causing fast completions to idle and new arrivals to queue needlessly, wasting both GPU time and latency budgets.

Iteration batching fixes this by reshaping the batch at every generation step, allowing completed requests to exit immediately and new work to join without stalling the model. Built into Friendli Inference, this scheduling approach delivers order of magnitude higher throughput at equivalent latency, translating directly to better tokens per second and tokens per dollar for your production workloads.