# Topic 1: Amazon Bedrock

> Source: [AWS Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

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
