# Topic 9: AWS AI Managed Services

> Sources: [Amazon Q](https://aws.amazon.com/q/) | [Rekognition](https://docs.aws.amazon.com/rekognition/) | [Comprehend](https://docs.aws.amazon.com/comprehend/) | [Textract](https://docs.aws.amazon.com/textract/) | [Transcribe](https://docs.aws.amazon.com/transcribe/) | [Polly](https://docs.aws.amazon.com/polly/) | [Lex](https://docs.aws.amazon.com/lex/) | [Kendra](https://docs.aws.amazon.com/kendra/)
> Back to: [AWS AI Practitioner Study Guide](../index.md)

> **Exam Domains 2 & 3** — These are **pre-trained, fully managed AI services** ("AI services" layer). No ML expertise needed — call an API, get results. Knowing **which service does what** is heavily tested.

---

## Quick Revision (TL;DR)

| Phrase in the question | Service |
|------------------------|---------|
| Image / video / faces / objects / moderation | **Rekognition** |
| Scanned document / form / table / invoice / handwriting | **Textract** |
| Sentiment / entities / key phrases / language / PII in **text** | **Comprehend** |
| **Speech → text** (transcribe a call, captions) | **Transcribe** |
| **Text → speech** (read aloud, voice) | **Polly** |
| Translate **text** between languages | **Translate** |
| Chatbot / voice assistant / intent + slots | **Lex** |
| Intelligent enterprise **search** | **Kendra** |
| Enterprise **GenAI assistant** over company data | **Amazon Q Business** |
| AI **coding** assistant | **Amazon Q Developer** |
| Real-time **recommendations** | **Personalize** |
| Time-series **forecasting** | **Forecast** |
| Online **fraud** detection | **Fraud Detector** |
| **Human review** of ML predictions | **Augmented AI (A2I)** |
| Foundation models / GenAI | **Bedrock** |
| Build **custom** ML models | **SageMaker** |

**Never confuse these pairs:** Polly (text→speech) vs. Transcribe (speech→text) · Rekognition (images/video) vs. Textract (documents) · Comprehend (analyzes text) vs. Lex (holds a conversation) · Comprehend (understands meaning) vs. Translate (changes language).

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
- Processes **video frame by frame** for object/activity detection (Textract and Comprehend cannot analyze video)
- **Custom Labels** — train Rekognition to recognize your own domain-specific objects (e.g., your products, a rare bird species) with a small labeled dataset
- Every detection returns a **confidence score** (0–100) — apps set a threshold and route low-confidence results to manual review

**Use when:** "Analyze images or video," "detect faces," "moderate visual content."

> **Rekognition can detect text *in* an image, but for structured data from documents (forms/tables) use Textract.**

---

### Amazon Comprehend — Natural Language Processing (NLP)

- **Sentiment** analysis (positive/negative/neutral/mixed)
- **Entity** recognition (people, places, brands, dates)
- **Key phrase** extraction
- **Language** detection (Dominant Language)
- **PII** detection and redaction
- **Topic modeling** across large document collections
- **Custom classification & custom entity recognition** — train on your own labels
- Comprehend **understands the meaning** of static text; it does **not** hold a conversation (that is Lex) or change the language (that is Translate)

**Use when:** "Extract meaning/sentiment/entities/PII from **text**."

---

### Amazon Textract — Document Text & Data Extraction

- Extracts **text, handwriting, tables, and form** key-value pairs from scanned documents
- Goes beyond basic OCR — understands document **structure** (knows a "Total" box and its value)
- Returns structured **JSON** with the text and its location on the page
- Accepts PDF, TIFF, JPEG, PNG
- Specialized for invoices, receipts, IDs, tax forms, and lending documents
- Works only on **static documents** — not video

**Use when:** "Extract data from **documents/forms/PDFs/scans**."

> **Rekognition vs. Textract:** Rekognition = general images/video & scene text. **Textract = structured data from documents** (forms, tables).

---

### Amazon Transcribe — Speech-to-Text

- Converts **audio/speech into text** using Automatic Speech Recognition (**ASR**)
- Handles audio **files** and **live streams**
- Automatic language identification, **speaker diarization** (who spoke), automatic punctuation
- **Custom vocabulary** — add domain-specific words (e.g., medical/industry terms) to improve accuracy
- **PII redaction**; **Transcribe Medical** for clinical speech
- Output can flow directly to S3 or to Comprehend for sentiment analysis

**Use when:** "Convert **audio → text**," "caption/subtitle," "transcribe calls."

---

### Amazon Polly — Text-to-Speech

- Converts **text into lifelike speech** (opposite of Transcribe) using Text-to-Speech (**TTS**)
- **Standard voices** (faster, more robotic) vs. **Neural voices (NTTS)** (more natural, human-like intonation — higher cost per character)
- Many languages and voices
- **SSML** (Speech Synthesis Markup Language) tags control pronunciation, pauses, emphasis, and prosody
- Polly does **not** understand meaning — intonation comes from punctuation/SSML, not semantics
- Priced **per character** synthesized

**Use when:** "Convert **text → speech / audio / voice**."

---

### Amazon Lex — Conversational Chatbots

- Build **voice and text chatbots** (same tech as Alexa)
- Combines **ASR** (speech-to-text) + **NLU** (natural language understanding) — figures out the user's *intention*, not just words
- **Intents** (the user's goal, e.g., `BookFlight`), **utterances** (example phrases), and **slots** (required info, e.g., destination, date)
- Triggers **AWS Lambda** for fulfillment (query a database, place an order)
- Has **built-in ASR and TTS**, so a Lex voice bot does not separately need Transcribe + Polly
- Integrates with **Amazon Connect** (contact center) and can **fall back to a human agent** when confidence drops
- Each language requires its **own** bot version, utterances, and slots (not automatic)
- Priced **per request**

**Use when:** "Build a **chatbot / virtual agent / IVR**."

> **Lex vs. Transcribe + Polly:** Use **Lex** when you need a back-and-forth **dialogue with intent recognition**. Use **Transcribe** and **Polly** separately for one-way speech-to-text or text-to-speech processing.

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

### Amazon Translate — Neural Machine Translation

- Translates **text** between languages using **neural machine translation (NMT)** trained on billions of sentence pairs
- Considers whole sentences/paragraphs as a unit (far more natural than older word-by-word statistical translation)
- Input and output are **both text** — it does not process audio and does not change meaning, only language
- **Custom terminology** — force specific translations for brand names or technical terms
- Can translate documents stored in S3
- Priced **per character** translated

**Use when:** "Convert **text from one language to another**" / localize a website or user message.

> **Translate vs. Transcribe:** they sound alike but Translate changes the **language of text**; Transcribe converts **audio to text** in the same language.

---

## Service Customization Options

Most AI services work out of the box, but several can be tailored to your domain:

| Service | Customization | Purpose |
|---------|---------------|---------|
| **Rekognition** | Custom Labels | Recognize your own objects/brands |
| **Comprehend** | Custom Classification & Custom Entities | Classify/extract using your own labels |
| **Transcribe** | Custom Vocabulary | Improve accuracy for domain-specific terms |
| **Translate** | Custom Terminology | Force exact translation of brand/technical terms |
| **Polly** | SSML + Lexicons | Control pronunciation, pauses, emphasis |
| **Lex** | Intents, Utterances, Slots | Define the conversation your bot handles |

> **Exam note:** These services are **pre-trained** — you do **not** need to train them to get started. Customization is optional and uses far less data than building a model from scratch in SageMaker.

---

## Conversational AI Pipeline

The language services are often chained together. Know which service sits at each stage.

```
User speaks (audio)
    ↓  Amazon Transcribe (speech → text)   [or Lex built-in ASR]
    ↓  Amazon Translate (optional language conversion)
    ↓  Amazon Lex (intent + slot detection) → AWS Lambda (backend action)
    ↓  Amazon Polly (text → speech)         [or Lex built-in TTS]
User hears response (audio)
```

| Stage | Service | Note |
|-------|---------|------|
| Speech in | Transcribe / Lex ASR | Standalone transcript → Transcribe; dialogue → Lex |
| Translate | Translate | Only if crossing languages |
| Understand intent | Lex | Extracts intents and slots |
| Fulfillment | Lambda | Executes the action (DB query, order) |
| Speech out | Polly / Lex TTS | Neural voice for natural output |

> **Exam pattern:** "Speak German → turn to text → translate to English → read aloud" = **Transcribe → Translate → Polly**. Add **Lex** only when a back-and-forth conversation with intent is required.

**Common integrations:** all AI services are called via the **AWS SDK** (e.g., Python Boto3), triggered by **Lambda**, read/write **S3**, log to **CloudWatch**, and use **IAM roles** for permissions. Data is typically staged in **S3** before processing.

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

**Q8.** A travel app lets a user speak their destination in German. The app must turn the speech into text, translate it to English, then read the English result aloud. Which sequence of services is correct?

- A) Polly → Translate → Transcribe
- B) Transcribe → Translate → Polly
- C) Lex → Comprehend → Polly
- D) Translate → Transcribe → Polly

**Q9.** A team building a voice chatbot with Amazon Lex asks whether they must also call Amazon Transcribe and Amazon Polly separately for speech input and output. What is correct?

- A) Yes — Lex has no speech capability
- B) No — Lex has built-in ASR and TTS, so it can handle voice input and output natively
- C) Yes — Lex only supports text
- D) No — but only if Comprehend is added

**Q10.** A hospital wants to improve transcription accuracy for specialized medical terminology in recorded consultations. Which capability should they use?

- A) Polly SSML tags
- B) Translate custom terminology
- C) Transcribe custom vocabulary
- D) Rekognition custom labels

**Q11.** A retailer wants Amazon Rekognition to recognize their own specific product packaging that generic object detection misses. Which feature enables this?

- A) Rekognition Custom Labels
- B) Comprehend custom entities
- C) Textract queries
- D) Kendra connectors

**Q12.** A developer needs Amazon Polly to pause and emphasize certain words when reading a script. Which mechanism provides this control?

- A) Custom vocabulary
- B) SSML tags
- C) Intent slots
- D) Neural machine translation

**Q13.** An application must decide whether to route a Rekognition object-detection result to a human reviewer. Which returned value should it check?

- A) The intent name
- B) The confidence score
- C) The SSML tag
- D) The slot value

---

**Answers:**
1. B — Textract extracts structured text/tables/forms from documents
2. C — Rekognition performs image/video content moderation
3. B — Polly is text-to-speech
4. A — Comprehend does sentiment analysis and entity extraction on text
5. C — Amazon Q Business is the enterprise GenAI assistant over company data
6. B — Transcribe converts speech to text with diarization
7. B — Lex builds intent/slot-based voice and text chatbots
8. B — One-way pipeline: Transcribe (speech→text) → Translate (language) → Polly (text→speech)
9. B — Lex includes built-in ASR and TTS for native voice handling
10. C — Transcribe custom vocabulary improves accuracy for domain terms
11. A — Rekognition Custom Labels trains on your own object classes
12. B — SSML tags control pronunciation, pauses, and emphasis in Polly
13. B — The confidence score (0–100) drives human-review thresholds
