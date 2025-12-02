# Technical Writing Portfolio

A collection of technical explainers focused on LLM inference infrastructure, GPU optimization, and production serving patterns. Written as study notes while exploring FriendliAI-style inference systems.

## Featured Explainers

### 1. [Iteration Batching for LLM Inference](docs/01-iteration-batching-explainer.md)
Breaks down how continuous batching solves GPU underutilization by dynamically reshaping request batches at each decoding iteration. Covers the problem, approach, and why this matters for throughput and latency.

**Key topics**: Batching, GPU utilization, throughput optimization, latency

### 2. [Multi-LoRA Serving for LLMs](docs/02-multi-lora-serving-explainer.md)
Explains how to serve many customized model variants without duplicating the full base model. LoRA adapters enable fine-grained per-customer or per-task customization while sharing 99%+ of model weights.

**Key topics**: LoRA, parameter-efficient fine-tuning, GPU memory efficiency, multi-tenant serving

### 3. [From Hugging Face Model Card to Production Endpoint](docs/03-huggingface-friendli-endpoints-explainer.md)
Documents the Hugging Face and FriendliAI integration that enables one-click deployment from model discovery to production. Covers Dedicated and Serverless endpoint options, model support, and operational benefits.

**Key topics**: Model deployment, production infrastructure, cost optimization, developer experience

---

## About These Documents

These explainers are written for engineers and technical stakeholders who need to understand:
- Why certain infrastructure patterns matter for cost and performance
- How specific techniques solve real production problems
- The trade-offs and benefits of different serving approaches

Each document follows a consistent structure:
1. **Problem** - What challenge does this solve?
2. **Approach** - How does the solution work?
3. **Benefits** - Why does this matter for real workloads?
4. **Key Concepts** - Essential terminology and definitions

The writing style balances technical depth with accessibility, assuming readers understand LLMs and GPUs but may not be familiar with specific inference patterns.

---

## How to Use This Portfolio

- **Hiring managers**: Start with the README and choose one explainer that matches your technical depth
- **Engineers reviewing**: Each document stands alone but together show breadth across batching, serving, and deployment
- **For feedback**: Open an issue or discussion if you have questions about the content

---

## Writing Approach

These explainers emphasize:
- **Clarity over jargon**: Definitions provided for all technical terms
- **Problem-first structure**: Always explain why something matters before diving into how
- **Production focus**: Examples grounded in real infrastructure constraints
- **Consistency**: Matching style, terminology, and formatting across documents

---

## Contact & Next Steps

Interested in discussing these topics further? Feel free to reach out:
- GitHub: https://github.com/JMORAF87/FriendliAi-Portfolio
- LinkedIn: www.linkedin.com/in/jorgemoraf87
- Email: contact@jmmultiservices.xyz

---

*Last updated: December 2025*
