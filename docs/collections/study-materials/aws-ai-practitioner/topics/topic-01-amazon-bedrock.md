# Topic 1: Amazon Bedrock

> Source: [AWS Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

---

## Quick Revision (TL;DR)

- **Bedrock** = fully managed, **serverless** access to 100+ foundation models via one **unified API**; no infrastructure to manage; pay per token.
- **Converse API** = recommended for **multi-turn**, model-agnostic chat.
- **Knowledge Bases** = managed **RAG** (retrieve private data + citations). **Agents** = FM that **takes actions** via tools/APIs. **Guardrails** = safety/PII/grounding filters.
- **Guardrail filters:** content (hate/violence/etc.), denied topics, word, **sensitive info (PII mask)**, **contextual grounding** (detect hallucination), **automated reasoning** (logical validation).
- **Data privacy:** your prompts/completions are **not** used to train base models and are **not** shared with providers.
- **Choose an FM by:** task type, context window, cost, latency, capability, modality.

---

## In Plain English

Picture a huge buffet line. Each steaming tray is a **foundation model** already cooked by a world-class chef — Anthropic (Claude), Meta (Llama), Amazon (Nova/Titan), Stability AI (images), and more. Instead of spending weeks cooking from scratch in your own kitchen (training a model), you grab a plate and point at the tray you want. You don't need to know how the chef simmered the broth; you just pick the tray that suits your appetite (your use case — chatbot, summarization, image generation). You pay only for what you scoop onto your plate, and the kitchen (AWS infrastructure) never runs out.

That buffet is **Amazon Bedrock**: one managed, serverless service that lets you discover, test, and call many pre-built models through a **single API**. Because the API is the same across providers, you can swap Claude for Llama without rewriting your app. AWS hosts the models, handles scaling and security, and — importantly — does **not** use your prompts or data to train the base models for anyone else.

You can also customize your portion. Want extra spice? **Fine-tune** the model on your own data to match your tone. Want the dish served with today's facts? Attach a **Knowledge Base** so the model retrieves your documents at answer time (**RAG**) instead of guessing. Want a bouncer checking every plate for anything unsafe? Add **Guardrails**. And before you commit, you can taste-test any model for free in the no-code **playground** inside the AWS Console.

---

## What is Amazon Bedrock?

Amazon Bedrock is a **fully managed service** that provides secure, enterprise-grade access to high-performing foundation models (FMs) from leading AI companies. It enables you to build and scale generative AI applications **without managing infrastructure**.

**Key characteristics:**
- No need to provision capacity or write infrastructure code
- Serverless — pay only for what you use
- Unified API across multiple model providers
- Enterprise security: AWS IAM, VPC, encryption at rest and in transit
- Supports **100+ foundation models** from: Amazon, Anthropic, Meta, Cohere, Mistral, DeepSeek, OpenAI, xAI, and more

---

## Supported API Styles

| API | Best For |
|-----|----------|
| **Converse API** | Multi-turn conversations; unified across models — recommended for new apps |
| **InvokeModel API** | Single-turn, model-specific payloads |
| **Messages API** | Anthropic-compatible interface |
| **Chat Completions API** | OpenAI-compatible interface |
| **Responses API** | OpenAI Responses format |

---

## Model Selection Criteria

When choosing a foundation model on Bedrock, evaluate:

| Factor | Considerations |
|--------|----------------|
| **Task type** | Text generation, summarization, coding, image understanding, embedding |
| **Context window** | Larger windows needed for long documents (e.g., Grok 4.6: 500K tokens) |
| **Cost** | Priced per input/output tokens; larger models cost more |
| **Latency** | Smaller models return responses faster |
| **Capability** | Benchmark scores (MMLU, HumanEval, etc.) |
| **Modality** | Text-only vs. multimodal (images, audio, video) |
| **Reasoning** | Models with configurable reasoning effort (low/medium/high) for complex tasks |

> **Exam tip:** Amazon Nova models are AWS-native and optimized for cost/performance trade-offs. Anthropic Claude models excel at reasoning and instruction-following. Match the model to the use case, not just capability alone.

---

## Amazon Bedrock Knowledge Bases

> Source: [AWS Knowledge Bases docs](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)

Knowledge Bases enable **Retrieval-Augmented Generation (RAG)** — injecting your private data into model responses at query time.

### Two Types of Knowledge Bases

| Type | Description |
|------|-------------|
| **Managed Knowledge Base** | AWS manages ingestion, indexing, storage, and retrieval. Supports multi-modal data, auto-scaling, agentic retrieval (multi-hop). **Recommended for most users.** |
| **Customer-managed Knowledge Base** | You control vector store (OpenSearch Serverless, Aurora, Neptune), parsing, and indexing. Full control but more operational overhead. |

### How It Works

```
User Query
	↓
Convert query to embedding vector
	↓
Similarity search in vector store
	↓
Retrieve top-K relevant document chunks
	↓
Inject chunks into FM prompt as context
	↓
FM generates grounded response with citations
```

### Key Features

| Feature | Description |
|---------|-------------|
| **Data connectors** | Amazon S3, SharePoint, Confluence, Google Drive, OneDrive, Web Crawler |
| **Smart Parsing** | Auto-selects parsing strategy per document type (PDF, DOCX, PPTX, embedded visuals, audio) |
| **Multimodal retrieval** | Images can be extracted and used in retrieval |
| **Reranking models** | Improve relevance of retrieved results |
| **Access Control Lists (ACLs)** | Document-level permissions at retrieval time (Managed KB only, except Web Crawler) |
| **Citations** | Responses include references to source documents |
| **AgentCore Gateway integration** | MCP-compatible agent frameworks can invoke Knowledge Base as a tool without custom code |

---

## Amazon Bedrock Agents

> Source: [AWS Agents docs](https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html)

Agents enable foundation models to **take actions** — not just generate text. They orchestrate multi-step tasks using tools, APIs, and data sources.

### What Agents Can Do

- Break down complex user requests into smaller, executable steps
- Collect additional information via natural conversation
- Make **API calls** to company systems (action groups)
- Query knowledge bases to supplement responses
- Maintain conversational context across turns

### Agent Components

| Component | Role |
|-----------|------|
| **Foundation model** | The reasoning engine driving orchestration |
| **Action groups** | Defined APIs the agent can call (via OpenAPI schema or AWS Lambda) |
| **Knowledge bases** | Data sources the agent can search |
| **Prompt templates** | Customizable instructions for each orchestration step |

### Prompt Template Stages

| Stage | Purpose |
|-------|---------|
| **Pre-processing** | Validate and classify user input |
| **Orchestration** | Plan and execute multi-step tasks |
| **KB response generation** | Format knowledge base results |
| **Post-processing** | Format final response to user |

### Agent Lifecycle

1. Create a knowledge base (optional)
2. Configure agent + add action groups + associate knowledge bases
3. Customize prompt templates for each stage
4. Test with **traces** to inspect step-by-step reasoning
5. Create an alias → deploy to application
6. Application calls agent via alias ID

> **Note:** Amazon Bedrock Agents Classic is in maintenance mode. New customers should use **Amazon Bedrock AgentCore**.

---

## Amazon Bedrock Guardrails

> Source: [AWS Guardrails docs](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)

Guardrails provide **configurable safeguards** to detect and filter undesirable content consistently across all foundation models on Bedrock.

### Filter Types

| Filter | What It Does |
|--------|-------------|
| **Content filters** | Detects and filters harmful text or image content. Categories: Hate, Insults, Sexual, Violence, Misconduct, Prompt Attack. Configurable strength per category. |
| **Denied topics** | Blocks topic areas you define (e.g., "investment advice", "competitor products") |
| **Word filters** | Blocks exact words/phrases — includes a profanity list + custom words |
| **Sensitive information filters** | Blocks or **masks** PII (SSN, DOB, address) and custom regex patterns |
| **Contextual grounding checks** | Detects **hallucinations** — flags responses not grounded in source or irrelevant to query |
| **Automated Reasoning checks** | Validates FM responses against logical rules; suggests corrections, highlights assumptions |

### Usage Patterns

```
# Use with FM inference
response = bedrock.invoke_model(
	modelId="...",
	guardrailIdentifier="my-guardrail-id",
	guardrailVersion="1",
	...
)

# Use without invoking a model (evaluate content only)
response = bedrock.apply_guardrail(
	guardrailIdentifier="my-guardrail-id",
	guardrailVersion="1",
	source="INPUT",
	content=[{"text": {"text": user_input}}]
)
```

### Guardrail Safeguard Tiers

| Tier | Description |
|------|-------------|
| **Classic** | Standard content filtering across all defined categories |
| **Standard** | Extended protection — detects harmful content within code elements (comments, variable names, string literals) |

### Key Use Cases

| Use Case | Relevant Filters |
|----------|-----------------|
| Customer chatbot | Content filters, denied topics |
| Banking application | Denied topics (illegal financial advice) |
| Call center summarization | Sensitive information filters (mask PII) |
| RAG application | Contextual grounding checks (detect hallucinations) |
| Code generation | Standard tier content filters for code |

---

## How AIF-C01 Actually Tests This

Bedrock is one of the most heavily tested services. The exam checks whether you understand what Bedrock *is*, how you *customize* models, and the *security* guarantees.

**Exam topics you must master:**

- **Bedrock = managed access to FMs from *multiple* providers**, not just Amazon's own models. Remember the roster: Anthropic (Claude), Meta (Llama), Amazon (Nova/Titan), Stability AI (images), Cohere, Mistral, and more.
- **Inference vs. customization.** *Inference* = send a prompt, get a response from a pre-trained model. *Customization* = **fine-tuning** (retrain weights on your data) or **RAG via Knowledge Bases** (retrieve your documents at query time, no retraining).
- **"Use my documents without retraining" → RAG (Knowledge Bases).** Source documents live in **Amazon S3**.
- **Data privacy facts:** encrypted in transit and at rest; your prompts/completions are **not** used to train the base models or shared with providers; access is controlled with **IAM**.
- **Guardrails** provide model-independent safety: content filters, denied topics, word filters, PII masking, contextual grounding (hallucination detection), automated reasoning.
- **Playground** = no-code console interface to test/compare models before building.
- **Converse API** = recommended unified, multi-turn interface across models.

**Trap patterns to watch for:**

- **"Bedrock only has Amazon Titan/Nova models"** → false; it's multi-provider.
- **Fine-tuning vs. RAG confusion:** if the need is *current or private facts*, the answer is **RAG**; if it's *tone/style/format*, the answer is **fine-tuning**.
- **"Your data trains the shared model"** → false; your data stays private to your account.
- **"You must build/train a model to use Bedrock"** → false; models are pre-built.
- **Bedrock vs. SageMaker:** Bedrock = consume/customize ready-made FMs with minimal ML skill; SageMaker = build/train/host custom models with full control.

---

## Common Misconceptions

- **Misconception:** Amazon Bedrock only offers Amazon's own models. **Reality:** It's a multi-provider catalog — Anthropic, Meta, Amazon, Stability AI, Cohere, Mistral, and others — all behind one API. The name makes it *sound* Amazon-only.
- **Misconception:** Using Bedrock means training a model from scratch for each use case. **Reality:** The models are pre-trained; you use them as-is or lightly customize with fine-tuning or RAG. Training from scratch is a SageMaker job, and rarely needed.
- **Misconception:** Fine-tuning and RAG are the same. **Reality:** Fine-tuning changes the model's weights using labeled examples; RAG leaves the model unchanged and injects retrieved documents into the prompt. RAG is the right tool for fresh or private facts.
- **Misconception:** Your confidential prompts are used to improve the shared model. **Reality:** AWS states your data is encrypted and not used to train the base models for other customers.
- **Misconception:** You must write code just to try a model. **Reality:** The Bedrock **playground** lets you type prompts and compare models with no code.

---

## Knowledge Test — Amazon Bedrock

**Q1.** A company wants to build a chatbot that answers questions using their internal HR policy documents stored in S3. Which Amazon Bedrock feature should they use?

- A) Fine-tuning
- B) Guardrails
- C) Knowledge Bases
- D) Action Groups

**Q2.** A developer needs to prevent a customer-facing Bedrock application from discussing competitor products. Which Guardrails filter is most appropriate?

- A) Content filters
- B) Sensitive information filters
- C) Denied topics
- D) Contextual grounding checks

**Q3.** An agent-powered application is returning responses that contradict the retrieved source documents. Which Guardrails feature addresses this?

- A) Word filters
- B) Denied topics
- C) Automated Reasoning checks
- D) Contextual grounding checks

**Q4.** Which Bedrock API is recommended for multi-turn conversations and is unified across different model providers?

- A) InvokeModel API
- B) Converse API
- C) Messages API
- D) Chat Completions API

**Q5.** What is the primary difference between a Managed Knowledge Base and a Customer-managed Knowledge Base?

- A) Managed supports only S3; Customer-managed supports all connectors
- B) Managed uses AWS-controlled vector stores and infrastructure; Customer-managed gives you full control over vector store and ingestion
- C) Customer-managed supports citations; Managed does not
- D) They are functionally identical but differ only in pricing

**Q6.** A call center application uses Bedrock to summarize conversations. The company must ensure user SSNs and dates of birth never appear in summaries. Which Guardrails filter handles this?

- A) Denied topics
- B) Contextual grounding checks
- C) Sensitive information filters
- D) Word filters

**Q7.** Which component of a Bedrock Agent defines the external APIs the agent is permitted to call?

- A) Knowledge base
- B) Prompt template
- C) Action group
- D) Alias

---

**Answers:**
1. C — Knowledge Bases implement the RAG pipeline against S3 documents
2. C — Denied topics blocks defined subject areas
3. D — Contextual grounding checks detect responses not grounded in retrieved source
4. B — Converse API is unified and designed for multi-turn conversations
5. B — Managed KB abstracts all infrastructure; Customer-managed gives vector store control
6. C — Sensitive information filters block/mask PII entities
7. C — Action groups define the APIs an agent can invoke
