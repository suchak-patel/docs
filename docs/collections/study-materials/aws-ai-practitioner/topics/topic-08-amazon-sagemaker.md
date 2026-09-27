# Topic 8: Amazon SageMaker

> Sources: [Amazon SageMaker AI Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) | [SageMaker JumpStart](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html) | [SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domains 2 & 3** — SageMaker is AWS's platform for building **custom** ML models, complementing Bedrock's managed foundation models.

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
6. A — Automatic Model Tuning (HPO) searches hyperparameter space# Topic 08: Amazon SageMaker

This page is the new collection location for the Amazon SageMaker study topic.

The detailed content will be migrated here from the legacy `aws-ai-practitioner/` source page.
