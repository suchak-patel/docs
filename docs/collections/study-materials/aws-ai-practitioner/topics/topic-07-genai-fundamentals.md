# Topic 7: Generative AI Fundamentals

> Sources: [What is Generative AI?](https://aws.amazon.com/what-is/generative-ai/) | [What is a Foundation Model?](https://aws.amazon.com/what-is/foundation-models/) | [What are LLMs?](https://aws.amazon.com/what-is/large-language-model/) | [What are Embeddings?](https://aws.amazon.com/what-is/embeddings-in-machine-learning/)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domain 2 — Fundamentals of Generative AI (24% of exam)**

---

## Quick Revision (TL;DR)

- **Generative AI** creates new content; **discriminative** AI labels/classifies existing data.
- **Foundation model (FM):** large, **pre-trained on broad unlabeled data (self-supervised)**, adaptable to many tasks. An **LLM** is an FM for text.
- A **fine-tuned** model is **not** itself a foundation model.
- **LLMs are next-token predictors** → fluent but can **hallucinate**.
- **Transformer + self-attention** is the key architecture; **tokens** are billing/limit units; **context window** = max tokens in/out; **embeddings** = semantic vectors (power RAG).
- **Adaptation cost order:** prompt engineering → RAG → fine-tuning → continued pre-training → train from scratch.
- **Inference params:** low temperature = factual/deterministic; high = creative.
- **Access FMs on AWS via Amazon Bedrock** (managed); SageMaker builds custom models.

---

## In Plain English

Imagine a head chef at a world-famous restaurant. Over years, they've developed **master recipes** — the fundamental techniques and ratios for their signature dishes. When a guest orders, the chef doesn't reinvent the dish; they take the master recipe and adapt it (a pinch of chili here, dark chocolate instead of milk there). A **foundation model** is that master recipe: a huge, general-purpose model pre-trained on an enormous, mostly **unlabeled** pile of internet text, books, and code. It knows a little about almost everything, but it's an expert in nothing specific until you adapt it.

Here's the crucial mental shift. Older "analytical" AI was a smart filing clerk — show it a photo and it tells you "cat" or "dog." That's **discriminative** AI: it labels or classifies things that already exist. **Generative** AI is a creative machine — it produces a *new* cat photo, writes a *new* email, drafts *new* code. A **Large Language Model (LLM)** is a foundation model specialized for text. Under the hood it does something surprisingly simple: it predicts the **next token** (a word or word-piece) over and over. That's why it's so fluent — and also why it can **hallucinate**: it outputs the statistically likely next words, not verified facts.

A few terms the exam leans on. A **token** is a chunk of text ("cloud" might be one token; pricing and limits are counted in tokens). The **context window** is how much text the model can "see" at once. **Embeddings** turn text into vectors of numbers so similar meanings sit close together — the engine behind semantic search and RAG. And **parameters** are the model's learned "knobs"; billions of them give the model its nuance. To adapt a foundation model, start cheap and climb only as needed: prompt engineering → RAG → fine-tuning → continued pre-training.

---

## What is Generative AI?

Generative AI is a type of deep learning that **creates new content** — text, images, audio, video, or code — based on patterns learned from massive datasets. Unlike traditional (discriminative) ML that **classifies or predicts labels**, generative AI **produces novel output**.

| | Discriminative AI | Generative AI |
|-|-------------------|---------------|
| **Goal** | Distinguish/label existing data | Generate new data |
| **Question answered** | "Is this spam?" | "Write me an email." |
| **Example** | Fraud detection, sentiment analysis | Text/image generation, chatbots |

---

## Foundation Models (FMs)

A **foundation model** is a large ML model **pre-trained on vast, broad datasets** that can be adapted to a wide range of downstream tasks.

**Key characteristics:**
- Trained on huge, diverse, mostly unlabeled data (self-supervised)
- **General-purpose** — one model serves many tasks
- Adaptable via prompting, RAG, or fine-tuning
- Very expensive to train from scratch (millions of dollars, huge compute)

| FM Category | Produces | Examples |
|-------------|----------|----------|
| **Large Language Models (LLMs)** | Text | Claude, Amazon Nova, Llama, Mistral |
| **Diffusion / image models** | Images | Stable Diffusion, Amazon Nova Canvas |
| **Multimodal models** | Text + image/audio/video | Amazon Nova, Claude (vision) |
| **Embedding models** | Vectors | Amazon Titan Embeddings, Cohere Embed |

> On AWS, foundation models are accessed through **Amazon Bedrock** (managed FMs) — see [topic-01-amazon-bedrock.md](topic-01-amazon-bedrock.md).

### Exam Traps: What Is (and Isn't) a Foundation Model

| Trap | Reality |
|------|---------|
| "A model fine-tuned on medical records is a foundation model." | **No** — once adapted to a narrow task it is a **fine-tuned/customized model**, not the general-purpose FM it started from. |
| "An FM was trained on 1,000 labeled support chats." | **No** — FMs are **pre-trained on massive *unlabeled* data using self-supervised learning**. Labeled data is used later for fine-tuning. |
| "Use SageMaker to get a managed API for pre-built FMs." | **No** — **Amazon Bedrock** provides managed FM APIs. SageMaker is for building/training custom models. |
| "An LLM that returns a link to an image is an image-generation model." | **No** — that is **text output** from an LLM. Judge a model by what it actually **produces**. |

> **Parameters** are the model's learned weights ("knobs and dials") — more parameters generally allow more complex, nuanced behavior. Do not confuse **parameters** (learned during training) with **inference parameters** like temperature (set at request time).

---

## Large Language Models (LLMs)

An **LLM** is a foundation model trained on enormous text corpora to understand and generate human language.

**How LLMs work (simplified):**
1. Text is broken into **tokens**
2. Tokens are converted to **embeddings** (numeric vectors)
3. A **transformer** neural network processes them using **attention**
4. The model predicts the **next token**, one at a time (autoregressive generation)

> LLMs are fundamentally **next-token predictors** — they generate the statistically most likely continuation, which is why they can **hallucinate** (produce fluent but false content).

---

## The Transformer Architecture

The **transformer** (introduced in the 2017 paper *"Attention Is All You Need"*) is the neural network architecture behind modern LLMs.

| Concept | Description |
|---------|-------------|
| **Self-attention** | Lets the model weigh the relevance of every token to every other token, capturing context and long-range dependencies |
| **Parallelization** | Processes all tokens simultaneously (unlike older sequential RNNs) → faster training on large data |
| **Encoder** | Understands/encodes input (used in embedding/classification models like BERT) |
| **Decoder** | Generates output tokens (used in generative models like GPT/Claude) |

> **Exam tip:** The **attention mechanism** is the key innovation that lets transformers understand context far better than earlier architectures (RNNs, LSTMs).

---

## Tokens

A **token** is the basic unit of text an LLM processes — roughly a word, sub-word, or character sequence.

| Fact | Detail |
|------|--------|
| **Rough conversion** | ~1 token ≈ 4 characters ≈ 0.75 words in English |
| **Why it matters** | Pricing on Bedrock is **per input + output token** |
| **Tokenization** | Breaking text into tokens before processing |

> More tokens = higher cost and latency. Concise prompts and outputs reduce spend.

---

## Context Window

The **context window** is the maximum number of tokens (input + output) a model can consider at once.

| Aspect | Detail |
|--------|--------|
| **Includes** | System prompt + conversation history + user input + generated output |
| **Larger window** | Can handle long documents, more history — but higher cost/latency |
| **Exceeding it** | Older context is dropped/truncated ("forgetting") |

---

## Embeddings

An **embedding** is a numerical vector representation of data (text, images) that captures **semantic meaning**. Items with similar meaning have vectors that are close together in vector space.

| Concept | Description |
|---------|-------------|
| **Vector** | An array of numbers representing meaning |
| **Semantic similarity** | Measured by distance/angle (e.g., cosine similarity) between vectors |
| **Vector database** | Stores embeddings for fast similarity search (OpenSearch, Aurora pgvector, etc.) |
| **Use cases** | Semantic search, RAG retrieval, recommendations, clustering, classification |

> **Exam tip:** Embeddings are the foundation of **RAG** — user queries and documents are embedded, then similarity search retrieves relevant chunks. See [topic-03-rag-vs-finetuning.md](topic-03-rag-vs-finetuning.md).

**Example:** "king" − "man" + "woman" ≈ "queen" — embeddings capture semantic relationships numerically.

---

## Model Adaptation Approaches (Increasing Cost/Effort)

| Approach | Changes Weights? | Effort | When to Use |
|----------|-----------------|--------|-------------|
| **Prompt engineering** | No | Low | Steer behavior via instructions/examples |
| **RAG** | No | Medium | Inject private/current data at inference time |
| **Fine-tuning** | Yes | High | Change style, domain expertise, format |
| **Continued pre-training** | Yes | Very High | Deep domain adaptation on large unlabeled corpora |
| **Train from scratch** | Yes | Extreme | Rarely justified; only for unique needs |

> **Rule of thumb (exam):** Start with the **cheapest** approach that solves the problem. Prompt engineering → RAG → fine-tuning.

---

## In-Context Learning

LLMs can learn a task **from examples in the prompt**, without weight changes.

| Technique | Description |
|-----------|-------------|
| **Zero-shot** | Task instruction only, no examples |
| **One-shot** | One example provided |
| **Few-shot** | Multiple examples provided |

See [topic-02-prompt-engineering.md](topic-02-prompt-engineering.md) for details.

---

## Generative AI Capabilities and Use Cases

| Capability | Example Use Case |
|------------|------------------|
| **Text generation** | Marketing copy, emails, articles |
| **Summarization** | Condense documents, meetings, chats |
| **Question answering** | Chatbots, enterprise search |
| **Code generation** | Autocomplete, code review (Amazon Q Developer) |
| **Translation** | Multilingual content |
| **Image generation** | Product design, marketing visuals |
| **Chatbots / virtual assistants** | Customer support, internal help |
| **Content personalization** | Tailored recommendations |
| **Data augmentation** | Generate synthetic training data |
| **Agents** | Multi-step task automation with tools |

---

## Limitations and Risks of Generative AI

| Risk | Description | Mitigation |
|------|-------------|------------|
| **Hallucination** | Confident but false/fabricated output | RAG, grounding, human review, Guardrails |
| **Knowledge cutoff** | Doesn't know events after training | RAG for current data |
| **Bias / toxicity** | Reflects biases in training data | Guardrails, responsible AI practices |
| **Non-determinism** | Same prompt can give different outputs | Lower temperature for consistency |
| **Prompt injection** | Malicious input manipulates the model | Input validation, Guardrails |
| **Data privacy** | Sensitive data exposure | PII filters, encryption, no training on your data (Bedrock) |
| **Interpretability** | Hard to explain why output was produced | Documentation, evaluation, AI Service Cards |
| **Cost** | Token-based costs scale with usage | Prompt efficiency, right-size models |

---

## Inference Parameters (Controlling Output)

| Parameter | Effect | Higher Value → |
|-----------|--------|----------------|
| **Temperature** | Randomness/creativity | More diverse, less predictable output |
| **Top-p (nucleus)** | Cumulative probability cutoff for token choice | More varied word choice |
| **Top-k** | Limits choices to the k most likely tokens | Wider vocabulary considered |
| **Max tokens / length** | Caps output length | Longer responses (more cost) |
| **Stop sequences** | Strings that end generation | — |

> **Exam tip:** For **factual, deterministic** tasks use **low temperature**. For **creative** tasks use **higher temperature**.

---

## How AIF-C01 Actually Tests This

This domain is 24% of the exam. Questions test definitions with surgical precision — the traps live in the wording.

**Exam topics you must master:**

- **Generative vs. discriminative.** Creating new content (text, image, code) = generative. Labeling/classifying existing data (spam filter, fraud detection) = discriminative. Most real-world AI is still discriminative.
- **Foundation model definition.** A large model **pre-trained on broad, unlabeled data using self-supervised learning**, adaptable to many downstream tasks. An **LLM** is a foundation model for text.
- **Model output type identifies the model.** Text out = LLM; image out = image-generation model; text **and** image in/out = multi-modal. Judge by what the model actually *produces*.
- **Terminology:** token (unit of text, billing/limits), context window (max tokens seen at once), hallucination (confident but false output), parameters (learned weights).
- **Bedrock provides FMs; SageMaker builds custom models.** "Managed, serverless API for pre-built FMs" → Bedrock.

**Trap patterns to watch for:**

- **"A fine-tuned medical model is a foundation model"** → false. Once adapted to a narrow task it's a *fine-tuned/customized* model, not the general-purpose FM.
- **"An FM was trained on 1,000 labeled chats"** → false. FMs pre-train on **massive unlabeled** data; labeled data is used later for fine-tuning.
- **"An LLM that returns a link to an image is an image-generation model"** → false; that's text output from an LLM.
- **"Use SageMaker for a managed FM API"** → false; that's Bedrock's job.
- **Confusing parameters (learned weights) with inference parameters (temperature, top-p).**

---

## Common Misconceptions

- **Misconception:** Generative AI is conscious or "understands" like a human. **Reality:** It's a pattern-matcher predicting the next token from statistics. When it says "I'm happy," it's emitting likely text, not feeling anything.
- **Misconception:** A foundation model works perfectly out of the box for any task. **Reality:** It's a talented generalist that usually needs adaptation (prompting, RAG, or fine-tuning) to be reliable for a specific job; a raw model can give wrong or unsafe answers.
- **Misconception:** LLMs learn facts like a person reading a textbook. **Reality:** They learn statistical relationships between words. They have no live fact database, so knowledge freezes at training time — use RAG for current facts.
- **Misconception:** All AI is generative AI. **Reality:** Most production AI (spam filters, recommendations, fraud detection) is analytical/discriminative; generative AI is one sub-category that creates new content.
- **Misconception:** Bigger context window means the model "remembers" past conversations forever. **Reality:** The window is a per-request limit; anything outside it (or a prior stateless call) is not retained unless you resend it.

---

## Knowledge Test — Generative AI Fundamentals

**Q1.** What is the primary function that underlies how an LLM generates text?

- A) Classifying text into categories
- B) Predicting the next token in a sequence
- C) Clustering similar documents
- D) Compressing text into embeddings

**Q2.** A company needs their generative AI application to answer questions using up-to-date internal documents without retraining the model. Which approach is most appropriate?

- A) Fine-tuning
- B) Continued pre-training
- C) Retrieval-Augmented Generation (RAG)
- D) Training a model from scratch

**Q3.** Which neural network innovation allows transformers to understand the relationship between all words in a sequence?

- A) Convolution
- B) Recurrence
- C) Self-attention
- D) Pooling

**Q4.** An embedding model converts text into what?

- A) Tokens
- B) A numerical vector capturing semantic meaning
- C) A probability distribution over labels
- D) A knowledge graph

**Q5.** A developer wants the most **deterministic, factual** responses from an LLM. Which parameter setting helps?

- A) High temperature
- B) Low temperature
- C) High top-k
- D) Increased max tokens

**Q6.** What best describes a foundation model?

- A) A small model trained for a single narrow task
- B) A rules-based expert system
- C) A large model pre-trained on broad data, adaptable to many tasks
- D) A vector database used for retrieval

**Q7.** Why do LLMs sometimes produce confident but factually incorrect answers?

- A) They intentionally deceive users
- B) They predict statistically likely text and are not grounded in verified facts
- C) They lack a context window
- D) They only use supervised learning

**Q8.** A team fine-tunes a base model on their internal legal documents to create a contract-review assistant. Is the resulting model a foundation model?

- A) Yes — any large model is a foundation model
- B) No — it is a fine-tuned/customized model derived from a foundation model
- C) Yes — fine-tuning creates a new foundation model
- D) No — it is now a discriminative model

**Q9.** Which statement about how foundation models are pre-trained is correct?

- A) They are trained on small, carefully labeled datasets
- B) They are trained on massive unlabeled datasets using self-supervised learning
- C) They require reinforcement learning for all pre-training
- D) They are trained only on structured tabular data

**Q10.** A scenario asks which AWS service provides a fully managed, serverless API to access pre-built foundation models from multiple providers. Which is correct?

- A) Amazon SageMaker
- B) Amazon Comprehend
- C) Amazon Bedrock
- D) Amazon Kendra

---

**Answers:**
1. B — LLMs are autoregressive next-token predictors
2. C — RAG injects current external data at inference without retraining
3. C — Self-attention is the transformer's key mechanism
4. B — Embeddings are semantic numerical vectors
5. B — Lower temperature reduces randomness for factual consistency
6. C — Foundation models are broadly pre-trained and adaptable
7. B — Hallucination stems from probabilistic generation without factual grounding
8. B — Adapting an FM to a narrow task yields a fine-tuned model, not a new foundation model
9. B — FMs are pre-trained on vast unlabeled data via self-supervised learning
10. C — Amazon Bedrock is the managed, serverless FM API; SageMaker builds custom models
