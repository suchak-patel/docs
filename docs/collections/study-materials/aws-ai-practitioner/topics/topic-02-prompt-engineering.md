# Topic 2: Prompt Engineering

> Source: [AWS Prompt Engineering Concepts](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-engineering-guidelines.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

---

## Quick Revision (TL;DR)

- **Prompt components:** instruction, context, input data, output indicator.
- **Zero-shot** = no examples · **few-shot** = example input/output pairs · **chain-of-thought** = "think step by step" for reasoning.
- **Temperature:** low (0–0.2) = deterministic/factual; high (0.7–1.0) = creative. Top-P / Top-K also shape randomness.
- **Templates** standardize and reuse prompts with `{{placeholders}}`.
- Models are **stateless** — resend conversation history each call (or use the Converse API).
- **Reduce hallucinations:** grounding instructions, RAG, Guardrails, lower temperature.
- **System prompt** sets role/behavior; **user prompt** is the request.

---

## What is Prompt Engineering?

Prompt engineering is the practice of **optimizing textual input** to a Large Language Model (LLM) to obtain desired responses. It helps LLMs perform a wide variety of tasks including classification, question answering, code generation, creative writing, summarization, and more.

> The quality of prompts you provide directly impacts the quality of model responses.

---

## Components of a Prompt

A well-designed prompt includes one or more of the following components.

| Component | Description | Example |
|-----------|-------------|---------|
| **Instruction** | What you want the model to do | "Summarize the following in one sentence." |
| **Context** | Background information guiding interpretation | "The following is a restaurant review:" |
| **Input data** | The content the model should act on | The review text itself |
| **Output indicator** | Desired format or structure | `Answer:`, `Output (JSON):` |

### Example prompt with all four components

```
The following is text from a restaurant review:     <- Context

"I finally got to check out Alessandro's Brilliant Pizza..."  <- Input data

Summarize the above restaurant review in one sentence.  <- Instruction
														 <- (Output format implied)
```

---

## Zero-Shot Prompting

Provide the task **without any examples**. Relies entirely on the model's pre-trained knowledge.

```
Tell me the sentiment of the following headline and categorize it
as either positive, negative or neutral:
"New airline between Seattle and San Francisco offers a great
opportunity for both passengers and investors."

Output: Positive
```

**When to use:**
- Simple, well-understood tasks where the model performs adequately without examples
- Rapid prototyping and exploration

**Limitation:** May produce inconsistent output formatting or underperform on nuanced tasks.

---

## Few-Shot Prompting (In-Context Learning)

Provide **input-output example pairs** to demonstrate the expected behavior. A *shot* = one example pair.

```
Tell me the sentiment of the following headline.

Research firm fends off allegations of impropriety.
Answer: Negative

Offshore windfarms continue to thrive as vocal minority dwindles.
Answer: Positive

Manufacturing plant is the latest target in investigation by state officials.
Answer:

Output: Negative
```

**When to use:**
- Zero-shot performance is insufficient
- Consistent output formatting is required
- The task has nuanced classification criteria not easily described in words

**Best practices for Anthropic Claude:**
- Wrap examples in `<example></example>` XML tags
- Use `H:` / `A:` inside examples — do NOT use `Human:` / `Assistant:` (reserved for the outer prompt)
- Leave the final `A:` off so Claude generates the answer

```
Human: Classify the email as "Personal" or "Commercial".

<example>
H: Hi Tom, we plan to have a party at my house this weekend. Can you come?
A: Personal
</example>

<example>
H: Hi Tom, special offer — save 35% if you book in the next two days!
A: Commercial
</example>

H: Hi Tom, we just launched new products. Order now and save $100!

A:
```

---

## Chain-of-Thought (CoT) Prompting

Ask the model to **show its reasoning step-by-step** before giving the final answer. Dramatically improves performance on multi-step reasoning tasks.

**Few-shot CoT** — provide reasoning examples:

```
Q: Roger has 5 tennis balls. He buys 2 cans of 3 balls each.
How many tennis balls does he have now?
A: Roger started with 5 balls. 2 cans x 3 balls = 6 new balls. 5 + 6 = 11.
The answer is 11.

Q: The cafeteria had 23 apples. Used 20 for lunch, bought 6 more.
How many apples do they have?
A: Let's think step by step.
```

**Zero-shot CoT** — add a single trigger phrase without examples:

```
Q: If a store sells 12 items per hour and is open 8 hours, how many
items are sold? Answer step by step.
```

**When to use:**
- Multi-step arithmetic or logical reasoning
- Planning and problem decomposition
- Code debugging
- Tasks where intermediate steps matter

---

## Prompt Templates

A prompt template is a **reusable recipe** with `{{placeholders}}` for variable content.

```
Tell me the sentiment of the following {{Text Type, e.g. "restaurant review"}}
and categorize it as either {{Sentiment A}} or {{Sentiment B}}.

Here are some examples:

Text: {{Example Input 1}}
Answer: {{Sentiment A}}

Text: {{Example Input 2}}
Answer: {{Sentiment B}}

Text: {{Input}}
Answer:
```

**Benefits:**
- Standardizes prompts across your application
- Reduces inconsistency in outputs
- Enables reuse across multiple inputs without rewriting the prompt

---

## Inference Parameters

These parameters control how the model generates output tokens.

| Parameter | Effect | Typical Range |
|-----------|--------|---------------|
| **Temperature** | Controls randomness. Higher = more creative/diverse. Lower = more deterministic/consistent. | 0.0 – 1.0 |
| **Top-P (nucleus sampling)** | Considers only tokens whose cumulative probability reaches P. Lower = less random. | 0.0 – 1.0 |
| **Top-K** | Considers only the K most probable next tokens at each step. | 1 – 500+ |
| **Max tokens** | Maximum number of tokens in the output | Use-case dependent |
| **Stop sequences** | Strings that halt generation (e.g., `"\n\nHuman:"`) | — |

### Guidance by Task Type

| Task | Temperature | Top-P |
|------|-------------|-------|
| Code generation | 0.0 – 0.2 | 0.9 |
| Classification | 0.0 – 0.2 | — |
| Summarization | 0.2 – 0.4 | — |
| Conversational chat | 0.5 – 0.7 | — |
| Creative writing | 0.7 – 1.0 | 0.95+ |

---

## Hallucination Mitigation via Prompting

Per AWS documentation, strategies to reduce hallucinations:

| Strategy | How It Helps |
|----------|-------------|
| **Refine the prompt** | Add grounding instructions: "Only answer based on the provided context." |
| **RAG** | Inject factual source documents so the model has verified ground truth |
| **Bedrock Guardrails** | Contextual grounding checks detect when responses deviate from source |
| **Lower temperature** | Reduce randomness in generation |
| **Different model** | Switch to a model with stronger factual grounding on your task |

---

## Conversation Memory

Models accessed via API do **not** retain memory across API calls.

**To maintain conversational context:**
- Include prior conversation history in the current request payload
- Use the Converse API which handles message history natively
- For Anthropic Claude (legacy API): wrap in `\n\nHuman: ... \n\nAssistant:` format

```python
# Converse API maintains history via messages array
response = bedrock.converse(
	modelId="anthropic.claude-opus-4-7",
	messages=[
		{"role": "user",      "content": [{"text": "What is RAG?"}]},
		{"role": "assistant", "content": [{"text": "RAG stands for..."}]},
		{"role": "user",      "content": [{"text": "How does it differ from fine-tuning?"}]},
	]
)
```

---

## Prompt Optimization Tips (AWS Best Practices)

1. Be **specific and explicit** — vague instructions produce vague output
2. Assign a **role**: "You are an expert financial analyst..."
3. Use **delimiters** to separate input from instructions (`"""`, `<text></text>`, `---`)
4. Specify **output format** explicitly (JSON, bullet list, table, single sentence)
5. Use **negative constraints**: "Do not include personal opinions."
6. **Iterate** — test multiple phrasings and compare outputs
7. For long documents, put instructions **after** the document (recency bias)

---

## Knowledge Test — Prompt Engineering

**Q1.** A developer adds three labeled examples to a prompt before asking the model to classify a new input. This technique is called:

- A) Zero-shot prompting
- B) Chain-of-thought prompting
- C) Few-shot prompting
- D) System prompting

**Q2.** Which prompt addition would most improve a model's performance on a multi-step math problem?

- A) Increasing temperature to 0.9
- B) Adding "Let's think step by step."
- C) Reducing max_tokens to force brevity
- D) Adding more stop sequences

**Q3.** A chatbot built on Amazon Bedrock via direct API calls is forgetting what the user said earlier in the conversation. What is the correct solution?

- A) Enable model fine-tuning
- B) Create a Knowledge Base
- C) Include prior conversation turns in each new API request
- D) Increase the temperature parameter

**Q4.** For a classification task requiring consistent, deterministic output format, which temperature setting is most appropriate?

- A) 0.9
- B) 0.7
- C) 0.5
- D) 0.1

**Q5.** A prompt template contains `{{Customer Name}}` and `{{Product Type}}` placeholders. What is the primary benefit of this approach?

- A) It reduces token count automatically
- B) It standardizes prompt structure and enables reuse across inputs
- C) It enables the model to learn from examples permanently
- D) It bypasses the need for inference parameters

**Q6.** A model is generating overly verbose, wandering creative stories. Which parameter change is most likely to improve focus and consistency?

- A) Increase Top-K
- B) Decrease temperature
- C) Increase max_tokens
- D) Increase Top-P

**Q7.** When using Anthropic Claude's few-shot prompting, which format is recommended for the examples inside the prompt?

- A) Use `Human:` and `Assistant:` labels inside each example
- B) Use `<example>` tags with `H:` and `A:` delimiters inside
- C) Place examples after the final question, not before it
- D) Use JSON format for all examples

---

**Answers:**
1. C — Few-shot prompting provides input-output pairs as demonstrations
2. B — Zero-shot CoT trigger ("Let's think step by step") activates reasoning
3. C — Models are stateless; conversation history must be included in each request
4. D — Low temperature (0.0–0.2) produces deterministic, consistent output
5. B — Templates standardize structure and enable reuse with different input values
6. B — Lower temperature reduces randomness and produces more focused output
7. B — AWS docs recommend `<example>` tags with `H:`/`A:` to avoid delimiter confusion
