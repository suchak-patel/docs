# Topic 6: AI and ML Fundamentals

> Sources: [What is Artificial Intelligence?](https://aws.amazon.com/what-is/artificial-intelligence/) | [What is Machine Learning?](https://aws.amazon.com/what-is/machine-learning/) | [Amazon SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domain 1 — Fundamentals of AI and ML (20% of exam)**

---

## The AI Hierarchy

Understanding how these terms nest is a common exam question.

```
Artificial Intelligence (AI)
    └── Machine Learning (ML)
            └── Deep Learning (DL)
                    └── Generative AI (GenAI)
```

| Term | Definition |
|------|------------|
| **Artificial Intelligence (AI)** | Broad field of building systems that perform tasks requiring human-like intelligence (reasoning, perception, decision-making) |
| **Machine Learning (ML)** | Subset of AI where systems **learn patterns from data** instead of being explicitly programmed |
| **Deep Learning (DL)** | Subset of ML using multi-layer **neural networks** to learn complex patterns (images, audio, language) |
| **Generative AI** | Subset of DL that **creates new content** (text, images, code, audio) using foundation models |

---

## AI vs. ML vs. Deep Learning — Key Distinctions

| Aspect | Traditional Programming | Machine Learning |
|--------|------------------------|------------------|
| **Logic** | Human writes explicit rules | Model infers rules from data |
| **Input** | Rules + data → output | Data + output → rules (model) |
| **Adaptation** | Manual code changes | Retrain on new data |
| **Best for** | Deterministic, well-defined tasks | Pattern recognition, prediction, complex/fuzzy tasks |

> **Exam tip:** If a problem can be solved with simple `if/then` rules, you likely **don't** need ML. ML is appropriate when the logic is too complex to hand-code, when patterns must be learned from large datasets, or when the system must adapt over time.

---

## Types of Machine Learning

### 1. Supervised Learning

The model learns from **labeled data** — each training example includes the correct answer (label).

| Sub-type | Output | Example | AWS Service Example |
|----------|--------|---------|---------------------|
| **Classification** | Discrete category | Spam vs. not spam; fraud vs. legitimate | Amazon Comprehend (sentiment) |
| **Regression** | Continuous number | Predict house price, sales forecast | Amazon Forecast, SageMaker |

**Key idea:** Requires large amounts of labeled data. Labeling is often costly and time-consuming.

### 2. Unsupervised Learning

The model finds hidden structure in **unlabeled data** — no correct answers provided.

| Sub-type | Purpose | Example |
|----------|---------|---------|
| **Clustering** | Group similar items | Customer segmentation |
| **Dimensionality reduction** | Compress features | PCA for visualization |
| **Anomaly detection** | Find outliers | Fraud / intrusion detection |
| **Association** | Find co-occurrence rules | Market basket analysis |

**Key idea:** No labels needed — useful for exploratory analysis and finding unknown patterns.

### 3. Reinforcement Learning (RL)

An **agent** learns by interacting with an **environment**, receiving **rewards** or **penalties** for actions, and optimizing a long-term reward.

| Concept | Description |
|---------|-------------|
| **Agent** | The learner / decision-maker |
| **Environment** | The world the agent acts in |
| **Action** | A choice the agent makes |
| **Reward** | Feedback signal (positive or negative) |
| **Policy** | Strategy mapping states → actions |

**Examples:** Robotics, game playing, autonomous navigation, recommendation optimization.
**AWS example:** AWS DeepRacer (RL training via a simulated race car).

> **RLHF (Reinforcement Learning from Human Feedback)** is used to align large language models with human preferences — a key GenAI connection.

### Comparison Table

| | Supervised | Unsupervised | Reinforcement |
|-|-----------|--------------|---------------|
| **Data** | Labeled | Unlabeled | No dataset — learns from interaction |
| **Goal** | Predict labels | Find structure | Maximize cumulative reward |
| **Feedback** | Correct answer given | None | Reward/penalty signal |
| **Examples** | Classification, regression | Clustering, anomaly detection | Robotics, game AI, DeepRacer |

### Semi-Supervised & Self-Supervised (bonus)

| Type | Description |
|------|-------------|
| **Semi-supervised** | Small amount of labeled data + large amount of unlabeled data |
| **Self-supervised** | Model generates its own labels from the data structure (how LLMs pre-train on text) |

---

## The Machine Learning Lifecycle

```
1. Business problem framing
2. Data collection
3. Data preparation (cleaning, labeling, feature engineering)
4. Model training
5. Model evaluation / validation
6. Deployment (inference)
7. Monitoring & retraining
```

| Phase | Description | AWS Tooling |
|-------|-------------|-------------|
| **Data preparation** | Clean, transform, label, split data | SageMaker Data Wrangler, Ground Truth |
| **Feature engineering** | Create/select input variables | SageMaker Feature Store |
| **Training** | Fit model to training data | SageMaker Training Jobs |
| **Evaluation** | Measure quality on held-out data | SageMaker, Clarify |
| **Deployment** | Serve predictions | SageMaker Endpoints |
| **Monitoring** | Detect drift, degradation | SageMaker Model Monitor |

---

## Training, Validation, and Test Data

A dataset is split into three parts to build and honestly evaluate a model.

| Split | Purpose | Typical Ratio |
|-------|---------|---------------|
| **Training set** | Model learns patterns / adjusts weights | ~70–80% |
| **Validation set** | Tune hyperparameters, select best model | ~10–15% |
| **Test set** | Final unbiased performance estimate | ~10–15% |

> **Exam tip:** The test set must remain **completely unseen** during training and tuning — otherwise performance estimates are overly optimistic (data leakage).

---

## Overfitting vs. Underfitting

| Problem | Symptom | Cause | Fix |
|---------|---------|-------|-----|
| **Overfitting** | High training accuracy, low test accuracy | Model memorizes noise; too complex | More data, regularization, simpler model, dropout, early stopping |
| **Underfitting** | Low accuracy on both train and test | Model too simple; not enough training | More complex model, more features, train longer |

| Concept | Definition |
|---------|------------|
| **Bias (in ML modeling sense)** | Error from overly simplistic assumptions → underfitting |
| **Variance** | Error from sensitivity to training data noise → overfitting |
| **Bias–variance trade-off** | Balancing the two for best generalization |

---

## Training Concepts and Hyperparameters

| Term | Definition |
|------|------------|
| **Parameters** | Values the model **learns** during training (e.g., weights) |
| **Hyperparameters** | Values **set before** training that control the process |
| **Epoch** | One full pass through the training dataset |
| **Batch size** | Number of samples processed before updating weights |
| **Learning rate** | How much weights change per update step |
| **Loss function** | Measures error between prediction and truth |
| **Gradient descent** | Optimization algorithm that minimizes loss |

> **Hyperparameter tuning** (a.k.a. HPO) automatically searches for the best hyperparameter combination — SageMaker Automatic Model Tuning does this.

---

## Inference

**Inference** is using a trained model to make predictions on new data.

| Inference Type | Description | Best For |
|----------------|-------------|----------|
| **Real-time inference** | Low-latency, synchronous predictions via a persistent endpoint | Chatbots, fraud detection at checkout |
| **Batch inference** | Predictions on large datasets at once, asynchronous | Nightly scoring, bulk document processing |
| **Asynchronous inference** | Queued requests, large payloads, near-real-time | Large images, long audio |
| **Serverless inference** | Auto-scales, pay-per-use, scales to zero | Intermittent / spiky traffic |

---

## Types of Data

| Data Type | Description | Example |
|-----------|-------------|---------|
| **Structured** | Rows and columns, fixed schema | Databases, CSV, spreadsheets |
| **Semi-structured** | Flexible tags/keys | JSON, XML, logs |
| **Unstructured** | No predefined model | Text, images, audio, video |
| **Time-series** | Sequential data indexed by time | Stock prices, sensor readings |

---

## Common ML Use Cases and AWS Mapping

| Use Case | ML Type | AWS Service |
|----------|---------|-------------|
| Image/object recognition | Supervised (DL) | Amazon Rekognition |
| Sentiment analysis | Supervised (NLP) | Amazon Comprehend |
| Demand forecasting | Supervised (regression) | Amazon SageMaker / Forecast |
| Customer segmentation | Unsupervised (clustering) | Amazon SageMaker |
| Fraud/anomaly detection | Unsupervised / supervised | Amazon Fraud Detector |
| Chatbots | NLP / GenAI | Amazon Lex, Amazon Q |
| Content generation | Generative AI | Amazon Bedrock |
| Personalized recommendations | Supervised / RL | Amazon Personalize |

---

## When NOT to Use Machine Learning

- The problem is solvable with simple deterministic rules
- You lack sufficient quality data
- You need fully explainable, guaranteed-correct outputs (safety-critical hard constraints)
- The cost/effort of building and maintaining ML outweighs the benefit

---

## Knowledge Test — AI and ML Fundamentals

**Q1.** A bank wants to group customers into segments based on spending behavior, but has no predefined categories. Which type of machine learning is most appropriate?

- A) Supervised learning
- B) Unsupervised learning
- C) Reinforcement learning
- D) Semi-supervised learning

**Q2.** A model achieves 99% accuracy on training data but only 62% on the test set. What is happening?

- A) Underfitting
- B) Overfitting
- C) Data leakage into the test set
- D) High bias

**Q3.** Which type of machine learning is best described by an agent learning through rewards and penalties by interacting with an environment?

- A) Supervised learning
- B) Unsupervised learning
- C) Reinforcement learning
- D) Self-supervised learning

**Q4.** A company needs to predict next quarter's sales revenue as a continuous dollar amount. Which supervised learning task is this?

- A) Classification
- B) Clustering
- C) Regression
- D) Anomaly detection

**Q5.** What is the purpose of the validation dataset in the ML lifecycle?

- A) To train the model's weights
- B) To provide the final unbiased performance estimate
- C) To tune hyperparameters and select the best model
- D) To label the raw data

**Q6.** Which of the following is a **hyperparameter** rather than a learned parameter?

- A) A neural network weight
- B) The learning rate
- C) The model's bias term
- D) The predicted output value

**Q7.** A retailer needs low-latency predictions to detect fraud during checkout. Which inference type fits best?

- A) Batch inference
- B) Real-time inference
- C) Asynchronous inference
- D) Offline scoring

---

**Answers:**
1. B — No predefined labels/categories → unsupervised clustering
2. B — Large train/test gap is the classic sign of overfitting
3. C — Agent + environment + reward signal defines reinforcement learning
4. C — Predicting a continuous numeric value is regression
5. C — Validation is used for hyperparameter tuning and model selection
6. B — Learning rate is set before training (hyperparameter); weights/bias are learned
7. B — Synchronous, low-latency predictions require a real-time endpoint# Topic 06: AI and ML Fundamentals

This page is the new collection location for the AI and ML fundamentals study topic.

The detailed content will be migrated here from the legacy `aws-ai-practitioner/` source page.
