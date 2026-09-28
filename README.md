# BodhaQ

> **Study → Assess → Evaluate → Understand → Identify Weakness → Practice → Improve**

BodhaQ is an AI-powered learning workspace that transforms study materials and topics into interactive learning experiences.

It combines document understanding, Retrieval-Augmented Generation (RAG), AI-generated learning content, assessments, deterministic evaluation, resume-based preparation, targeted practice, and an interactive coding workspace into one learning platform.

---

## 🚀 Overview

BodhaQ helps students:

- Understand study material
- Ask questions about their own documents
- Generate and take assessments
- Identify learning gaps
- Practice weak areas
- Prepare from their resume
- Practice coding
- Track recent learning activity

### Core Learning Loop

```text
STUDY
  ↓
ASSESS
  ↓
EVALUATE
  ↓
UNDERSTAND
  ↓
IDENTIFY WEAKNESS
  ↓
PRACTICE
  ↓
IMPROVE
```

---

# ✨ Features

## 📚 Study Materials

Supported formats:

- PDF
- PPTX
- DOCX
- Topic-based learning without a document

Documents are extracted, chunked, embedded, indexed, and made available to the learning and RAG pipelines.

---

## 🧠 AI Learning

Provide a topic and BodhaQ can generate structured learning content including:

- Definition
- Key concepts
- Examples
- Important points

Gemini is used for AI-generated learning content.

---

# 🔎 Retrieval-Augmented Generation (RAG)

BodhaQ implements a document-grounded RAG pipeline.

```text
Document
   ↓
Text Extraction
   ↓
Chunking
   ↓
Embedding Generation
   ↓
ChromaDB
   ↓
Similarity Retrieval
   ↓
Relevant Context
   ↓
Gemini
   ↓
Grounded Response
```

### RAG Components

- PyMuPDF
- python-pptx
- python-docx
- Document chunking
- Gemini Embeddings
- ChromaDB
- Similarity retrieval
- Gemini response generation

### RAG is used for

- Document-based doubts
- Document-grounded quiz generation
- Targeted practice from study materials

### Embedding model

```text
gemini-embedding-001
```

Generated ChromaDB data is runtime data and is intentionally excluded from Git.

---

# 💬 Doubt Solving

BodhaQ supports questions about study material.

### Document mode

```text
Question
   ↓
Query Embedding
   ↓
ChromaDB Retrieval
   ↓
Relevant Document Chunks
   ↓
Gemini
   ↓
Answer
```

### Topic mode

Questions can also be asked without a document using general AI knowledge.

---

# 📝 AI Assessments

BodhaQ can generate quizzes from:

- Topics
- Uploaded documents
- Targeted weak areas
- Resume items

Quiz generation uses Gemini, while quiz evaluation is performed deterministically by the backend.

```text
Generate Quiz
     ↓
Attempt Questions
     ↓
Submit
     ↓
Deterministic Evaluation
     ↓
Score
     ↓
Topic Analysis
     ↓
Learning Gaps
```

Correct answers remain on the backend during quiz generation.

---

# 📊 Learning Gaps

BodhaQ identifies learning gaps from completed assessments.

| Score | Status |
|---:|---|
| < 60% | Needs Practice |
| 60–79% | Improving |
| ≥ 80% | Learned |

Learning gaps are calculated from quiz performance rather than being decided by the LLM.

Assessment-specific learning gaps remain associated with their respective assessments.

---

# 🎯 Targeted Practice

Targeted practice can be generated from:

- A specific topic
- A weak area from an assessment
- A document using RAG

The existing quiz generation and evaluation infrastructure is reused.

---

# 📄 Resume Prep

Users can upload:

- PDF resume
- DOCX resume

BodhaQ extracts structured information such as:

- Skills
- Projects
- Certifications

### Workflow

```text
Upload Resume
      ↓
Extract Resume Content
      ↓
Structure Resume
      ↓
Skills / Projects / Certifications
      ↓
Select Item
      ↓
Configure Assessment
      ↓
Generate Quiz
      ↓
Submit
      ↓
Evaluate
      ↓
Track Progress
```

A new resume replaces the active resume version rather than mixing items between resume versions.

---

# 💻 Coding Workspace

BodhaQ includes a dedicated coding workspace designed as a learning environment rather than simply a problem library.

```text
Coding
   ↓
Code
   ├── AI Learn Code
   └── IDE
```

### AI Learn Code

Users can provide:

- A problem name
- A complete problem statement

Optional:

- Constraints
- Sample test case

The intended workspace includes:

- Problem statement
- Input/output details
- Constraints
- Examples
- Difficulty/topics
- Code editor
- Public tests
- Hidden tests
- Custom input
- Run
- Submit
- Explain Code

### IDE

The IDE allows users to solve problems independently.

Focus Mode is designed to keep AI assistance disabled while the user is solving independently.

When Focus Mode is disabled, AI analysis can provide:

- Explain
- Create Test Cases
- Improve Code

AI analyzes the code currently present in the editor.

---

# 🧪 Code Evaluation

Code execution is designed around isolated execution rather than directly executing arbitrary user code inside the FastAPI process.

```text
Frontend
   ↓
FastAPI
   ↓
Code Execution Service
   ↓
Isolated Runtime
   ↓
Java / Python
   ↓
Execution Result
   ↓
Deterministic Evaluation
   ↓
Frontend
```

The execution environment must enforce:

- Time limits
- Memory limits
- Output limits
- Process limits
- Temporary workspace cleanup
- Filesystem isolation
- Network restrictions

User code must not access:

- `.env`
- Gemini API keys
- SQLite database
- ChromaDB
- Uploaded study materials
- Internal application files
- Unrestricted host resources

> Code execution and its isolation are still part of the active development and hardening process. Production security should not be assumed until the complete execution environment has been verified.

---

# 💾 Client-Side Persistence

The current MVP uses browser `localStorage` for selected client-side persistence.

It supports areas such as:

- Recent quiz history
- Resume quiz progress
- Resume assessment state
- Learning-gap information
- Coding saved-code state

Quiz history is limited to the most recent five completed quizzes.

### Limitation

`localStorage` is:

- Browser-specific
- Device-specific
- Not synchronized between devices
- Lost if site data is cleared

A future authenticated version can move persistent user data to a backend database.

---

# 🏗️ Architecture

```text
                         ┌─────────────────────┐
                         │      React UI       │
                         │     Vite + JS       │
                         └──────────┬──────────┘
                                    │
                              REST APIs
                                    │
                         ┌──────────▼──────────┐
                         │       FastAPI       │
                         │       Backend       │
                         └──────────┬──────────┘
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
              ▼                     ▼                     ▼
       ┌─────────────┐       ┌─────────────┐       ┌─────────────┐
       │   Gemini    │       │  ChromaDB   │       │   SQLite    │
       │     AI      │       │    RAG      │       │  Metadata   │
       └─────────────┘       └─────────────┘       └─────────────┘
              │                     ▲
              │                     │
              └──── Embeddings ─────┘
```

---

# 🛠️ Technology Stack

### Frontend

- React
- Vite
- JavaScript
- CSS
- Monaco Editor

### Backend

- Python
- FastAPI
- Pydantic
- Uvicorn

### AI

- Google Gemini
- Gemini Embeddings

### RAG

- ChromaDB
- Embeddings
- Document chunking
- Similarity retrieval

### Document Processing

- PyMuPDF
- python-docx
- python-pptx

### Storage

- SQLite
- Browser localStorage

### Coding

- Monaco Editor
- Isolated code execution architecture
- Java
- Python

---

# 📁 Project Structure

```text
BodhaQ/
│
├── backend/
│   ├── app/
│   │   ├── ingestion/
│   │   │   ├── chunker.py
│   │   │   ├── docx_loader.py
│   │   │   ├── pdf_loader.py
│   │   │   └── pptx_loader.py
│   │   │
│   │   ├── models/
│   │   │   ├── requests.py
│   │   │   └── responses.py
│   │   │
│   │   ├── rag/
│   │   │   ├── embeddings.py
│   │   │   ├── retriever.py
│   │   │   └── vector_store.py
│   │   │
│   │   ├── routes/
│   │   │   ├── coding.py
│   │   │   ├── documents.py
│   │   │   ├── doubts.py
│   │   │   ├── health.py
│   │   │   ├── learning.py
│   │   │   ├── quiz.py
│   │   │   ├── resume.py
│   │   │   └── settings.py
│   │   │
│   │   ├── services/
│   │   │   ├── code_execution_service.py
│   │   │   ├── document_service.py
│   │   │   ├── evaluation_service.py
│   │   │   ├── gemini_service.py
│   │   │   ├── quiz_service.py
│   │   │   ├── rag_service.py
│   │   │   └── resume_service.py
│   │   │
│   │   └── utils/
│   │       └── json_parser.py
│   │
│   ├── .env.example
│   ├── requirements.txt
│   └── ...
│
├── frontend/
│   ├── src/
│   │   ├── api/
│   │   ├── components/
│   │   ├── pages/
│   │   └── utils/
│   │
│   ├── public/
│   ├── package.json
│   ├── package-lock.json
│   └── vite.config.js
│
├── .gitignore
└── README.md
```

---

# ⚙️ Local Development

## Prerequisites

Install:

- Python 3.14+
- Node.js
- npm
- Git

For coding execution, the required isolated execution environment must also be configured and verified before production use.

---

## 🔧 Backend Setup

```bash
cd backend
```

Create a virtual environment:

```bash
python -m venv venv
```

Windows:

```cmd
venv\Scriptsctivate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create:

```text
backend/.env
```

Add:

```env
GEMINI_API_KEY=your_gemini_api_key
```

Do not commit `.env`.

Start the backend:

```bash
uvicorn app.main:app --reload
```

API:

```text
http://localhost:8000
```

FastAPI documentation:

```text
http://localhost:8000/docs
```

---

## 🎨 Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The Vite development server will display the local URL.

---

# 🔐 Environment Variables

Never commit real API keys.

The backend uses:

```env
GEMINI_API_KEY=your_gemini_api_key
```

A safe template is provided in:

```text
backend/.env.example
```

The following are intentionally excluded from Git:

```text
.env
venv/
node_modules/
dist/
backend/data/
backend/uploads/
ChromaDB runtime data
SQLite databases
```

---

# 🔒 Security Principles

### API keys

Gemini credentials remain on the backend.

### Quiz answers

Correct quiz answers remain server-side during quiz generation.

### RAG data

Generated vector-store data is runtime data rather than source code.

### User uploads

Uploaded documents are runtime/user data and are not committed to Git.

### Code execution

Arbitrary code must not be executed directly through Python `exec()`, `eval()`, or unrestricted host subprocess execution.

Code execution should occur inside an appropriately isolated runtime.

---

# 🧪 Testing

Backend API test-case documentation:

```text
backend/BodhaQ_Backend_API_Test_Cases.txt
```

Testing should cover:

- Health endpoints
- Learning generation
- Document upload
- Document retrieval
- RAG retrieval
- Doubt solving
- Quiz generation
- Quiz submission
- Learning gaps
- Targeted practice
- Resume processing
- Resume assessments
- Coding APIs
- Error handling
- API-key handling
- Execution isolation

Frontend testing should cover:

- Navigation
- Study workflow
- Document workflow
- Doubts
- Quiz generation
- Quiz submission
- Learning gaps
- Resume preparation
- Coding workspace
- Persistence
- Error states
- Responsive layouts

---

# 🚧 Current Development Status

BodhaQ is currently under active development.

- [x] React + Vite frontend
- [x] FastAPI backend
- [x] Gemini integration
- [x] PDF/PPTX/DOCX ingestion
- [x] Document chunking
- [x] Gemini embeddings
- [x] ChromaDB vector storage
- [x] RAG retrieval
- [x] Document-based doubts
- [x] AI learning content
- [x] AI quiz generation
- [x] Deterministic quiz evaluation
- [x] Learning-gap analysis
- [x] Targeted practice
- [x] Resume extraction workflow
- [x] Resume-based assessments
- [x] Client-side learning persistence
- [x] Coding workspace foundation
- [x] Monaco Editor integration
- [ ] Complete coding execution and isolation hardening
- [ ] Complete AI coding workflow
- [ ] Final production security review
- [ ] Production deployment

---

# 🗺️ Development Roadmap

## Phase 1 — Core Learning Platform

- Document ingestion
- Topic learning
- RAG
- Doubt solving
- Quiz generation
- Evaluation
- Learning gaps
- Targeted practice

## Phase 2 — Resume Preparation

- Resume extraction
- Structured resume items
- Resume assessments
- Persistent preparation queue
- Progress tracking

## Phase 3 — Coding Workspace

- Monaco IDE
- AI Learn Code
- Java/Python execution
- Public/private tests
- Custom tests
- Deterministic judging
- Explain Code
- Create Test Cases
- Improve Code
- Saved coding work

## Phase 4 — Production Hardening

- Secure execution isolation
- Security testing
- API hardening
- Error handling
- Performance testing
- Deployment
- Monitoring
- Documentation
- Final regression testing

---

# 🎯 Design Principles

### AI assists; deterministic systems evaluate

The LLM generates explanations, questions, and suggestions.

The backend determines quiz scores and test results.

### RAG grounds document-based interactions

When a user asks about uploaded material, relevant document chunks are retrieved before generating the response.

### User code is never silently modified

AI coding assistance explains or proposes improvements, while the user's code remains under the user's control.

### Features remain modular

Learning gaps, resume preparation, coding progress, and other feature-specific state remain logically separated.

### Security before convenience

User-provided code and uploaded documents are treated as untrusted input.

---

# 📌 Important MVP Limitations

The current MVP intentionally does not include:

- User authentication
- Multi-user accounts
- Cross-device synchronization
- Production database architecture
- Kubernetes deployment
- Multi-agent architecture
- Custom ML model training
- Coding collaboration
- GitHub coding integration
- Large curated coding-problem library
- Production-grade code execution until isolation is verified

---

# 🔮 Future Work

Potential future improvements include:

- User authentication
- Cloud database
- Cross-device synchronization
- Advanced learner analytics
- Adaptive learning paths
- Improved recommendation systems
- Production-grade isolated code execution
- More programming languages
- Cloud deployment
- Observability and monitoring
- Role-based access
- Improved document processing
- More advanced AI tutoring capabilities

---

# 👨‍💻 Author

**Kolli Jayanth Eswar**

Computer Science & Engineering  
KL University

GitHub:  
https://github.com/KOLLIJAYANTHESWAR

---

# 📄 License

License information will be added before the production release.
