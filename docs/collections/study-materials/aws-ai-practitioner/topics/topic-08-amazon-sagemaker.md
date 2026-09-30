# Topic 8: Amazon SageMaker

> Sources: [Amazon SageMaker AI Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) | [SageMaker JumpStart](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html) | [SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domains 2 & 3** — SageMaker is AWS's platform for building **custom** ML models, complementing Bedrock's managed foundation models.

---

## Quick Revision (TL;DR)

- **SageMaker** = build / train / deploy **custom** ML models (full control, ML expertise). **Bedrock** = consume **pre-built FMs** via API.
- **Data:** Ground Truth (**labeling**), Data Wrangler (prep), Feature Store, **Canvas** (**no-code** for analysts), **JumpStart** (model hub + FMs + templates).
- **Train/tune:** Training Jobs, **Automatic Model Tuning (HPO)**, Experiments.
- **Deploy:** real-time endpoint, **serverless**, **asynchronous**, **batch transform**.
- **MLOps/governance:** **Model Monitor** (drift), **Model Registry** (versioning/approval), **Model Cards** (governance docs), **Clarify** (bias + explainability), Pipelines (CI/CD).
- **Drift:** data drift (input shifts) vs concept drift (input→target relationship shifts) → monitor + retrain.

---

## In Plain English

If **Bedrock** is a buffet of ready-made dishes you just scoop onto a plate, **SageMaker** is a fully equipped professional kitchen where you cook your own recipe from scratch. You reach for SageMaker when an off-the-shelf AI service can't solve your specific problem — say, predicting *your* customers' churn or classifying a rare species of bird that no pre-trained model knows.

SageMaker covers the **entire** machine-learning lifecycle in one place. You label raw data with **Ground Truth**, prep and engineer features with **Data Wrangler** and **Feature Store**, build and experiment in **Studio** (or point-and-click in **Canvas**, or start from a pre-built model in **JumpStart**), train with **Training Jobs** (letting **Automatic Model Tuning** search for the best hyperparameters), then **deploy** to an endpoint. After go-live, **Model Monitor** watches for drift, **Model Registry** versions and approves models, and **Model Cards** document them for governance.

The trade-off is control vs. effort. SageMaker gives you full control and requires more ML skill than Bedrock — but tools like **Canvas** (no-code for business analysts) and **JumpStart** (one-click foundation models and templates) lower the barrier so you don't need a PhD to get started.

---

## What is Amazon SageMaker?

Amazon SageMaker AI is a **fully managed service** to **build, train, and deploy** machine learning models at scale. It covers the entire ML lifecycle in one platform.

> **Bedrock vs. SageMaker — the key exam distinction:**
> - **Amazon Bedrock** → consume **pre-built foundation models** via API (serverless, GenAI-focused, minimal ML expertise).
> - **Amazon SageMaker** → **build, train, fine-tune, and host your own** custom models (full control, requires ML expertise).

---

## SageMaker Capabilities Across the ML Lifecycle

| Lifecycle Stage | SageMaker Feature |
|-----------------|-------------------|
| **Data preparation** | Data Wrangler, Ground Truth, Feature Store, Processing Jobs |
| **Build / experiment** | SageMaker Studio, Notebooks, JumpStart |
| **Train / tune** | Training Jobs, Automatic Model Tuning, Experiments |
| **Deploy / host** | Endpoints (real-time, serverless, async), Batch Transform |
| **Monitor / govern** | Model Monitor, Clarify, Model Cards, Model Registry, Pipelines |

---

## Key SageMaker Components

### Development & Data

| Feature | Purpose |
|---------|---------|
| **SageMaker Studio** | Web-based IDE for the full ML workflow |
| **SageMaker Ground Truth** | Human data **labeling** service (with human workforce / Mechanical Turk) |
| **SageMaker Data Wrangler** | Visual data preparation and feature engineering (no/low-code) |
| **SageMaker Feature Store** | Central repository to store, share, and reuse curated ML features |
| **SageMaker Canvas** | **No-code** ML — build models via a visual UI for business analysts |
| **SageMaker JumpStart** | Pre-trained models & solution templates (including foundation models) for quick start |

### Training & Tuning

| Feature | Purpose |
|---------|---------|
| **Training Jobs** | Managed, scalable model training on chosen instance types |
| **Automatic Model Tuning (HPO)** | Automatically searches for the best hyperparameters |
| **SageMaker Experiments** | Track, compare, and organize training runs |
| **Distributed training** | Data/model parallelism across multiple instances/GPUs |

### Deployment / Inference Options

| Option | Description | Best For |
|--------|-------------|----------|
| **Real-time endpoint** | Persistent, low-latency HTTPS endpoint | Live predictions (fraud, chat) |
| **Serverless inference** | Auto-scales, scales to zero, pay-per-use | Intermittent/spiky traffic |
| **Asynchronous inference** | Queues requests; large payloads | Large images, long processing |
| **Batch Transform** | Predictions on large datasets, no persistent endpoint | Offline/bulk scoring |

---

## Governance & MLOps Features

| Feature | Purpose |
|---------|---------|
| **SageMaker Model Registry** | Version, catalog, and manage model approval status for deployment |
| **SageMaker Pipelines** | CI/CD orchestration for repeatable ML workflows (MLOps) |
| **SageMaker Model Cards** | Document model details, intended use, risk rating, and performance for governance |
| **SageMaker Model Monitor** | Detects **data drift**, **model quality drift**, bias drift, feature attribution drift in production |
| **SageMaker Clarify** | Bias detection (pre/post-training) and explainability (SHAP) — see [topic-04-responsible-ai.md](topic-04-responsible-ai.md) |
| **SageMaker Role Manager** | Simplifies IAM permission management for ML users |

> **Exam tip:** **Model Monitor** = ongoing production monitoring for drift. **Model Cards** = governance documentation. **Model Registry** = versioning + approval workflow.

---

## Model Drift Concepts

| Drift Type | Description |
|------------|-------------|
| **Data drift (covariate shift)** | The distribution of **input data** changes over time vs. training data |
| **Concept drift** | The **relationship** between inputs and target changes (what a "good" answer is shifts) |
| **Model quality drift** | Prediction accuracy degrades over time |

> Drift is why models require **monitoring and periodic retraining** — a common exam theme.

---

## SageMaker JumpStart

**SageMaker JumpStart** is a model hub and solution library that accelerates ML.

- Pre-trained, open-source and proprietary **foundation models** ready to deploy
- One-click deployment and fine-tuning
- Pre-built end-to-end **solution templates** for common use cases
- Good bridge between "just use Bedrock" and "build fully custom"

---

## SageMaker Canvas

**No-code** ML for business analysts:
- Build, train, and generate predictions **without writing code**
- Visual, point-and-click interface
- Can integrate with Bedrock foundation models and Ready-to-use models
- Great answer when the scenario says **"business users with no ML/coding skills"**

---

## Choosing the Right AWS ML Approach

| Requirement | Best Fit |
|-------------|----------|
| Use a GenAI foundation model via API, no infra | **Amazon Bedrock** |
| Build/train a fully custom model with full control | **SageMaker Training / Studio** |
| No-code model building for business analysts | **SageMaker Canvas** |
| Pre-trained models + templates to start fast | **SageMaker JumpStart** |
| Label a large raw dataset | **SageMaker Ground Truth** |
| Detect production drift | **SageMaker Model Monitor** |
| Bias detection & explainability | **SageMaker Clarify** |
| Off-the-shelf AI (vision, speech, text) | **AWS AI services** — see [topic-09-aws-ai-services.md](topic-09-aws-ai-services.md) |

---

## How AIF-C01 Actually Tests This

The exam rarely asks *how* to configure SageMaker. It asks *which* SageMaker capability fits a scenario, and *when* to choose SageMaker over Bedrock or an AI service.

**Exam topics you must master:**

- **Bedrock vs. SageMaker.** Consume/customize ready-made FMs with little ML skill → **Bedrock**. Build/train/host a **custom** model with full control → **SageMaker**. Common task (translate, detect faces) → **AI service**.
- **No-code for business analysts → SageMaker Canvas.**
- **Label a large raw dataset → SageMaker Ground Truth.**
- **Detect production drift → SageMaker Model Monitor.**
- **Bias detection + explainability → SageMaker Clarify.**
- **Version/approve models → Model Registry;** **document for governance → Model Cards.** Don't mix these three (Monitor / Registry / Cards).
- **Inference type by workload:** live low-latency → real-time endpoint; spiky/intermittent → serverless; large payloads/long jobs → asynchronous; offline bulk scoring → batch transform.
- **Pre-trained models + templates to start fast → JumpStart.**

**Trap patterns to watch for:**

- **"SageMaker only serves pre-built models"** → false; it's for building/training custom models (Bedrock serves pre-built FMs).
- **Model Monitor ≠ auto-retrain** — it *detects* drift and alerts; you trigger retraining.
- **"You need a persistent endpoint for every prediction"** → false; **batch transform** scores large datasets with no standing endpoint.
- **Canvas vs. Studio:** Canvas = no-code visual; Studio = full IDE for practitioners.

---

## Common Misconceptions

- **Misconception:** You need to be a data-science PhD to use SageMaker. **Reality:** Canvas (no-code) and JumpStart (pre-built models/templates) make it approachable; managed training abstracts the infrastructure.
- **Misconception:** SageMaker is just another way to call pre-built models like Rekognition. **Reality:** SageMaker builds/trains/deploys *your own* custom models; the pre-trained AI services are separate.
- **Misconception:** Model Monitor automatically fixes or retrains a drifting model. **Reality:** It detects data/quality/bias drift and alerts — remediation (retraining) is a separate action you take.
- **Misconception:** Every deployment needs a real-time endpoint. **Reality:** Choose by workload — batch transform for offline scoring, serverless for spiky traffic, async for large payloads.
- **Misconception:** Model Registry, Model Cards, and Model Monitor do the same thing. **Reality:** Registry = versioning/approval; Cards = governance documentation; Monitor = production drift detection.

---

## Knowledge Test — Amazon SageMaker

**Q1.** A team of business analysts with no coding experience needs to build predictive models using a visual interface. Which SageMaker capability fits best?

- A) SageMaker Training Jobs
- B) SageMaker Canvas
- C) SageMaker Pipelines
- D) SageMaker Ground Truth

**Q2.** A deployed model's accuracy is silently degrading because real-world input data has shifted from the training distribution. Which SageMaker feature detects this?

- A) SageMaker Clarify
- B) SageMaker Model Registry
- C) SageMaker Model Monitor
- D) SageMaker JumpStart

**Q3.** A company needs to label 100,000 images to create a training dataset. Which service should they use?

- A) SageMaker Feature Store
- B) SageMaker Ground Truth
- C) SageMaker Canvas
- D) SageMaker Model Cards

**Q4.** Which statement best distinguishes Amazon Bedrock from Amazon SageMaker?

- A) Bedrock trains custom models; SageMaker only serves pre-built models
- B) Bedrock provides managed foundation models via API; SageMaker is for building/training/hosting custom models
- C) They are identical services with different names
- D) SageMaker is serverless; Bedrock always requires managing servers

**Q5.** A team wants to version models, track approval status, and manage which models are ready for deployment. Which feature supports this?

- A) SageMaker Experiments
- B) SageMaker Model Registry
- C) SageMaker Data Wrangler
- D) SageMaker Feature Store

**Q6.** Which SageMaker feature automatically searches for the best combination of hyperparameters?

- A) Automatic Model Tuning
- B) Model Monitor
- C) Feature Store
- D) Batch Transform

---

**Answers:**
1. B — SageMaker Canvas provides no-code, visual model building
2. C — Model Monitor detects data/model quality drift in production
3. B — Ground Truth is the managed data labeling service
4. B — Bedrock = managed FMs via API; SageMaker = build/train/host custom models
5. B — Model Registry handles versioning and deployment approval
6. A — Automatic Model Tuning (HPO) searches hyperparameter space
