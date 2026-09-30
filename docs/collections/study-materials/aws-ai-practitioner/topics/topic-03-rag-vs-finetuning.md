# Topic 3: RAG vs. Fine-Tuning

> Source: [AWS Knowledge Bases docs](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html) | [AWS Custom Models](https://docs.aws.amazon.com/bedrock/latest/userguide/custom-models.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

---

## Quick Revision (TL;DR)

- **RAG:** no weight change; injects **current/private** data at inference; gives **citations**; cheap and easy to update. Best for frequently changing facts.
- **Fine-tuning:** **changes weights**; teaches **style/tone/format/domain**; needs labeled data; costly; knowledge becomes stale. Best for stable, specialized tasks.
- **Agents:** let a model **take actions** via tools/APIs (multi-step workflows) — RAG only *retrieves*.
- **Try cheapest first:** prompt engineering → RAG → fine-tuning → RAG + fine-tuning.
- **Fine-tuning is poor at inserting new facts** — use **RAG** for current knowledge.
- **RAG infra:** embeddings + vector store + chunking + top-K similarity search.

---

## Overview

Both RAG and fine-tuning improve foundation model performance for specific use cases, but they solve **different problems** through different mechanisms. Choosing the right approach (or combination) is a core exam topic.

| | RAG | Fine-Tuning |
|-|-----|-------------|
| **Changes model weights?** | No | Yes |
| **Requires training data?** | No | Yes |
| **Provides citations?** | Yes | No |
| **Data freshness?** | Real-time | Frozen at training time |
| **Changes model behavior/style?** | No | Yes |

---

## Retrieval-Augmented Generation (RAG)

### Definition

RAG is a technique that retrieves relevant information from external data sources **at inference time** and injects it into the prompt context to improve response relevancy and accuracy.

> "While foundation models have general knowledge, you can further improve their responses by using Retrieval Augmented Generation (RAG)." — AWS documentation

### How It Works

```
User Query
	↓
1. Convert query to embedding vector
	↓
2. Similarity search in vector store (cosine similarity)
	↓
3. Retrieve top-K most relevant document chunks
	↓
4. Inject retrieved chunks into FM prompt as context
	↓
5. FM generates grounded response
	↓
6. Response includes citations to source documents
```

### Best For

- Proprietary or private data the model was never trained on
- Information that changes frequently (pricing, policies, news, product catalogs)
- Applications requiring **citations** and **source attribution**
- Reducing hallucinations about factual enterprise data
- Large knowledge bases (documentation, wikis, support articles)
- Compliance scenarios where traceable sources are required

### Limitations

| Limitation | Detail |
|-----------|--------|
| **Retrieval quality** | Output quality depends on chunking strategy and embedding quality |
| **No behavior change** | Cannot change the model's tone, style, or output format |
| **Added latency** | Retrieval step adds delay before inference |
| **Infrastructure** | Requires vector store and ingestion pipeline |
| **Context window** | Retrieved chunks consume tokens; very large retrievals can exceed context limits |

---

## Fine-Tuning

### Definition

Fine-tuning updates the model's **weights** using a curated dataset to specialize its behavior, tone, domain knowledge, or output format.

### Types on Amazon Bedrock

| Type | What It Does | Use Case |
|------|-------------|----------|
| **Instruction fine-tuning** | Teaches the model specific instruction formats and task patterns | Consistent task execution and output format |
| **Continued pre-training** | Exposes the model to more domain text to internalize vocabulary | Legal, medical, financial domain expertise |
| **Distillation** | Transfers knowledge from a large teacher model to a smaller student model | Reduce inference cost while preserving capability |

### Best For

- Teaching a specific **writing style, tone, or persona**
- Improving performance on a narrow, well-defined task with a consistent schema
- Internalizing domain-specific vocabulary and jargon
- Reducing prompt length — bakes instructions into the model weights
- Tasks where the underlying knowledge is **stable** and rarely changes

### Limitations

| Limitation | Detail |
|-----------|--------|
| **Requires labeled data** | High-quality training pairs are costly to create |
| **Catastrophic forgetting** | Model may lose general capabilities while specializing |
| **Knowledge staleness** | Embedded knowledge becomes outdated as the world changes |
| **Retraining cost** | Expensive compute required every time data needs updating |
| **No citations** | Model cannot cite where it learned information |
| **Overfitting risk** | Small datasets can cause the model to overfit to training examples |

---

## Decision Matrix

Use this table to determine which approach to use:

| Criterion | Use RAG | Use Fine-Tuning |
|-----------|---------|-----------------|
| Data changes frequently | ✅ | ❌ |
| Need source citations | ✅ | ❌ |
| Large corpus of documents | ✅ | ❌ |
| No labeled training data available | ✅ | ❌ |
| Need to change model tone/style | ❌ | ✅ |
| Narrow, stable task with consistent schema | ❌ | ✅ |
| Domain-specific jargon internalization | ❌ | ✅ |
| Reduce token cost per request | ❌ | ✅ |
| Compliance: auditable source traceability | ✅ | ❌ |

---

## Combined Approach: RAG + Fine-Tuning

These approaches are **not mutually exclusive**. Using both provides:

1. **Fine-tuning** — teaches the model to understand domain jargon and produce the correct output format
2. **RAG** — provides current, factual grounding at inference time

**Example:** A medical Q&A system where the model is fine-tuned on clinical language and terminology, AND uses RAG against an up-to-date drug database.

---

## Agents: The Third Customization Approach

The exam frames foundation-model customization as **three** approaches: **fine-tuning, RAG, and agents**. Fine-tuning and RAG make a model **know** more; agents let a model **do** more.

| Approach | What it changes | Best for |
|----------|-----------------|----------|
| **Fine-tuning** | Model weights (style, tone, task behavior) | Stable, specialized tasks with a consistent format |
| **RAG** | The **input** (adds retrieved context) | Frequently changing or private **knowledge**; grounded, cited answers |
| **Agents** | The **workflow** (plans and calls tools) | Multi-step tasks that must **take actions** (API calls, transactions) |

> **Key distinction:** RAG only **retrieves information**; an **agent** can **take actions** by calling tools/APIs (look up an order, process a refund, send an email). If a scenario needs multi-step reasoning that *does* something, the answer is an **agent**, not RAG.

### Agent vs. Standard Chatbot

| | Agent | Standard Chatbot |
|-|-------|------------------|
| **Capability** | Plans and executes multi-step tasks using tools | Generates text responses only |
| **Actions** | Calls APIs, databases, functions | Takes no external action |
| **Complexity** | Needs careful prompting/tool design | Simpler to build, more limited |

> On AWS, agents are built with **Amazon Bedrock Agents / AgentCore** — see [topic-01-amazon-bedrock.md](topic-01-amazon-bedrock.md).

### Choosing the Approach (exam pattern)

| Scenario signal | Lean toward |
|-----------------|-------------|
| "Frequently changing data" / "private data that must stay current" | **RAG** |
| "Consistent specialized tone/style" / "stable task like legal review" | **Fine-tuning** |
| "Multi-step task" / "take actions" / "orchestrate tools" | **Agent** |
| "Insert new factual knowledge" | **RAG** (fine-tuning is poor at reliably adding new facts) |

---

## Prompt Engineering as the First Option

Before investing in RAG or fine-tuning, always try:

1. **Prompt engineering** (zero-cost, fastest iteration)
2. **RAG** (moderate cost, no training required)
3. **Fine-tuning** (higher cost, slower iteration, requires data)
4. **RAG + Fine-tuning** (highest performance potential)

---

## Embeddings and Vector Stores (RAG Infrastructure)

| Concept | Description |
|---------|-------------|
| **Embedding** | A numerical vector representation of text that captures semantic meaning |
| **Vector store** | A database optimized for storing and searching embedding vectors by similarity |
| **Chunking** | Splitting documents into smaller segments before embedding; affects retrieval quality |
| **Similarity search** | Finds the most semantically similar chunks to a query using cosine or dot product similarity |
| **Top-K retrieval** | Returns the K most similar chunks; K is a tunable parameter |

**Supported vector stores in Amazon Bedrock (Customer-managed KB):**
- Amazon OpenSearch Serverless
- Amazon Aurora (PostgreSQL with pgvector)
- Amazon Neptune Analytics
- Pinecone, Redis, MongoDB (third-party)

---

## Knowledge Test — RAG vs. Fine-Tuning

**Q1.** A financial services company wants their chatbot to answer questions about their daily-updated product pricing catalog (10,000+ products). Which approach is most appropriate?

- A) Fine-tuning, because the dataset is large
- B) RAG, because the data changes frequently and grounded responses with citations are needed
- C) Continued pre-training on all pricing data
- D) Prompt engineering alone is sufficient

**Q2.** A company wants their AI assistant to always respond in a formal legal writing style, never using casual language, even without extensive per-prompt instructions. Which approach best addresses this?

- A) RAG with a style guide document
- B) Increasing max_tokens
- C) Fine-tuning on formal legal writing examples
- D) Adding a system prompt every time

**Q3.** Which is a key limitation of fine-tuning compared to RAG?

- A) Fine-tuned models cannot be deployed on Bedrock
- B) Fine-tuning does not reduce token usage
- C) Fine-tuned knowledge becomes stale as the real world changes
- D) Fine-tuning always results in slower inference

**Q4.** A RAG pipeline is returning irrelevant chunks, resulting in poor final responses. Which component is most likely the cause?

- A) The foundation model's temperature is too high
- B) The vector store chunking strategy or embedding quality is poor
- C) The guardrails are blocking retrieval
- D) The knowledge base requires fine-tuning

**Q5.** Which Amazon Bedrock fine-tuning method transfers knowledge from a large model to a smaller, more cost-efficient model?

- A) Instruction fine-tuning
- B) Continued pre-training
- C) Distillation
- D) RLHF

**Q6.** A compliance team requires that every AI-generated response can be traced back to a specific source document. Which approach satisfies this requirement?

- A) Fine-tuning with labeled compliance data
- B) Continued pre-training on compliance documents
- C) RAG with citations enabled in Amazon Bedrock Knowledge Bases
- D) Chain-of-thought prompting

**Q7.** A developer wants to minimize the number of tokens used per request by eliminating the need for a long system prompt on every call. Which approach achieves this?

- A) RAG
- B) Instruction fine-tuning (bakes instructions into model weights)
- C) Increasing context window size
- D) Using the Converse API

**Q8.** A customer-service application must look up a customer's order, process a refund, and email a confirmation — a multi-step task that takes real actions. Which customization approach fits best?

- A) RAG, because it retrieves the order details
- B) An agent, because it plans and executes actions using tools
- C) Fine-tuning on past refund emails
- D) Prompt engineering alone

**Q9.** A team needs their assistant to answer questions using this quarter's brand-new internal sales figures. Why is fine-tuning a poor choice for inserting this knowledge?

- A) Fine-tuning cannot run on AWS
- B) Fine-tuning is best for style/behavior and is unreliable at inserting new facts; RAG is the correct choice for current factual data
- C) Fine-tuning always causes hallucinations
- D) Fine-tuning requires a vector database

**Q10.** What is the key difference between a RAG system and an agent?

- A) RAG changes model weights; agents do not
- B) RAG only retrieves information to ground an answer; an agent can take actions by calling tools/APIs
- C) Agents cannot use a knowledge base
- D) There is no difference; the terms are interchangeable

---

**Answers:**
1. B — Frequently updated data + citation requirement = RAG
2. C — Style/behavioral change requires fine-tuning
3. C — Fine-tuned knowledge is frozen at training time and becomes stale
4. B — Retrieval quality depends on chunking strategy and embedding model quality
5. C — Distillation transfers a teacher model's capability to a smaller student model
6. C — Knowledge Bases RAG provides per-response citations to source documents
7. B — Instruction fine-tuning embeds behavior in weights, reducing prompt overhead
8. B — Multi-step tasks that take actions require an agent orchestrating tools
9. B — Fine-tuning adapts style/behavior, not reliable fact insertion; use RAG for current facts
10. B — RAG retrieves context; an agent additionally acts by invoking tools/APIs
