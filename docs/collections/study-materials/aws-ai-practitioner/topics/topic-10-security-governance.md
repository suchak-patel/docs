# Topic 10: Security, Compliance, and Governance

> Sources: [AWS AI Service Cards](https://aws.amazon.com/ai/responsible-ai/resources/) | [AWS IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) | [Bedrock Security](https://docs.aws.amazon.com/bedrock/latest/userguide/security.html) | [AWS Artifact](https://aws.amazon.com/artifact/) | [SageMaker Model Cards](https://docs.aws.amazon.com/sagemaker/latest/dg/model-cards.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domain 5 — Security, Compliance, and Governance for AI Solutions (14% of exam)**

---

## Quick Revision (TL;DR)

| Need | Service |
|------|---------|
| Control **who** can access a model/resource | **IAM** (least privilege, roles) |
| Keep AI traffic **off the public internet** | **VPC endpoints / PrivateLink** |
| Encrypt data **at rest** | **KMS** (customer-managed keys) |
| Encrypt data **in transit** | **TLS/HTTPS** |
| **Who did what, when** (API audit trail) | **CloudTrail** |
| Monitor metrics/logs/alarms | **CloudWatch** |
| Track **resource config** & compliance rules | **AWS Config** |
| Automate **compliance audit** evidence | **AWS Audit Manager** |
| Download AWS **compliance reports** (SOC/ISO/PCI) | **AWS Artifact** |
| Discover **PII in S3** | **Amazon Macie** |
| Responsible-AI transparency for AWS services | **AI Service Cards** |
| Governance docs for **your** models | **SageMaker Model Cards** |
| Content safety / PII filtering on GenAI | **Bedrock Guardrails** |

**Mental model:** CloudTrail is **reactive** (records events), Config is **proactive** (checks rules), Audit Manager is **organizational** (bundles evidence for auditors). Under the **Shared Responsibility Model**, AWS secures the cloud; **you** secure what's *in* it (IAM, data, config).

---

## In Plain English

Imagine a busy restaurant kitchen. The head chef (your AI model) turns out great dishes (predictions), but who makes sure the kitchen is clean, the recipes are followed, and nobody cut corners? That's the job of three "inspectors," and they map directly to the three AWS governance tools.

- **AWS CloudTrail** is the **security camera system**. It records every person who entered the kitchen, which door they used, and what they touched. If a batch of soup tastes wrong, you don't accuse anyone — you rewind the footage to see exactly who added the salt and when. CloudTrail logs every **API call**: who did what, when, from where.
- **AWS Config** is the **master recipe book that constantly checks every dish against the approved recipe**. If someone uses an ingredient that isn't on the approved list (an unauthorized configuration change, like making an S3 bucket public), Config rings an alarm. It tracks **resource configuration** and flags anything that drifts from your rules.
- **AWS Audit Manager** is the **health inspector who arrives with a pre-printed checklist** from the regulator (HIPAA, GDPR, SOC 2). It doesn't guess the rules — it walks through collecting **evidence** (pulled automatically from CloudTrail and Config) that each control is met, and bundles it into a report you can hand to the auditor.

Sitting above all of this is the **Shared Responsibility Model**: AWS secures the physical kitchen and appliances (security *of* the cloud), while you're responsible for how you use them — who has keys (IAM), whether ingredients are locked up (encryption), and how you run the line (security *in* the cloud).

---

## AWS Shared Responsibility Model

Security is a **shared responsibility** between AWS and the customer.

| Party | Responsible For | Examples |
|-------|-----------------|----------|
| **AWS** | Security **OF** the cloud | Physical data centers, hardware, managed service infrastructure |
| **Customer** | Security **IN** the cloud | IAM policies, data encryption config, access controls, prompt/data governance |

> For managed AI services (Bedrock, Rekognition, etc.), AWS handles more of the stack, but **you** still control access (IAM), data protection settings, and how the service is used.

---

## Identity and Access Management (IAM)

IAM controls **who** can do **what** on **which** AWS resources.

| Concept | Description |
|---------|-------------|
| **Users / Groups** | Human identities and collections of them |
| **Roles** | Temporary credentials assumed by services or users (preferred over long-lived keys) |
| **Policies** | JSON documents granting/denying permissions |
| **Least privilege** | Grant only the minimum permissions required — a core best practice |

**For AI workloads:**
- Restrict which users/apps can invoke specific Bedrock models or access Knowledge Bases
- Use IAM roles for SageMaker jobs and endpoints (SageMaker Role Manager simplifies this)
- Scope permissions per environment (dev/test/prod)

> **Exam tip:** "Control who can access a foundation model or ML resource" → **IAM**.

---

## Data Protection & Encryption

| Protection | Mechanism |
|------------|-----------|
| **Encryption at rest** | AWS KMS-managed keys encrypt stored data (S3, model artifacts, KB vector stores) |
| **Encryption in transit** | TLS/HTTPS for all API calls |
| **AWS KMS** | Create and control encryption keys; audit key usage |
| **Customer Managed Keys (CMK)** | Full control over key policy and rotation |

**Amazon Bedrock data privacy (important exam facts):**
- Your prompts and completions are **not used to train** the base foundation models
- Your data is **not shared** with model providers
- Data can be kept within your **VPC** and encrypted with your KMS keys
- Fine-tuned model copies are private to your account

---

## Network Security

| Control | Purpose |
|---------|---------|
| **Amazon VPC** | Isolate resources in a private network |
| **VPC Endpoints (AWS PrivateLink)** | Access Bedrock/SageMaker **privately** without traversing the public internet |
| **Security groups / NACLs** | Control inbound/outbound traffic |

> **Exam tip:** "Keep AI traffic off the public internet" → **VPC endpoints / PrivateLink**.

---

## Governance & Transparency Tools

### AWS AI Service Cards

**AI Service Cards** are AWS's **responsible AI documentation** for its AI services. Each card describes:
- Intended **use cases** and limitations
- **Responsible AI** design considerations
- **Performance** and fairness considerations
- Best practices for deployment

> A form of **transparency** — helps customers understand a service's capabilities and appropriate use. Example: an AI Service Card for Amazon Rekognition face matching.

### SageMaker Model Cards

Document **your own** models for governance:
- Intended use, risk rating, training details
- Evaluation results and limitations
- Supports audit and compliance workflows

| | AI Service Cards | SageMaker Model Cards |
|-|------------------|------------------------|
| **Who authors** | AWS (for AWS AI services) | You (for your custom models) |
| **Purpose** | Transparency about AWS AI services | Governance docs for your models |

---

## Monitoring, Auditing & Compliance Services

| Service | Purpose |
|---------|---------|
| **AWS CloudTrail** | Records **API calls** — who did what, when (auditing) |
| **Amazon CloudWatch** | Metrics, logs, alarms, dashboards (monitoring) |
| **AWS Config** | Tracks resource configuration changes / compliance rules |
| **AWS Artifact** | Self-service access to AWS **compliance reports** (SOC, ISO, PCI) |
| **AWS Audit Manager** | Continuously audit usage against compliance frameworks |
| **Amazon Macie** | Discovers and protects **sensitive data (PII)** in S3 |
| **AWS Trusted Advisor** | Best-practice checks (security, cost, performance) |

> **Exam tip:** "Who invoked this model / audit trail" → **CloudTrail**. "Monitor performance/metrics" → **CloudWatch**. "Get compliance certifications" → **AWS Artifact**. "Find PII in S3" → **Macie**.

---

## Governance & Auditability Deep Dive (CloudTrail vs. Config vs. Audit Manager)

These three services are the core of AI governance and a favorite exam trap. Learn the one-sentence purpose of each.

| Service | One-line purpose | Nature |
|---------|------------------|--------|
| **CloudTrail** | Records every **API call** — who did what, when, from where | **Reactive** (records after the fact) |
| **AWS Config** | Records **resource configuration** changes and evaluates them against rules | **Proactive** (checks compliance rules) |
| **AWS Audit Manager** | Automates **audit evidence** collection from CloudTrail + Config | **Organizational** (bundles for auditors) |

### AWS CloudTrail

- Logs the **who / what / when / from where** of every API call
- **Management events** (create/delete resources) logged by default; **data events** (reading an object inside an S3 bucket) must be **explicitly enabled**
- **Event history** shows the last **90 days**; create a **trail** to store logs indefinitely in S3, or use **CloudTrail Lake** for long-term analytics
- **CloudTrail Insights** flags unusual activity (e.g., mass download at 3 AM)
- Query logs with **Amazon Athena** (SQL over the log files)

> **Trap:** CloudTrail records *who accessed* an object (with data events on); Config does **not** log object reads.

### AWS Config

- Tracks the **configuration state** of resources over time (a "configuration item" per point in time)
- **Config rules** (AWS-managed or custom) enforce policies, e.g., `s3-bucket-public-read-prohibited`, encryption enabled, required tags present
- Flags **non-compliance** (configuration drift) and can alert via SNS
- Does **not** auto-fix by itself — needs **auto-remediation** (e.g., a Systems Manager document) configured separately

> **Trap:** "A security group was opened to the public; alert when config deviates from baseline" → **Config** (CloudTrail only tells you *who* made the change).

### AWS Audit Manager

- Continuously collects **evidence** to prepare for external audits
- Provides **pre-built frameworks** for common standards (HIPAA, GDPR, SOC 2, PCI DSS)
- A **control** is a requirement (e.g., "data encrypted at rest"); **evidence** is the proof (a CloudTrail log or Config snapshot)
- Automatically maps evidence from CloudTrail and Config to controls; generates shareable reports
- Is a **reporting/evidence** tool — it does **not** block actions

> **Audit Manager vs. AWS Artifact:** Audit Manager collects **your own** environment's evidence for audits; **Artifact** provides **AWS's** pre-built compliance reports (SOC, ISO, PCI). "Prepare for a SOC 2 audit by collecting evidence from your workloads" → **Audit Manager**; "download AWS's SOC 2 report" → **Artifact**.

> **Multi-service scenarios:** "Know *who* modified a resource AND confirm it now meets a rule" → needs **both** CloudTrail (who) **and** Config (rule). Watch for a distractor like **GuardDuty** (threat detection) or **CloudWatch Logs** (application logs, not API audit).

---

## Amazon Bedrock Guardrails (Governance for GenAI)

Guardrails enforce safety and compliance policies on GenAI applications:

| Filter | Purpose |
|--------|---------|
| **Content filters** | Block hate, violence, sexual, insults, misconduct |
| **Denied topics** | Block defined subject areas |
| **Word filters** | Block specific words/phrases |
| **Sensitive information filters** | Block/mask PII |
| **Contextual grounding checks** | Detect hallucination / ungrounded output |

> See [topic-01-amazon-bedrock.md](topic-01-amazon-bedrock.md) for full Guardrails coverage.

---

## Data Governance & Lifecycle

| Concept | Description |
|---------|-------------|
| **Data lineage** | Track origin and transformations of data |
| **Data classification** | Label data by sensitivity (public, internal, confidential, PII) |
| **Data retention** | Policies for how long data is kept |
| **Data residency / sovereignty** | Keep data within specific regions to meet regulations |
| **Anonymization / masking** | Remove or obscure PII before use |

---

## Compliance Frameworks & Standards (Awareness)

| Framework | Domain |
|-----------|--------|
| **GDPR** | EU data privacy |
| **HIPAA** | US healthcare data |
| **SOC 1/2/3** | Service organization controls |
| **ISO 27001** | Information security management |
| **PCI DSS** | Payment card data |

> Customers access proof of AWS compliance via **AWS Artifact**.

---

## Secure AI — Best Practices Summary

- Apply **least privilege** IAM to all AI resources
- **Encrypt** data at rest (KMS) and in transit (TLS)
- Use **VPC endpoints / PrivateLink** to avoid public internet exposure
- Apply **Guardrails** to GenAI apps for content safety and PII protection
- **Monitor** with CloudWatch; **audit** with CloudTrail
- Detect sensitive data with **Macie**
- Document models with **Model Cards**; review **AI Service Cards**
- Follow the **Shared Responsibility Model**

---

## How AIF-C01 Actually Tests This

Security and governance is 14% of the exam. Questions give a scenario plus three tools and one distractor; you pick the right one. The whole game is knowing each service's **one-sentence purpose**.

**Exam topics you must master:**

- **One-liners:** CloudTrail = records API activity (who/what/when/where). Config = records resource configuration and checks it against rules. Audit Manager = automates audit evidence collection from CloudTrail + Config.
- **Who accessed what → CloudTrail** (with **data events** enabled for object-level reads).
- **Configuration drift / "alert when a security group deviates from baseline" → Config.**
- **"Prepare for a SOC 2 audit by collecting evidence from our workloads" → Audit Manager.**
- **Shared Responsibility:** the customer owns IAM, data protection, and configuration; AWS owns the physical/infrastructure layer.
- **Bedrock data privacy:** prompts/completions aren't used to train base models; keep traffic private with **VPC endpoints/PrivateLink**; encrypt with **KMS**.

**Trap patterns to watch for:**

- **Config vs. CloudTrail:** Config does **not** log who *read* an object — that's a CloudTrail data event. Config tracks the resource's *configuration state*.
- **Audit Manager vs. AWS Artifact:** Audit Manager collects **your** environment's evidence; Artifact only serves **AWS's** pre-built compliance reports (SOC/ISO/PCI).
- **CloudTrail vs. CloudWatch Logs:** API audit trail = CloudTrail; application/system logs = CloudWatch Logs.
- **Distractors:** GuardDuty (threat detection) and Artifact often appear as wrong options.
- **Multi-service scenarios:** "know *who* changed it AND confirm it now meets a rule" needs **both** CloudTrail and Config.
- **CloudTrail retention:** 90-day event history is not a hard cap — a **trail** stores logs in S3 indefinitely.

---

## Common Misconceptions

- **Misconception:** AWS Config records every time someone reads a file from an S3 bucket. **Reality:** Config records **configuration** changes (who *can* access, encryption settings), not object reads. Object access is a CloudTrail data event.
- **Misconception:** Audit Manager actively blocks non-compliant actions. **Reality:** It's a reporting/evidence tool — it documents compliance; it doesn't enforce or block.
- **Misconception:** CloudTrail only keeps 90 days of logs, then they're gone. **Reality:** The console **event history** shows 90 days, but a **trail** delivers logs to S3 for indefinite retention (and CloudTrail Lake for analytics).
- **Misconception:** AWS Config automatically fixes non-compliant resources. **Reality:** Config **detects and reports**; auto-remediation must be configured separately (e.g., a Systems Manager document).
- **Misconception:** One tool covers governance. **Reality:** Governance is layered — CloudTrail (actions) + Config (configuration) + Audit Manager (audit evidence) work together.

---

## Knowledge Test — Security, Compliance, and Governance

**Q1.** A compliance officer needs an audit trail of every API call made to Amazon Bedrock, including who invoked which model. Which service provides this?

- A) Amazon CloudWatch
- B) AWS CloudTrail
- C) AWS Config
- D) Amazon Macie

**Q2.** A healthcare company must ensure that traffic to their SageMaker endpoints never travels over the public internet. Which approach should they use?

- A) IAM policies
- B) KMS encryption
- C) VPC endpoints (AWS PrivateLink)
- D) CloudWatch alarms

**Q3.** Which statement about Amazon Bedrock data privacy is correct?

- A) Prompts are used to train the base foundation models
- B) Customer data is shared with third-party model providers
- C) Customer prompts and completions are not used to train the base models
- D) Fine-tuned models are shared across all AWS customers

**Q4.** A team needs to download AWS's SOC 2 and ISO 27001 compliance reports. Which service provides them?

- A) AWS Artifact
- B) AWS Trusted Advisor
- C) Amazon Inspector
- D) AWS Config

**Q5.** What is the primary purpose of an AWS AI Service Card?

- A) To store encryption keys for AI services
- B) To provide responsible-AI transparency about an AWS AI service's intended use and limitations
- C) To grant IAM permissions to a model
- D) To monitor model drift in production

**Q6.** A company wants to automatically discover and protect personally identifiable information stored in Amazon S3. Which service should they use?

- A) Amazon Macie
- B) Amazon Comprehend
- C) AWS CloudTrail
- D) Amazon Rekognition

**Q7.** Under the AWS Shared Responsibility Model, which task is the **customer's** responsibility when using Amazon Bedrock?

- A) Securing the physical data center hardware
- B) Patching the underlying model-hosting infrastructure
- C) Configuring IAM permissions and data access controls
- D) Maintaining the availability of the Bedrock service

**Q8.** A security group protecting an AI model's EC2 instance was changed to expose a port publicly. The team wants to be automatically flagged whenever resource configuration drifts from a secure baseline. Which service should they use?

- A) AWS CloudTrail
- B) AWS Config
- C) Amazon CloudWatch
- D) AWS Artifact

**Q9.** A company is preparing for a SOC 2 audit and needs to automatically collect evidence from its own AWS workloads that security controls are met. Which service fits best?

- A) AWS Artifact
- B) AWS Audit Manager
- C) AWS CloudTrail
- D) Amazon Macie

**Q10.** A regulator asks a company to prove which IAM user read objects from an S3 bucket of AI training data on a specific date. What must be enabled to capture this?

- A) AWS Config rules
- B) CloudTrail **data events**
- C) CloudWatch alarms
- D) Macie classification jobs

**Q11.** Which statement about CloudTrail retention is correct?

- A) Logs are permanently deleted after 90 days with no option to retain them
- B) The event history keeps 90 days, but a trail can store logs indefinitely in S3
- C) CloudTrail retains all logs for exactly 7 years automatically
- D) Retention is managed only by AWS Config

**Q12.** A team needs both to identify *who* changed a resource and to confirm the resource now complies with a rule. Which combination is correct?

- A) CloudTrail only
- B) Config only
- C) CloudTrail (who) + Config (rule)
- D) Audit Manager only

---

**Answers:**
1. B — CloudTrail records API activity for auditing
2. C — VPC endpoints / PrivateLink keep traffic off the public internet
3. C — Bedrock does not use customer prompts/completions to train base models
4. A — AWS Artifact provides compliance reports
5. B — AI Service Cards provide responsible-AI transparency documentation
6. A — Amazon Macie discovers and protects PII in S3
7. C — Customers manage IAM and data access (security "in" the cloud)
8. B — Config detects configuration drift against rules (CloudTrail only says who changed it)
9. B — Audit Manager collects your own environment's evidence; Artifact only serves AWS's reports
10. B — Object-level reads require CloudTrail data events (off by default)
11. B — 90-day event history; a trail retains logs indefinitely in S3
12. C — CloudTrail identifies the actor; Config validates the compliance rule
