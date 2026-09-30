# Topic 6: AI and ML Fundamentals

> Sources: [What is Artificial Intelligence?](https://aws.amazon.com/what-is/artificial-intelligence/) | [What is Machine Learning?](https://aws.amazon.com/what-is/machine-learning/) | [Amazon SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domain 1 — Fundamentals of AI and ML (20% of exam)**

---

## Quick Revision (TL;DR)

- **Nesting:** AI ⊃ ML ⊃ Deep Learning ⊃ Generative AI.
- **ML types:** **supervised** (labeled → classification/regression), **unsupervised** (unlabeled → clustering/anomaly), **reinforcement** (agent + reward).
- **Lifecycle:** problem → data prep → train → **evaluate** → deploy (**inference**) → monitor/retrain (loop).
- **Phase ID:** learns/adjusts weights = **training**; tested on held-out labels = **evaluation**; predicts on new live data = **inference** (no learning).
- **Overfitting** = train ≫ test; **underfitting** = both low. Fix overfit with more data/regularization/simpler model.
- **Split:** training (~70–80%) / validation (tune) / test (final, unseen).
- **Metrics:** accuracy (balanced), **precision** (costly FP), **recall** (costly FN), **F1** (imbalanced); regression = MAE/RMSE/R².
- **Features** = inputs; **label** = the answer being predicted. **Concept drift** → retrain.

---

## In Plain English

Think about a traditional recipe book: mix 200g flour, 2 eggs, bake at 180°C for 30 minutes. The rules are fixed, so the result is predictable. That's **traditional programming** — a human writes every rule. Now imagine teaching someone to judge a *good* cake by taste, without giving exact proportions. You let them taste hundreds of cakes, each time saying "good" or "bad," until they build a mental model of what works. That's **machine learning** — the computer learns patterns from labeled examples instead of hand-written rules.

Every ML system runs the same loop. During **training**, you feed the model labeled examples (features like bedrooms and square footage; the label is the sale price). It makes a guess, measures how wrong it was, and nudges its internal "knobs" to guess better — repeated many times. During **evaluation**, you test it on a **held-out** set it never saw, because grading it on the data it trained on is like giving a student the exam answers in advance. Only when it passes do you move to **inference**: running the finished model on brand-new, real-world data. Crucially, the model does **not** learn during inference — its parameters are frozen.

Two failure shapes come up constantly. **Overfitting** is memorizing the training data (99% on training, 55% on the test set) — like memorizing answers instead of understanding the subject. **Underfitting** is a model too simple to capture the pattern (low on both). And when the world shifts after deployment — customers change habits, fraudsters invent new tricks — accuracy drifts (**concept drift**), so you retrain. That's why the lifecycle is a loop, not a one-time build.

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

> **Exam trap:** Inference is **not** learning. During inference the model's parameters are **fixed** — it only applies patterns learned during training. "A model classifies transactions in a live app" = inference; "a model is tested on a held-out labeled dataset" = evaluation; "a model is retrained on new data" = training.

---

## Training vs. Inference vs. Evaluation

The exam frequently describes a scenario and asks which lifecycle phase it is. Match the scenario to the phase.

| Phase | What happens | Data used | Key signal in the question |
|-------|-------------|-----------|----------------------------|
| **Training** | Model adjusts internal parameters to minimize error | Labeled **training set** | "learns", "adjusts weights", "is retrained on new data" |
| **Evaluation** | Measure performance on unseen data with known answers | Held-out **test set** (labeled) | "tested", "measured accuracy/F1", "compared predictions to labels" |
| **Inference** | Trained model predicts on brand-new data | New, **unlabeled** production data | "in production", "predicts on new/live data", "real-time predictions" |

> **Features vs. labels:** **Features** are the input variables the model uses (e.g., bedrooms, square footage). The **label** is the correct output it learns to predict (e.g., house price). Labels exist only in training/evaluation data — never in the new data seen at inference.

---

## Model Evaluation Metrics

How you measure model quality depends on the task type. You don't need to calculate these on the exam, but you must know **what each measures**.

### The Confusion Matrix (Classification)

| | Predicted Positive | Predicted Negative |
|-|--------------------|--------------------|
| **Actual Positive** | True Positive (TP) | False Negative (FN) |
| **Actual Negative** | False Positive (FP) | True Negative (TN) |

### Classification Metrics

| Metric | Formula (concept) | What it measures | Use when |
|--------|-------------------|------------------|----------|
| **Accuracy** | (TP+TN) / all | Overall fraction of correct predictions | Classes are **balanced** |
| **Precision** | TP / (TP+FP) | Of predicted positives, how many were correct | **False positives** are costly (e.g., flagging good email as spam) |
| **Recall (Sensitivity)** | TP / (TP+FN) | Of actual positives, how many were caught | **False negatives** are costly (e.g., missing fraud or disease) |
| **F1 score** | Harmonic mean of precision & recall | Balance of precision and recall | Classes are **imbalanced**; both error types matter |

> **Exam tip:** On **imbalanced** data, accuracy is misleading. A fraud model that predicts "not fraud" every time can be 99% accurate yet catch **zero** fraud — recall and F1 reveal this failure.

> **Precision vs. recall trade-off:** Precision answers "when we say yes, are we right?" Recall answers "did we find all the real cases?" Raising one often lowers the other.

### Regression Metrics

| Metric | What it measures |
|--------|------------------|
| **MAE** (Mean Absolute Error) | Average absolute difference between prediction and actual |
| **MSE / RMSE** | Average (root) squared error — penalizes large errors more |
| **R² (coefficient of determination)** | Proportion of variance explained by the model (1.0 = perfect) |

> For foundation-model / generative-output metrics (ROUGE, BLEU, BERTScore, Perplexity), see [topic-05-evaluation-metrics.md](topic-05-evaluation-metrics.md).

---

## Concept Drift

**Concept drift** occurs when the real-world relationship between inputs and the target **changes over time**, degrading a deployed model's accuracy (e.g., customer behavior shifts after a pricing change; fraudsters invent new tactics).

| Response | Description |
|----------|-------------|
| **Monitor** | Track production accuracy against a threshold |
| **Retrain** | Collect new labeled data, retrain, re-evaluate, redeploy |

> Drift is why the ML lifecycle is a **loop**, not a one-time build. See also SageMaker Model Monitor in [topic-08-amazon-sagemaker.md](topic-08-amazon-sagemaker.md).

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

## How AIF-C01 Actually Tests This

Domain 1 is 20% of the exam. Expect scenario questions that ask **which phase** is happening or **which concept** a situation illustrates.

**Exam topics you must master:**

- **Identify the lifecycle phase.** "Model classifies images in a production app" → **inference**. "Model is tested on a held-out labeled dataset" → **evaluation**. "Model is retrained on new data" → **training**.
- **Why a separate test set exists:** evaluating on training data inflates the score because the model already saw the answers (that's how overfitting hides).
- **Overfitting vs. underfitting.** High train / low test = overfitting. Low on both = underfitting.
- **What the metrics measure (no calculation needed):** **precision** is about avoiding **false positives**; **recall** is about avoiding **false negatives**; **F1** balances the two; **accuracy** is misleading on imbalanced data.
- **Features vs. labels.** Features are the inputs; the label is the correct output, present only in training/evaluation data.
- **The lifecycle is iterative.** Poor results → collect more data / change features / retrain. That's normal iteration, not failure.

**Trap patterns to watch for:**

- **Predictions on new production data → inference, not evaluation.** Evaluation requires known labels to compare against.
- **"Model retrained with new data" → training,** not inference.
- **Dataset ≠ model.** The dataset is the raw examples; the model is the learned patterns.
- **99.9% training accuracy but 55% on test → overfitting** (memorization), not "a great model."
- **More data isn't always better** — noisy or irrelevant data, or an underfitting model, won't improve with volume.

---

## Common Misconceptions

- **Misconception:** Training and inference happen at the same time — the model keeps learning as it serves predictions. **Reality:** They're separate phases; during inference the parameters are fixed. Online learning exists but is rare and risky.
- **Misconception:** Evaluation just means "does the code run without errors." **Reality:** Evaluation measures how well predictions match known answers on a test set — it's about accuracy metrics, not debugging.
- **Misconception:** The training and test sets are the same data, just shuffled. **Reality:** They're separate, non-overlapping subsets; the test set is held out completely to give an honest score.
- **Misconception:** More training data always improves a model. **Reality:** Data quality and relevance matter as much as quantity; garbage in, garbage out.
- **Misconception:** 100% accuracy on training data means the best model. **Reality:** It's a red flag for overfitting — the model likely memorized noise and will generalize poorly.

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

**Q8.** A deployed fraud-detection model runs on new credit-card transactions in a production app, generating a fraud/not-fraud prediction for each. Which lifecycle phase is this?

- A) Training
- B) Evaluation
- C) Inference
- D) Feature engineering

**Q9.** A dataset of 100,000 transactions contains only 500 fraud cases. A model reports 99.5% accuracy but catches almost no fraud. Which metric best exposes this problem?

- A) Accuracy
- B) Recall
- C) Training loss
- D) Batch size

**Q10.** A medical screening model must minimize the number of sick patients it wrongly clears as healthy (false negatives). Which metric should the team prioritize?

- A) Precision
- B) Recall
- C) Specificity of the training set
- D) Learning rate

**Q11.** In a house-price dataset with columns for bedrooms, square footage, location, and sale price, which column is the **label**?

- A) Bedrooms
- B) Square footage
- C) Location
- D) Sale price

**Q12.** Six months after deployment, a model's accuracy drops because customer behavior changed following a pricing update. What is this called, and what is the standard response?

- A) Overfitting; add regularization
- B) Concept drift; collect new data and retrain
- C) Underfitting; add more features
- D) Data leakage; re-split the data

---

**Answers:**
1. B — No predefined labels/categories → unsupervised clustering
2. B — Large train/test gap is the classic sign of overfitting
3. C — Agent + environment + reward signal defines reinforcement learning
4. C — Predicting a continuous numeric value is regression
5. C — Validation is used for hyperparameter tuning and model selection
6. B — Learning rate is set before training (hyperparameter); weights/bias are learned
7. B — Synchronous, low-latency predictions require a real-time endpoint
8. C — Predicting on new production data with fixed parameters is inference (no learning occurs)
9. B — On imbalanced data, recall exposes missed positives that accuracy hides
10. B — Minimizing false negatives means maximizing recall
11. D — Sale price is the value being predicted (label); the rest are input features
12. B — A shift in the input-target relationship over time is concept drift; retrain on fresh data
