# Topic 9: AWS AI Managed Services

> Sources: [Amazon Q](https://aws.amazon.com/q/) | [Rekognition](https://docs.aws.amazon.com/rekognition/) | [Comprehend](https://docs.aws.amazon.com/comprehend/) | [Textract](https://docs.aws.amazon.com/textract/) | [Transcribe](https://docs.aws.amazon.com/transcribe/) | [Polly](https://docs.aws.amazon.com/polly/) | [Lex](https://docs.aws.amazon.com/lex/) | [Kendra](https://docs.aws.amazon.com/kendra/)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domains 2 & 3** — These are **pre-trained, fully managed AI services** ("AI services" layer). No ML expertise needed — call an API, get results. Knowing **which service does what** is heavily tested.

---

## The AWS AI/ML Stack (3 Layers)

| Layer | Description | Examples |
|-------|-------------|----------|
| **AI Services** | Pre-trained, ready-to-use APIs — no ML skills needed | Rekognition, Comprehend, Textract, Polly, Lex, Kendra, Q |
| **ML Services** | Build/train/deploy custom models | Amazon SageMaker |
| **ML Frameworks / Infrastructure** | Low-level compute & frameworks | EC2 (GPU), Trainium, Inferentia, TensorFlow/PyTorch |

> **Exam tip:** If a scenario needs a common capability (detect objects, transcribe audio, extract text) **without** building a model, the answer is an **AI service**, not SageMaker.

---

## Service-by-Service Reference

### Amazon Q — Generative AI Assistant

| Variant | Purpose |
|---------|---------|
| **Amazon Q Business** | Enterprise GenAI assistant — answers questions over your company data (S3, SharePoint, etc.), with citations and access controls |
| **Amazon Q Developer** | AI coding assistant — code generation, debugging, AWS guidance (formerly CodeWhisperer) |
| **Amazon Q in QuickSight / Connect** | Embedded GenAI for BI dashboards and contact centers |

**Use when:** "Enterprise chatbot over internal documents" or "AI pair programmer."

---

### Amazon Rekognition — Image & Video Analysis (Computer Vision)

- Object, scene, and activity detection
- **Facial** analysis, comparison, and recognition
- **Content moderation** (unsafe/inappropriate content)
- Text-in-image detection (OCR of scenes)
- Celebrity recognition, PPE detection, custom labels

**Use when:** "Analyze images or video," "detect faces," "moderate visual content."

---

### Amazon Comprehend — Natural Language Processing (NLP)

- **Sentiment** analysis (positive/negative/neutral/mixed)
- **Entity** recognition (people, places, brands)
- **Key phrase** extraction
- **Language** detection
- **PII** detection and redaction
- **Topic modeling**
- Custom classification & custom entity recognition

**Use when:** "Extract meaning/sentiment/entities/PII from **text**."

---

### Amazon Textract — Document Text & Data Extraction

- Extracts **text, handwriting, tables, and form** key-value pairs from scanned documents
- Goes beyond OCR — understands document **structure**
- Specialized for invoices, receipts, IDs, and lending documents

**Use when:** "Extract data from **documents/forms/PDFs/scans**."

> **Rekognition vs. Textract:** Rekognition = general images/video & scene text. **Textract = structured data from documents** (forms, tables).

---

### Amazon Transcribe — Speech-to-Text

- Converts **audio/speech into text**
- Automatic language identification, speaker diarization (who spoke)
- Custom vocabulary, **PII redaction**
- **Transcribe Medical** for clinical speech

**Use when:** "Convert **audio → text**," "caption/subtitle," "transcribe calls."

---

### Amazon Polly — Text-to-Speech

- Converts **text into lifelike speech** (opposite of Transcribe)
- Many languages, voices, and **neural TTS** voices
- SSML for pronunciation/prosody control

**Use when:** "Convert **text → speech / audio / voice**."

---

### Amazon Lex — Conversational Chatbots

- Build **voice and text chatbots** (same tech as Alexa)
- Automatic Speech Recognition (ASR) + Natural Language Understanding (NLU)
- **Intents**, **utterances**, and **slots** for dialogue
- Integrates with Lambda for fulfillment; used in contact centers (Amazon Connect)

**Use when:** "Build a **chatbot / virtual agent / IVR**."

> **Lex vs. Amazon Q:** Lex = build a **structured intent-based** bot you design. Amazon Q = ready-made **generative** assistant over your data.

---

### Amazon Kendra — Intelligent Enterprise Search

- **ML-powered semantic search** across enterprise content
- Natural-language queries return precise answers, not just links
- Many connectors (S3, SharePoint, Salesforce, databases)
- Often used as a **retriever for RAG**

**Use when:** "Intelligent **search** across enterprise documents."

> **Kendra vs. Amazon Q Business:** Kendra = search engine (retrieval). Q Business = full generative assistant (often uses Kendra-style retrieval under the hood).

---

## Other Notable AI Services

| Service | Purpose |
|---------|---------|
| **Amazon Translate** | Neural machine **translation** between languages |
| **Amazon Personalize** | Real-time **recommendations** (like Amazon.com) |
| **Amazon Forecast** | Time-series **forecasting** (demand, inventory) |
| **Amazon Fraud Detector** | Detect online **fraud** using ML |
| **Amazon Textract + Comprehend** | Combined document processing pipelines |
| **Amazon Augmented AI (A2I)** | Add **human review** to ML predictions |
| **Amazon Bedrock** | Managed **foundation models** / GenAI — see [topic-01-amazon-bedrock.md](topic-01-amazon-bedrock.md) |

---

## Quick "Which Service?" Decision Table

| Need | Service |
|------|---------|
| Detect objects/faces in **images/video** | **Rekognition** |
| Extract **text/tables/forms from documents** | **Textract** |
| Sentiment / entities / PII from **text** | **Comprehend** |
| **Audio → text** | **Transcribe** |
| **Text → audio/speech** | **Polly** |
| Build a **chatbot** (voice/text) | **Lex** |
| **Search** enterprise documents | **Kendra** |
| Enterprise **GenAI assistant** over company data | **Amazon Q Business** |
| AI **coding** assistant | **Amazon Q Developer** |
| **Translate** languages | **Translate** |
| **Recommendations** | **Personalize** |
| **Forecasting** demand | **Forecast** |
| **Fraud** detection | **Fraud Detector** |
| Use/customize **foundation models** | **Bedrock** |
| Build **custom** ML models | **SageMaker** |

---

## Knowledge Test — AWS AI Managed Services

**Q1.** A company needs to extract key-value pairs from thousands of scanned invoices and tax forms. Which service is best?

- A) Amazon Rekognition
- B) Amazon Textract
- C) Amazon Comprehend
- D) Amazon Kendra

**Q2.** A media company wants to automatically detect inappropriate content in user-uploaded videos. Which service should they use?

- A) Amazon Transcribe
- B) Amazon Comprehend
- C) Amazon Rekognition
- D) Amazon Polly

**Q3.** An application must convert written product descriptions into natural-sounding audio for accessibility. Which service fits?

- A) Amazon Transcribe
- B) Amazon Polly
- C) Amazon Lex
- D) Amazon Translate

**Q4.** A team wants to analyze customer support emails to determine sentiment and extract mentioned product names. Which service is best?

- A) Amazon Comprehend
- B) Amazon Textract
- C) Amazon Kendra
- D) Amazon Rekognition

**Q5.** A business wants an enterprise assistant that answers employee questions using internal wikis and documents, with source citations. Which service is the best fit?

- A) Amazon Lex
- B) Amazon Polly
- C) Amazon Q Business
- D) Amazon Transcribe

**Q6.** Which service converts recorded call-center **audio into text** with speaker identification?

- A) Amazon Polly
- B) Amazon Transcribe
- C) Amazon Comprehend
- D) Amazon Lex

**Q7.** A developer wants to build a voice-and-text chatbot with defined intents and slots for booking appointments. Which service should they use?

- A) Amazon Kendra
- B) Amazon Lex
- C) Amazon Rekognition
- D) Amazon Q Developer

---

**Answers:**
1. B — Textract extracts structured text/tables/forms from documents
2. C — Rekognition performs image/video content moderation
3. B — Polly is text-to-speech
4. A — Comprehend does sentiment analysis and entity extraction on text
5. C — Amazon Q Business is the enterprise GenAI assistant over company data
6. B — Transcribe converts speech to text with diarization
7. B — Lex builds intent/slot-based voice and text chatbots# Topic 09: AWS AI Managed Services

This page is the new collection location for the AWS AI managed services study topic.

The detailed content will be migrated here from the legacy `aws-ai-practitioner/` source page.
