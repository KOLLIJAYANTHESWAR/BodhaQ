# BodhaQ

> **Study → Assess → Evaluate → Understand → Identify Weakness → Practice → Improve**

BodhaQ is an AI-powered learning workspace that transforms study materials and topics into interactive learning experiences.

It combines document understanding, Retrieval-Augmented Generation (RAG), AI-generated learning content, assessments, deterministic evaluation, learning-gap analysis, resume-based preparation, targeted practice, resource discovery, and an isolated coding workspace into one learning platform.

---

## 🚀 Overview

BodhaQ helps students:

- Understand study material
- Ask questions about their own documents
- Learn topics with AI-generated explanations
- Discover learning resources and videos
- Generate and take assessments
- Identify learning gaps
- Practice weak areas
- Prepare from their resume
- Practice coding in an isolated execution environment
- Track selected learning state locally

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

BodhaQ supports:

- PDF
- PPTX
- DOCX
- Topic-based learning without a document

Uploaded documents are processed through the document-ingestion pipeline:

```text
Upload
  ↓
Validation
  ↓
Text Extraction
  ↓
Chunking
  ↓
Embedding Generation
  ↓
ChromaDB
```

Documents can then be used by the learning, doubt-solving, quiz, and targeted-practice workflows.

---

# 🧠 AI Learning

Users can provide a topic and generate structured learning content.

The learning workflow can provide information such as:

- Definition
- Key concepts
- Examples
- Important points
- Supporting learning resources
- Relevant videos

Gemini is used for AI-generated learning content.

External learning resources are discovered through the Tavily-powered resource-search service.

### Learning Flow

```text
Topic
  ↓
Gemini
  ↓
Structured Learning Content
  ↓
Resource Search
  ↓
Learning Resources / Videos
```

---

# 🔎 Retrieval-Augmented Generation (RAG)

BodhaQ implements document-grounded RAG.

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

### Embedding Model

```text
gemini-embedding-001
```

ChromaDB data is runtime data and is intentionally excluded from Git.

RAG collections are namespaced by the anonymous BodhaQ session and document, preventing one session from accessing another session's vector data.

---

# 💬 Doubt Solving

BodhaQ supports questions about study material as well as general topic questions.

## Document Mode

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
Grounded Answer
```

## Topic Mode

Questions can also be asked without an uploaded document.

In topic mode, Gemini generates the response using general AI knowledge rather than document retrieval.

---

# 📝 AI Assessments

BodhaQ can generate quizzes from:

- Topics
- Uploaded documents
- Targeted weak areas
- Resume items

Quiz generation uses Gemini.

Quiz evaluation is performed deterministically by the backend.

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

### Answer Security

Correct answers are kept in the backend's private quiz representation.

The public quiz response does not expose:

- `correct_answer`
- Answer explanations intended for post-submission evaluation

This prevents the frontend from receiving answer keys before the user submits the quiz.

---

# 📊 Learning Gaps

BodhaQ identifies learning gaps from completed assessments.

Current thresholds:

| Score | Status |
|---:|---|
| < 60% | Needs Practice |
| 60–79% | Improving |
| ≥ 80% | Learned |

Learning gaps are calculated from quiz performance by the backend rather than being decided by the LLM.

Assessment-specific learning-gap information remains associated with the relevant assessment/session.

---

# 🎯 Targeted Practice

Targeted practice can be generated from:

- A specific topic
- A weak area from an assessment
- A document using RAG

The existing quiz-generation and deterministic-evaluation infrastructure is reused for targeted practice.

```text
Weak Area
   ↓
Practice Generation
   ↓
Questions
   ↓
Attempt
   ↓
Evaluation
   ↓
Updated Learning Signal
```

---

# 📄 Resume Preparation

Users can upload:

- PDF resumes
- DOCX resumes

BodhaQ extracts structured resume information such as:

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

A new resume replaces the active resume version rather than mixing items from different resume versions.

Resume-related backend data is isolated by the anonymous session.

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

## AI Learn Code

Users can provide:

- A problem name
- A complete problem statement

Optional:

- Constraints
- Sample test case

The coding workflow supports problem generation and test-case generation through Gemini.

The generated problem and test-case state is stored server-side for the current anonymous session.

---

## IDE

The IDE allows users to solve problems independently.

The workspace supports:

- Monaco Editor
- Problem statement
- Input/output details
- Constraints
- Examples
- Difficulty/topics
- Public tests
- Private tests
- Custom input
- Run
- Submit
- AI code analysis

### Focus Mode

Focus Mode is designed to keep AI assistance disabled while the user is solving independently.

When Focus Mode is disabled, the coding workflow can provide AI-assisted operations such as:

- Explain Code
- Create Test Cases
- Improve Code

AI analyzes the code currently present in the editor.

---

# 🧪 Isolated Code Execution

BodhaQ does not execute arbitrary user code directly inside the FastAPI process.

Code execution uses a Docker-based isolated runtime.

```text
Frontend
   ↓
FastAPI
   ↓
Code Execution Service
   ↓
Docker Container
   ↓
Java / Python
   ↓
Execution Result
   ↓
Deterministic Evaluation
   ↓
Frontend
```

### Supported Languages

```text
Java
Python
```

### Current Sandbox Controls

The execution service currently applies:

- Docker-only execution
- CPU limits
- Memory limits
- PID limits
- Execution timeouts
- Compilation timeouts
- Output-size limits
- Network disabled
- Read-only root filesystem
- Temporary filesystem for `/tmp`
- Dropped Linux capabilities
- `no-new-privileges`
- Non-root execution
- Temporary workspace isolation
- Container cleanup
- Workspace cleanup
- Bounded process output handling

### Java Runtime

```text
eclipse-temurin:17-alpine
```

### Python Runtime

```text
python:3.10-alpine
```

### Resource Limits

Current execution limits include:

```text
Memory:       128 MB
CPU:          0.5 CPU
PIDs:         64
Compile:      10 seconds
Execution:    5 seconds
Output:       100 KB
```

The sandbox is designed so user code cannot directly access the BodhaQ application's:

- `.env`
- API credentials
- SQLite database
- ChromaDB data
- Uploaded study materials
- Internal application files
- Unrestricted host resources

The Docker execution and security controls have been exercised through the project's backend tests and production audit.

Additional production deployment hardening and infrastructure-level security review are still required before treating the complete platform as a production service.

---

# 🔐 BYOK API Key Architecture

BodhaQ uses a **Bring Your Own Key (BYOK)** model for provider API access.

Users provide their own:

- Gemini API key
- Tavily API key

BodhaQ does **not** maintain shared provider API keys in the backend.

### Key Flow

```text
Browser
  │
  ├── Gemini API Key
  └── Tavily API Key
          │
          ▼
    sessionStorage
          │
          ▼
 frontend/api/client.js
          │
          ▼
 HTTP request headers
          │
          ▼
      FastAPI
       │    │
       │    └── Tavily
       │
       └────── Gemini
```

### Storage Model

Provider API keys are:

- Stored only in browser `sessionStorage`
- Not stored in SQLite
- Not stored in ChromaDB
- Not stored in backend files
- Not stored in `localStorage`
- Not persisted by backend services
- Not returned by backend responses
- Not intentionally written to application logs

Closing the browser tab/session clears `sessionStorage`.

### Request Headers

Gemini requests use:

```text
X-Gemini-API-Key
```

Tavily requests use:

```text
X-Tavily-API-Key
```

The backend treats these as request-scoped credentials.

### Important BYOK Limitation

Because the keys are entered into the browser, they are inherently accessible to the browser environment.

The BYOK model therefore avoids server-side credential persistence but cannot provide the same secrecy as a fully server-controlled secret architecture.

Users should only provide API keys they are comfortable using from their own browser session.

---

# 👤 Anonymous Session Isolation

BodhaQ currently does not require user authentication.

Instead, the backend creates an anonymous session.

```text
Browser
   ↓
POST /api/session
   ↓
Session ID + Signed Session Token
   ↓
sessionStorage
   ↓
Subsequent API Requests
```

Requests include:

```text
X-BodhaQ-Session
```

The backend validates the signed token before accessing session-owned data.

### Session-Isolated Data

Session isolation is applied to backend state such as:

- Documents
- RAG collections
- Quizzes
- Evaluations
- Resume data
- Coding problems
- Coding test cases

A session cannot use another session's identifier to access its protected backend data.

### Current Session Model

The session token is:

- Signed server-side
- Time-limited
- Validated on protected routes
- Not used as a permanent authentication credential

The current MVP does not provide:

- User accounts
- Password authentication
- Social login
- Cross-device identity
- Account recovery

---

# 💾 Client-Side Persistence

The current MVP uses browser storage for selected client-side state.

Depending on the feature, this includes areas such as:

- Recent quiz history
- Resume quiz progress
- Resume assessment state
- Learning-gap information
- Coding saved-code state

Quiz history is limited to the most recent five completed quizzes.

### Session Credentials

Session credentials and BYOK credentials use `sessionStorage`.

### Feature State

Selected non-sensitive client-side application state may use `localStorage`.

### Limitations

Browser storage is:

- Browser-specific
- Device-specific
- Not synchronized between devices
- Lost when site data is cleared

A future authenticated version can move persistent user data to a backend database.

---

# 🏗️ Architecture

```text
                           ┌─────────────────────┐
                           │      React UI       │
                           │     Vite + JS       │
                           └──────────┬──────────┘
                                      │
                              REST API Requests
                                      │
                    ┌─────────────────┴─────────────────┐
                    │                                   │
                    ▼                                   ▼
           Session + BYOK Headers                Client State
                    │                                   │
                    ▼                                   ▼
             ┌──────────────┐                    Browser Storage
             │    FastAPI   │
             │    Backend   │
             └──────┬───────┘
                    │
       ┌────────────┼───────────────┬───────────────┐
       │            │               │               │
       ▼            ▼               ▼               ▼
   ┌────────┐  ┌──────────┐   ┌──────────┐   ┌────────────┐
   │ Gemini │  │ Tavily   │   │ ChromaDB │   │   SQLite   │
   │   AI   │  │ Resources│   │   RAG    │   │  Metadata  │
   └────────┘  └──────────┘   └──────────┘   └────────────┘
       │                            ▲
       │                            │
       └────── Embeddings ──────────┘

                    ┌──────────────────────┐
                    │ Code Execution       │
                    │ Service              │
                    └──────────┬───────────┘
                               │
                               ▼
                       ┌──────────────┐
                       │    Docker    │
                       │   Sandbox    │
                       └──────────────┘
```

---

# 🛠️ Technology Stack

## Frontend

- React
- Vite
- JavaScript
- CSS
- Monaco Editor

## Backend

- Python
- FastAPI
- Pydantic
- Uvicorn

## AI

- Google Gemini
- Gemini Embeddings

## Resource Discovery

- Tavily

## RAG

- ChromaDB
- Gemini Embeddings
- Document chunking
- Similarity retrieval

## Document Processing

- PyMuPDF
- python-docx
- python-pptx

## Storage

- SQLite
- Browser `localStorage`
- Browser `sessionStorage`

## Code Execution

- Docker
- Java
- Python
- Isolated execution containers

---

# 📁 Project Structure

```text
BodhaQ/
│
├── backend/
│   │
│   ├── app/
│   │   │
│   │   ├── dependencies/
│   │   │   └── session.py
│   │   │
│   │   ├── ingestion/
│   │   │   ├── chunker.py
│   │   │   ├── docx_loader.py
│   │   │   ├── pdf_loader.py
│   │   │   └── pptx_loader.py
│   │   │
│   │   ├── models/
│   │   │   ├── requests.py
│   │   │   ├── responses.py
│   │   │   └── session.py
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
│   │   │   ├── session.py
│   │   │   └── settings.py
│   │   │
│   │   ├── services/
│   │   │   ├── code_execution_service.py
│   │   │   ├── document_service.py
│   │   │   ├── evaluation_service.py
│   │   │   ├── gemini_service.py
│   │   │   ├── problem_store.py
│   │   │   ├── quiz_service.py
│   │   │   ├── rag_service.py
│   │   │   ├── resource_search_service.py
│   │   │   ├── resume_service.py
│   │   │   └── session_service.py
│   │   │
│   │   ├── utils/
│   │   │   └── json_parser.py
│   │   │
│   │   └── main.py
│   │
│   ├── tests/
│   │   ├── deep_test.py
│   │   ├── test_api.py
│   │   └── test_docker_execution.py
│   │
│   ├── .env.example
│   ├── .gitignore
│   ├── production_audit.py
│   ├── requirements.txt
│   └── README.md
│
├── frontend/
│   │
│   ├── public/
│   │
│   ├── src/
│   │   ├── api/
│   │   ├── components/
│   │   ├── pages/
│   │   └── utils/
│   │
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
- Docker Desktop

Docker is required for the isolated coding-execution workflow.

---

# 🔧 Backend Setup

From the project root:

```bash
cd backend
```

Create a virtual environment:

```bash
python -m venv venv
```

### Windows

Command Prompt:

```cmd
venv\Scripts\activate
```

PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Backend Environment Configuration

Create:

```text
backend/.env
```

The backend does **not** require Gemini or Tavily API keys in `.env`.

The backend configuration contains non-secret application configuration such as:

```env
FRONTEND_URL=http://localhost:5173
BODHAQ_SESSION_SECRET=replace_with_a_long_random_secret
```

Generate a strong session secret for local development.

Never commit the real `.env` file.

A safe template is provided at:

```text
backend/.env.example
```

---

## Start the Backend

From `backend/`:

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

Health endpoint:

```text
http://localhost:8000/health
```

---

# 🎨 Frontend Setup

From the project root:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Start the Vite development server:

```bash
npm run dev
```

The Vite development server will display the local URL.

---

# 🔑 Configure Provider API Keys

BodhaQ uses BYOK.

Open the BodhaQ Settings page and provide:

```text
Gemini API Key
Tavily API Key
```

The keys are stored only in the browser session and are sent to the backend through request headers when required.

They are not configured as backend `.env` secrets.

---

# 🔐 Environment and Runtime Files

Never commit real secrets.

The repository intentionally excludes runtime and local-development data such as:

```text
.env
venv/
node_modules/
dist/
backend/data/
backend/uploads/
SQLite databases
ChromaDB runtime data
logs
temporary files
```

The repository contains:

```text
backend/.env.example
```

as a safe configuration template.

---

# 🔒 Security Principles

## API Keys

Provider API keys use a BYOK model.

Keys are supplied by the browser per request and are not persisted by the backend.

Backend services must not log or store provider API keys.

---

## Anonymous Sessions

Protected backend resources are associated with an anonymous signed session.

The backend validates the session token before accessing session-owned data.

---

## Quiz Answers

Correct quiz answers remain in the backend's private representation until evaluation.

Public quiz responses do not expose the answer key.

---

## RAG Isolation

Document vector collections are isolated by session and document.

A session cannot directly access another session's document collection through the session-aware RAG APIs.

---

## User Uploads

Uploaded documents are treated as untrusted runtime data.

They are:

- Validated
- Size-limited
- Stored outside source code
- Associated with the current session
- Removed from Git tracking

---

## Path Security

Upload and runtime file handling uses controlled paths and validation to reduce path traversal and arbitrary-file access risks.

---

## Code Execution

User-submitted code is untrusted input.

It must not be executed using:

```python
exec()
eval()
```

or unrestricted host-process execution.

BodhaQ uses a Docker-based execution service with resource and isolation controls.

---

# 🧪 Testing

The backend contains automated and diagnostic tests covering important application and security behavior.

### Core Areas

Testing includes:

- FastAPI application startup
- API route registration
- Session creation
- Session token validation
- Invalid-session protection
- Session data isolation
- Document isolation
- RAG isolation
- Quiz answer leakage prevention
- Evaluation storage
- Coding problem isolation
- Coding problem concurrency
- Upload validation
- Temporary-file cleanup
- Docker availability
- Python execution
- Java execution
- Runtime-error handling
- Timeout protection
- Output-limit protection
- Security headers
- CORS configuration
- Dependency checks
- Debug-artifact checks

---

# 🛡️ Production Audit

BodhaQ includes:

```text
backend/production_audit.py
```

The current backend production audit verifies areas including:

```text
Foundation
BYOK configuration
Session security
Data isolation
File/path security
Docker sandbox controls
Code execution
HTTP security
Dependencies
Session-aware route protection
```

The latest backend audit completed with:

```text
PASS: 37
FAIL: 0
SKIP: 0
```

The audit also checks that provider credentials are not configured as backend environment secrets and that protected routes require valid anonymous sessions where applicable.

The learning endpoint is intentionally stateless and therefore does not require the session dependency because it does not access session-owned persistent data.

---

# 🚧 Current Development Status

BodhaQ is under active development.

## Core Platform

- [x] React + Vite frontend
- [x] FastAPI backend
- [x] Gemini integration
- [x] Tavily resource discovery
- [x] PDF ingestion
- [x] PPTX ingestion
- [x] DOCX ingestion
- [x] Document chunking
- [x] Gemini embeddings
- [x] ChromaDB vector storage
- [x] Session-aware RAG
- [x] Document-based doubts
- [x] Topic-based doubts
- [x] AI learning content
- [x] Learning resources
- [x] AI quiz generation
- [x] Deterministic quiz evaluation
- [x] Learning-gap analysis
- [x] Targeted practice
- [x] Resume extraction workflow
- [x] Resume-based assessments
- [x] Anonymous session isolation
- [x] BYOK Gemini integration
- [x] BYOK Tavily integration

## Coding Platform

- [x] Coding workspace foundation
- [x] Monaco Editor integration
- [x] Java execution
- [x] Python execution
- [x] Docker-based execution
- [x] Resource limits
- [x] Timeout protection
- [x] Output limits
- [x] Network restriction
- [x] Container cleanup
- [x] Session-aware coding problem storage
- [x] Public/private test infrastructure
- [ ] Complete end-to-end AI coding workflow
- [ ] Final production deployment hardening

## Platform Hardening

- [x] Backend production audit
- [x] Session token validation
- [x] Session data isolation
- [x] API security headers
- [x] CORS configuration
- [x] Upload validation
- [x] Runtime cleanup
- [x] Docker sandbox controls
- [ ] Full production infrastructure deployment
- [ ] Production monitoring
- [ ] Final end-to-end regression testing

---

# 🗺️ Development Roadmap

## Phase 1 — Core Learning Platform

- Document ingestion
- Topic learning
- RAG
- Doubt solving
- Quiz generation
- Deterministic evaluation
- Learning gaps
- Targeted practice
- Resource discovery

---

## Phase 2 — Resume Preparation

- Resume extraction
- Structured resume items
- Resume assessments
- Preparation workflow
- Progress tracking

---

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

---

## Phase 4 — Production Hardening

- Full end-to-end regression testing
- Deployment configuration
- Monitoring
- Observability
- Performance testing
- Infrastructure security review
- Production documentation
- Backup/recovery strategy
- Operational hardening

---

# 🎯 Design Principles

## AI assists; deterministic systems evaluate

The LLM is used for tasks such as:

- Explanations
- Learning content
- Quiz generation
- Coding assistance
- Resource-oriented learning workflows

Deterministic backend logic handles tasks such as:

- Quiz scoring
- Learning-gap calculation
- Session ownership
- Test-result processing
- Resource limits

---

## RAG grounds document-based interactions

When a user asks about uploaded material, relevant document chunks are retrieved before generating the document-grounded response.

---

## User code remains under user control

AI coding assistance analyzes or proposes changes.

The application should not silently overwrite the user's code.

---

## Features remain modular

Learning, documents, quizzes, evaluations, resumes, and coding maintain separate service responsibilities.

---

## Security before convenience

User-provided code, uploaded documents, API keys, and generated content are treated as untrusted or sensitive inputs where appropriate.

---

## Session isolation before multi-user authentication

The current MVP uses anonymous signed sessions to provide isolation without introducing a full account/authentication system.

---

# 📌 Important MVP Limitations

The current MVP intentionally does not include:

- User authentication
- Multi-user accounts
- Cross-device synchronization
- Production cloud database architecture
- Kubernetes deployment
- Multi-agent architecture
- Custom ML model training
- Coding collaboration
- GitHub coding integration
- Large curated coding-problem library
- Cloud-hosted execution infrastructure
- Production monitoring infrastructure
- Full production deployment

The current backend has undergone application-level production auditing, but deployment infrastructure, operational monitoring, and complete end-to-end production validation remain separate concerns.

---

# 🔮 Future Work

Potential future improvements include:

- User authentication
- Cloud database
- Cross-device synchronization
- Advanced learner analytics
- Adaptive learning paths
- Improved recommendation systems
- Cloud-hosted isolated code execution
- More programming languages
- Production cloud deployment
- Observability and monitoring
- Role-based access
- Improved document processing
- Advanced AI tutoring
- Larger coding-problem ecosystem
- GitHub integration
- Collaborative learning

---

# 📂 Runtime Data

BodhaQ generates runtime data that should not be committed to Git.

Examples include:

```text
backend/data/
backend/uploads/
SQLite databases
ChromaDB collections
temporary execution workspaces
logs
```

These paths are protected by the repository `.gitignore`.

---

# 🧩 Backend Documentation

The backend has its own detailed documentation:

```text
backend/README.md
```

It covers:

- Backend architecture
- API structure
- Session handling
- BYOK integration
- RAG
- Document processing
- Quiz generation
- Resume preparation
- Coding execution
- Docker sandbox
- Configuration
- Testing
- Security
- Production considerations

---

# 📜 API Areas

The backend currently exposes API areas for:

```text
Health
Session
Learning
Documents
Quizzes
Doubts
Settings
Resume Preparation
Coding
```

The FastAPI OpenAPI documentation is available during local development at:

```text
http://localhost:8000/docs
```

---

# 🔄 Request Flow

A typical protected request follows this pattern:

```text
React Frontend
      ↓
API Client
      ↓
Session Token
      +
Optional Gemini/Tavily API Key
      ↓
FastAPI
      ↓
Session Validation
      ↓
Route
      ↓
Service Layer
      ↓
Provider / Database / RAG / Sandbox
      ↓
Controlled Response
      ↓
React Frontend
```

Provider API keys are only included on requests that require the corresponding external provider.

---

# 🧠 Architecture Philosophy

BodhaQ intentionally separates responsibilities.

```text
Frontend
   │
   ▼
API Routes
   │
   ▼
Services
   │
   ├── Gemini
   ├── Tavily
   ├── RAG
   ├── Documents
   ├── Quiz
   ├── Evaluation
   ├── Resume
   └── Code Execution
```

The architecture avoids placing business logic directly inside frontend components or FastAPI route handlers wherever practical.

---

# 🚫 What BodhaQ Does Not Claim

BodhaQ does not currently claim to provide:

- A fully autonomous AI agent system
- A multi-agent architecture
- A custom-trained foundation model
- A production-scale multi-tenant SaaS architecture
- Cross-device authenticated user accounts
- Fully cloud-managed execution infrastructure
- Unlimited code execution
- Guaranteed correctness of AI-generated content

AI-generated content should be treated as assistance and reviewed by the user.

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

---

# ⭐ Project

**BodhaQ**

> **Study smarter. Assess yourself. Understand your gaps. Practice. Improve.**
