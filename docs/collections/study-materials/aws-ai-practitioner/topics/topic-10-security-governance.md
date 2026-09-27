# Topic 10: Security, Compliance, and Governance

> Sources: [AWS AI Service Cards](https://aws.amazon.com/ai/responsible-ai/resources/) | [AWS IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) | [Bedrock Security](https://docs.aws.amazon.com/bedrock/latest/userguide/security.html) | [AWS Artifact](https://aws.amazon.com/artifact/) | [SageMaker Model Cards](https://docs.aws.amazon.com/sagemaker/latest/dg/model-cards.html)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domain 5 — Security, Compliance, and Governance for AI Solutions (14% of exam)**

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

---

**Answers:**
1. B — CloudTrail records API activity for auditing
2. C — VPC endpoints / PrivateLink keep traffic off the public internet
3. C — Bedrock does not use customer prompts/completions to train base models
4. A — AWS Artifact provides compliance reports
5. B — AI Service Cards provide responsible-AI transparency documentation
6. A — Amazon Macie discovers and protects PII in S3
7. C — Customers manage IAM and data access (security "in" the cloud)# Topic 10: Security, Compliance, and Governance

This page is the new collection location for the security, compliance, and governance study topic.

The detailed content will be migrated here from the legacy `aws-ai-practitioner/` source page.
