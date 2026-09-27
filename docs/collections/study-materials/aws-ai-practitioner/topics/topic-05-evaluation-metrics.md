# Topic 5: Foundation Model Evaluation Metrics

> Sources: [Amazon Bedrock Model Evaluation](https://docs.aws.amazon.com/bedrock/latest/userguide/model-evaluation.html) | ROUGE (Lin, 2004) | BLEU (Papineni et al., 2002) | BERTScore (Zhang et al., 2019)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

---

## Why Evaluation Metrics Matter

Foundation models must be evaluated **objectively and systematically** before deployment. Different metrics suit different task types — choosing the wrong metric gives a misleading picture of model quality.

> **Exam tip:** Know which metric maps to which task type, and understand the key trade-off: lexical metrics (ROUGE, BLEU) are fast and reproducible but miss semantic equivalence; semantic metrics (BERTScore) are more accurate but more expensive.

---

## ROUGE

**Full name:** Recall-Oriented Understudy for Gisting Evaluation

**Primary use case:** Evaluating **text summarization** quality

### Core Idea

Compare **n-gram overlap** between the generated text (hypothesis) and one or more reference texts. ROUGE is **recall-oriented** — it measures how much of the reference appears in the generated output.

### ROUGE Variants

| Variant | Measures | Best For |
|---------|---------|---------|
| **ROUGE-1** | Unigram (single word) overlap | Vocabulary coverage |
| **ROUGE-2** | Bigram (two-word sequence) overlap | Phrase-level similarity |
| **ROUGE-L** | Longest Common Subsequence (LCS) | Sentence structure similarity |
| **ROUGE-S** | Skip-bigram overlap (non-consecutive word pairs) | Flexible phrase matching |

### ROUGE-N Formula

$$\text{ROUGE-N} = \frac{\sum_{s \in \text{References}} \sum_{\text{gram}_n \in s} \text{Count}_\text{match}(\text{gram}_n)}{\sum_{s \in \text{References}} \sum_{\text{gram}_n \in s} \text{Count}(\text{gram}_n)}$$

Simplified:

$$\text{ROUGE-N Recall} = \frac{\text{Matching N-grams between hypothesis and reference}}{\text{Total N-grams in reference}}$$

### ROUGE Score Interpretation

| ROUGE-1 Score | Quality Indication |
|---------------|--------------------|
| < 0.2 | Poor |
| 0.2 – 0.3 | Fair |
| 0.3 – 0.4 | Good |
| > 0.4 | Strong |

### ROUGE Limitations

- **Purely lexical** — does not capture semantic meaning
- "The cat sat on the mat" vs. "The feline rested on the rug" scores near zero despite identical meaning
- Rewards verbatim copying over quality paraphrasing
- Multiple valid reference summaries needed for fair evaluation

---

## BLEU

**Full name:** Bilingual Evaluation Understudy

**Primary use case:** Evaluating **machine translation** quality; also used for code generation and other generation tasks

### Core Idea

Measures **precision** of n-gram overlap between generated text and references, with a **brevity penalty** that discourages overly short outputs. BLEU is **precision-oriented**.

### BLEU Formula

$$\text{BLEU} = \text{BP} \cdot \exp\left(\sum_{n=1}^{N} w_n \log p_n\right)$$

**Where:**
- $\text{BP}$ = Brevity Penalty: penalizes outputs shorter than the reference
	- $\text{BP} = 1$ if candidate length $\geq$ reference length
	- $\text{BP} = e^{1 - r/c}$ if candidate length $c <$ reference length $r$
- $p_n$ = Modified precision for n-grams (clipped to prevent gaming by repeating words)
- $w_n$ = Uniform weights ($1/N$, typically $N=4$)

### BLEU Score Interpretation

| BLEU Score | Quality Indication |
|------------|--------------------|
| < 0.1 | Almost useless |
| 0.1 – 0.2 | Hard to get the gist |
| 0.2 – 0.3 | Understandable |
| 0.3 – 0.4 | Clear, with some mistakes |
| 0.4 – 0.6 | High quality translation |
| > 0.6 | Approaching human quality |

### ROUGE vs. BLEU Comparison

| | ROUGE | BLEU |
|--|-------|------|
| **Orientation** | Recall-focused | Precision-focused |
| **Primary use** | Summarization | Translation |
| **Brevity penalty** | No | Yes |
| **Multiple references** | Recommended | Supported |
| **N-gram range** | 1, 2, L, S | 1–4 |

### BLEU Limitations

- Same semantic blindness as ROUGE
- Single reference unfairly penalizes valid alternative phrasings
- Does not correlate well with human judgment at the sentence level
- Brevity penalty can be gamed with slightly longer but poor translations

---

## BERTScore

**Full name:** BERT-based text generation evaluation metric

**Primary use case:** Semantic similarity — works where ROUGE/BLEU fail due to paraphrasing

### Core Idea

Uses **pre-trained contextual embeddings** (BERT or similar) to compare semantic similarity between generated and reference tokens, rather than exact string matching.

### How BERTScore Works

```
Generated text: "The automobile was parked on the road."
Reference text: "The car was sitting on the street."

1. Encode both texts using BERT → contextual embedding vectors
2. Compute cosine similarity between all generated × reference token pairs
3. Greedy match: each generated token → most similar reference token
4. Compute Precision, Recall, F1 from matched similarity scores
```

### BERTScore Formula

$$P_{\text{BERT}} = \frac{1}{|\hat{x}|} \sum_{\hat{x}_j \in \hat{x}} \max_{x_i \in x} \cos(\mathbf{e}_{\hat{x}_j}, \mathbf{e}_{x_i})$$

$$R_{\text{BERT}} = \frac{1}{|x|} \sum_{x_i \in x} \max_{\hat{x}_j \in \hat{x}} \cos(\mathbf{e}_{x_i}, \mathbf{e}_{\hat{x}_j})$$

$$F_{\text{BERT}} = \frac{2 \cdot P_{\text{BERT}} \cdot R_{\text{BERT}}}{P_{\text{BERT}} + R_{\text{BERT}}}$$

### BERTScore Advantages

- Captures **semantic equivalence**: "automobile" ≈ "car" ≈ "vehicle"
- Works well across paraphrases and synonyms
- Correlates significantly better with **human judgment** than ROUGE or BLEU
- Handles diverse linguistic expressions naturally

### BERTScore Limitations

- **Computationally more expensive** — requires BERT inference for every evaluation
- Requires a pre-trained encoder model
- Scores less interpretable compared to ROUGE integers
- Score range depends on the underlying BERT model

---

## Other Key Evaluation Metrics

| Metric | Use Case | Description |
|--------|----------|-------------|
| **Perplexity** | Language model quality | How well the model predicts a test sample. Lower = better. |
| **F1 Score** | Classification, QA | Harmonic mean of Precision and Recall |
| **Exact Match (EM)** | Question answering | Percentage of predictions that exactly match the reference |
| **MMLU** | General reasoning | Multiple-choice benchmark across 57 academic subjects |
| **HumanEval** | Code generation | Measures pass rate on programming problems |
| **Pass@k** | Code generation | Probability that at least one of k generated solutions passes all tests |
| **TruthfulQA** | Truthfulness | Measures tendency to generate truthful responses |
| **HellaSwag** | Commonsense reasoning | Sentence completion benchmark |

---

## Human Evaluation Dimensions

Automated metrics have fundamental limits. Human evaluation remains the gold standard for:

| Dimension | What Evaluators Assess |
|-----------|----------------------|
| **Coherence** | Does the response flow logically and make sense? |
| **Fluency** | Is it grammatically correct and natural-sounding? |
| **Relevance** | Does it actually answer the question asked? |
| **Faithfulness** | Is it grounded in provided context without hallucinating? |
| **Helpfulness** | Would a real user find this response useful? |
| **Safety** | Does it avoid harmful, biased, or inappropriate content? |

AWS Bedrock Model Evaluation supports both **automated metric scoring** and managed **human evaluation** workflows.

---

## Metric Selection Guide

| Task | Primary Metric | Secondary Metric |
|------|---------------|-----------------|
| **Text summarization** | ROUGE-L | BERTScore F1 |
| **Machine translation** | BLEU | BERTScore |
| **Question answering** | Exact Match (EM), F1 | BERTScore |
| **Text classification** | F1, Accuracy | — |
| **Code generation** | Pass@k, HumanEval | — |
| **Semantic similarity** | BERTScore | ROUGE-L |
| **General knowledge** | MMLU | — |
| **Dialogue / chat** | Human evaluation | BERTScore |
| **Factual accuracy** | TruthfulQA | Human evaluation |

---

## Amazon Bedrock Model Evaluation

Amazon Bedrock provides a built-in **Model Evaluation** feature that supports:

- **Automatic evaluation** using built-in metrics (ROUGE, BERTScore, accuracy)
- **Human evaluation** with managed reviewer workflows
- Evaluate across task types: summarization, question answering, text generation, classification
- Compare multiple models side-by-side
- Results stored in Amazon S3

**Workflow:**
1. Select a model (or multiple models to compare)
2. Choose task type and evaluation metric
3. Provide evaluation dataset
4. Run evaluation job
5. Review results in the console

---

## Knowledge Test — Evaluation Metrics

**Q1.** A team is evaluating the quality of AI-generated meeting summaries against human-written reference summaries. Which metric family is most appropriate?

- A) BLEU
- B) ROUGE
- C) Perplexity
- D) Pass@k

**Q2.** ROUGE-1 scores two summaries identically, but one clearly better captures the meaning of the original text. What is the most likely reason ROUGE fails here?

- A) ROUGE-1 only measures bigram overlap
- B) ROUGE is recall-oriented and penalizes longer summaries
- C) ROUGE measures lexical n-gram overlap and cannot detect semantic equivalence
- D) The reference summary contained PII that was filtered

**Q3.** A developer compares translation quality using BLEU. They notice very short outputs are penalized even when the words are correct. Why?

- A) BLEU rewards recall over precision
- B) BLEU applies a brevity penalty to discourage very short translations
- C) Short outputs contain fewer bigrams which reduces n-gram coverage
- D) BLEU only measures unigram overlap

**Q4.** Which evaluation metric uses contextual BERT embeddings to compare semantic similarity rather than exact token matching?

- A) ROUGE-L
- B) BLEU
- C) BERTScore
- D) Exact Match

**Q5.** A team evaluating a code generation model wants to measure the probability that at least one of five generated solutions passes all unit tests. Which metric should they use?

- A) BLEU
- B) ROUGE-2
- C) Pass@k (k=5)
- D) F1 Score

**Q6.** Which metric is most appropriate for evaluating the factual accuracy of a question-answering model where the expected answer is a short, specific phrase (e.g., a date, name, or number)?

- A) ROUGE-L
- B) Exact Match (EM)
- C) BERTScore
- D) Perplexity

**Q7.** A company wants to evaluate two Bedrock models for their customer service summarization use case and compare them using ROUGE scores automatically. Which AWS feature should they use?

- A) Amazon SageMaker Clarify
- B) Amazon Bedrock Guardrails
- C) Amazon Bedrock Model Evaluation
- D) AWS Trusted Advisor

**Q8.** ROUGE-L uses the Longest Common Subsequence (LCS) instead of consecutive n-grams. What advantage does this provide over ROUGE-1 or ROUGE-2?

- A) It is faster to compute
- B) It captures sentence-level structure and word order without requiring consecutive matches
- C) It penalizes overly short outputs like BLEU
- D) It uses contextual embeddings for comparison

---

**Answers:**
1. B — ROUGE is the standard metric for summarization evaluation
2. C — ROUGE's lexical matching misses semantic equivalence ("automobile" ≠ "car" to ROUGE)
3. B — BLEU's brevity penalty penalizes outputs shorter than the reference, even accurate ones
4. C — BERTScore uses BERT contextual embeddings for semantic token matching
5. C — Pass@k measures probability that at least one of k solutions passes tests
6. B — Exact Match is used when the correct answer is a specific, unambiguous string
7. C — Amazon Bedrock Model Evaluation provides automated metric comparisons across models
8. B — LCS captures in-order subsequences, preserving sentence structure without requiring adjacent n-grams
