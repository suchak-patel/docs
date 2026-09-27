# Topic 4: Responsible AI

> Sources: [SageMaker Clarify docs](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-configure-processing-jobs.html) | [Bedrock Guardrails docs](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html) | [Amazon AI Fairness and Explainability Whitepaper](https://pages.awscloud.com/rs/112-TZM-766/images/Amazon.AI.Fairness.and.Explainability.Whitepaper.pdf)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

---

## What is Responsible AI?

Responsible AI refers to the practice of designing, developing, and deploying AI systems that are **fair, transparent, explainable, safe, and accountable** — minimizing harm to individuals and society.

### AWS Responsible AI Pillars

| Pillar | Description |
|--------|-------------|
| **Fairness** | Avoid biased decisions that disadvantage groups |
| **Explainability** | Understand why the model made a prediction |
| **Privacy & Security** | Protect sensitive data and user information |
| **Transparency** | Document model capabilities, limitations, and data lineage |
| **Robustness** | Model performs reliably under adversarial or out-of-distribution inputs |
| **Governance** | Policies, processes, and oversight for responsible deployment |

---

## Types of Bias

Bias can enter the ML pipeline at multiple stages. Understanding *where* bias originates is critical for the exam.

### Pre-Training Bias (Data Bias)

Bias present in the **training dataset** before the model is trained.

| Bias Type | Description | Example |
|-----------|-------------|---------|
| **Class imbalance** | One group is underrepresented in the data | 95% male loan applicants in historical training data |
| **Label bias** | Human annotators who labeled data held biased beliefs | Annotators consistently rated minority applicants as higher risk |
| **Sample bias** | Data does not represent the real deployment population | Training a global model only on English-language US data |
| **Historical bias** | Past societal discrimination embedded in historical records | Historical hiring data reflecting gender pay gaps |
| **Measurement bias** | Different data quality or collection methods across groups | Higher-quality medical records for affluent communities |

### Post-Training Bias (Model Bias)

Bias **introduced or amplified** by the training process or by how the model is used.

| Bias Type | Description |
|-----------|-------------|
| **Disparate impact** | Model produces systematically different outcomes for different demographic groups |
| **Representation bias** | Model underperforms for groups underrepresented in training data |
| **Automation bias** | Humans over-rely on AI decisions even when the AI is wrong |
| **Feedback loop bias** | Model decisions influence future training data, amplifying initial biases over time |

---

## Amazon SageMaker Clarify

> Source: [SageMaker Clarify docs](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-configure-processing-jobs.html)

SageMaker Clarify provides **bias detection and model explainability** tools across the entire ML lifecycle.

> **Note:** As of recent AWS updates, SageMaker Clarify is no longer open to new customers. Existing customers continue to use it as normal.

### Key Capabilities

| Capability | Stage | Description |
|-----------|-------|-------------|
| **Pre-training bias metrics** | Data prep | Detect bias in dataset before training |
| **Post-training bias metrics** | After training | Detect bias introduced by the algorithm or hyperparameters |
| **SHAP values** | After training / production | Feature-level attribution — which features drove the prediction |
| **Partial Dependence Plots (PDPs)** | After training | How predictions change as a single feature varies |
| **Feature attribution drift monitoring** | Production | Alert when feature importance changes over time |
| **Bias drift monitoring** | Production | Alert when bias metrics deviate from training-time baseline |
| **Governance reports** | Compliance | Generate bias/explainability reports for risk and regulatory teams |

### Key Pre-Training Bias Metrics

| Metric | What It Measures |
|--------|-----------------|
| **Class Imbalance (CI)** | Imbalance in representation of groups in training data |
| **Difference in Positive Proportions in Labels (DPL)** | Difference in proportion of positive labels between groups |
| **Kullback-Leibler Divergence (KL)** | How much label distribution differs across groups |

### Key Post-Training Bias Metrics

| Metric | What It Measures |
|--------|-----------------|
| **Disparate Impact (DI)** | Ratio of positive outcome rates between privileged and unprivileged groups. DI = 1.0 is ideal. |
| **Accuracy Difference** | Difference in model accuracy between demographic groups |
| **Conditional Demographic Disparity (CDD)** | Demographic disparity conditioned on other features |

### SHAP Values (Explainability)

SHAP (SHapley Additive exPlanations) is based on **game theory**:

- Assigns a contribution score to **each feature** for each individual prediction
- Higher absolute SHAP value = greater influence on that prediction
- Works **model-agnostically** (any ML model type)
- Provides both:
  - **Global explanations** — aggregate feature importance across all predictions
  - **Local explanations** — feature contribution for a single prediction

**Example:** A credit model with high SHAP for "zip code" means zip code strongly influences individual credit decisions — a potential proxy for protected attributes (redlining concern).

### How Clarify Works

```
Input dataset + Configuration
	↓
SageMaker Clarify Processing Container
	↓ (for post-training: send requests to model endpoint)
Compute bias metrics + SHAP values + PDPs
	↓
Save results to S3
	↓
JSON bias report + Visual HTML report + Local SHAP files
```

---

## Hallucinations

A hallucination occurs when an AI model **generates confident, plausible-sounding but factually incorrect** information not supported by its training data or provided context.

### Types

| Type | Description |
|------|-------------|
| **Factual hallucination** | Model states incorrect facts as true |
| **Faithfulness hallucination** | Response contradicts or adds to the provided context |
| **Instruction hallucination** | Model ignores or contradicts the user's explicit instruction |

### Mitigation Strategies

| Strategy | Mechanism |
|----------|-----------|
| **RAG** | Ground responses in retrieved factual source documents |
| **Bedrock Guardrails — Contextual grounding** | Detect when response diverges from retrieved context |
| **Bedrock Guardrails — Automated Reasoning** | Validate against logical rule sets |
| **Prompt engineering** | "Only use information from the provided context. If unsure, say you don't know." |
| **Lower temperature** | Reduce randomness in token selection |
| **Human evaluation** | Include human review for high-stakes applications |

---

## Amazon Bedrock Guardrails for Responsible AI

| Guardrail Feature | Responsible AI Dimension |
|-------------------|--------------------------| 
| Content filters (hate, violence, sexual) | **Safety** — prevent harmful outputs |
| Sensitive information filters (PII masking) | **Privacy** — protect user data |
| Denied topics | **Governance** — enforce scope of use |
| Contextual grounding checks | **Accuracy** — reduce hallucinations |
| Automated Reasoning checks | **Reliability** — validate logical correctness |
| Word filters | **Compliance** — block prohibited terms |

---

## AWS AI Service Cards

AWS publishes **AI Service Cards** for managed AI services — official transparency documents describing:

- Intended use cases and out-of-scope uses
- Fairness design considerations
- Model limitations
- Best practices for responsible deployment
- Safety evaluations

**Example services with published Service Cards:**
- Amazon Rekognition
- Amazon Comprehend
- Amazon Transcribe
- Amazon Bedrock models

---

## SageMaker Model Cards

SageMaker Model Cards document individual trained models and include:

| Section | Content |
|---------|---------|
| Model overview | Model name, version, purpose, intended use |
| Intended uses | Approved use cases + out-of-scope uses |
| Training data | Data description, preprocessing steps |
| Evaluation results | Metrics, benchmark results, performance by group |
| Ethical considerations | Fairness concerns, known failure modes |
| Caveats and recommendations | Limitations and safe deployment guidance |

Model Cards can be shared with risk, compliance, and regulatory teams.

---

## Privacy and Security in AI

| Concern | Mitigation |
|---------|-----------|
| PII in training data | Data anonymization, differential privacy |
| PII in user inputs | Bedrock Guardrails — sensitive information filters |
| Model inversion attacks | Limit access to model outputs, add output noise |
| Data leakage between tenants | AWS IAM, VPC isolation, AWS PrivateLink |
| Prompt injection | Bedrock Guardrails — content filters, prompt attack detection |

---

## The ML Lifecycle: Fairness Checkpoints

Per AWS SageMaker Clarify documentation, evaluate fairness and explainability at **every stage**:

```
Problem Formulation
	↓  Is the problem definition itself biased?
Dataset Construction
	↓  Is training data representative? (SageMaker Clarify: pre-training metrics)
Algorithm Selection
	↓  Is the algorithm appropriate and ethical?
Model Training
	↓  Are fairness constraints in the objective function?
Testing / Evaluation
	↓  Evaluate with fairness metrics across all groups (SageMaker Clarify: post-training)
Deployment
	↓  Is the model deployed on the population it was trained/evaluated for?
Monitoring & Feedback
	↓  Detect drift in bias and feature attributions over time (Clarify monitoring)
```

---

## Knowledge Test — Responsible AI

**Q1.** A bank trains a loan approval model using 10 years of historical approval data. The historical data reflects past discriminatory lending practices. What type of bias is this?

- A) Post-training bias
- B) Historical bias (pre-training data bias)
- C) Automation bias
- D) Representation bias from the algorithm

**Q2.** A SageMaker Clarify report shows high SHAP values for the "zip code" feature in a credit scoring model. What does this mean?

- A) The model is definitively biased against certain zip codes and must be retrained
- B) Zip code has high predictive influence on the model's credit score predictions
- C) The zip code feature has class imbalance in training data
- D) The model needs more training data from underrepresented zip codes

**Q3.** A deployed ML model starts producing biased predictions 6 months after launch as the demographic distribution of users shifts. Which SageMaker Clarify feature is designed to detect this?

- A) Pre-training bias metrics
- B) SHAP values
- C) Bias drift monitoring in production
- D) Partial Dependence Plots

**Q4.** A Bedrock-powered RAG application returns a confident response that includes facts not present in the retrieved documents. Which Guardrails feature should be enabled?

- A) Denied topics
- B) Word filters
- C) Sensitive information filters
- D) Contextual grounding checks

**Q5.** Which document type published by AWS describes the intended use cases, fairness considerations, and limitations for managed AI services like Amazon Rekognition?

- A) SageMaker Model Cards
- B) AWS AI Service Cards
- C) SageMaker Clarify Reports
- D) AWS Trusted Advisor reports

**Q6.** A company processes customer support transcripts using Amazon Bedrock. Legal requires that customer SSNs and dates of birth are never stored or passed to the model. Which feature enforces this?

- A) Denied topics
- B) Contextual grounding checks
- C) Bedrock Guardrails sensitive information filters (PII masking)
- D) Amazon Macie

**Q7.** Which type of bias occurs when an AI system's predictions are used to generate the next round of training data, causing initial biases to compound over time?

- A) Sample bias
- B) Label bias
- C) Feedback loop bias
- D) Measurement bias

**Q8.** A developer wants to understand which features most influenced a specific loan rejection decision to explain it to the applicant. Which SageMaker Clarify capability provides this?

- A) Pre-training bias metrics (Class Imbalance)
- B) Global SHAP values
- C) Local SHAP values for the individual prediction
- D) Partial Dependence Plots

---

**Answers:**
1. B — The bias originated in historical lending records (pre-training data bias, specifically historical bias)
2. B — High SHAP = high predictive influence; this warrants investigation but does not automatically confirm bias
3. C — Bias drift monitoring in production detects when real-world bias metrics deviate from training baseline
4. D — Contextual grounding checks detect responses that deviate from or add to retrieved source documents
5. B — AWS AI Service Cards are the transparency documents for managed AWS AI services
6. C — Sensitive information filters detect and mask/block PII before it reaches the model
7. C — Feedback loop bias describes the compounding of initial biases through iterative training cycles
8. C — Local SHAP values explain a single individual prediction; global SHAP values aggregate across all predictions
