<div align="center">

# 🎙️ AI Automated Interviewer

**An intelligent, real-time AI interviewer that conducts project presentation interviews using screen analysis, speech recognition, and GPT-4 powered evaluation.**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com)

---

</div>

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**AI Automated Interviewer** is a full-stack application that simulates a real-time project presentation interview. It captures the candidate's screen (via screen share) and audio (via microphone), processes them in real-time, and uses OpenAI's GPT-4 and Whisper models to:

- **Transcribe** spoken answers via Whisper speech-to-text.
- **Analyze** on-screen content via OCR (Tesseract).
- **Generate** intelligent, context-aware follow-up questions.
- **Evaluate** the candidate across multiple dimensions (technical depth, clarity, originality, understanding).
- **Produce** a downloadable evaluation report at the end of the session.

The interview flows naturally — it greets the candidate, asks for their name, learns about their project, and then asks progressively deeper technical questions based on what it sees and hears.

---

## Features

| Feature | Description |
|---|---|
| 🗣️ **Real-Time Speech Recognition** | Audio is streamed to the backend and transcribed using OpenAI Whisper in near real-time. |
| 🖥️ **Screen Analysis (OCR)** | Screen frames are captured periodically and analyzed with Tesseract OCR to understand the code/slides being presented. |
| 🤖 **AI-Powered Questions** | GPT-4 generates contextual, conversational follow-up questions based on what the candidate says and shows. |
| 📊 **Live Evaluation Scoring** | Candidates are scored in real-time on technical depth, clarity, originality, and understanding (1–10 scale). |
| 📝 **Downloadable Report** | A comprehensive markdown evaluation report can be downloaded after the interview session. |
| 🔊 **Text-to-Speech** | AI-generated questions are spoken aloud to the candidate using the browser's Speech Synthesis API. |
| 🔌 **WebSocket Communication** | Low-latency, bidirectional communication between frontend and backend via WebSockets. |
| 🔄 **Auto-Reconnect** | The frontend automatically attempts to reconnect if the WebSocket connection drops during an active session. |

---

## Architecture

```
┌──────────────────────────────────────────────────────────┐
│                      Frontend (React + Vite)             │
│                                                          │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │ Screen Share │  │ Audio Record │  │  UI Dashboard  │  │
│  │  (Canvas)    │  │ (WebAudio)   │  │  (Evaluation)  │  │
│  └──────┬───────┘  └──────┬───────┘  └────────┬───────┘  │
│         │                 │                   │          │
│         └────────┬────────┘                   │          │
│                  │ WebSocket                  │ HTTP     │
└──────────────────┼────────────────────────────┼──────────┘
                   │                            │
┌──────────────────┼────────────────────────────┼──────────┐
│                  ▼          Backend (FastAPI)  ▼          │
│  ┌─────────────────────┐  ┌──────────────────────────┐   │
│  │  WebSocket Handler  │  │     REST API (Report)    │   │
│  └──────────┬──────────┘  └──────────────────────────┘   │
│             │                                            │
│  ┌──────────▼──────────┐                                 │
│  │  Session Processor  │──► Process Queue (frames/audio) │
│  └──────────┬──────────┘                                 │
│             │                                            │
│  ┌──────────▼──────────────────────────────────────────┐  │
│  │              Interview Session (Model)              │  │
│  │  ┌──────────┐  ┌──────────┐  ┌───────────────────┐ │  │
│  │  │   OCR    │  │  Whisper │  │  GPT-4 (Q & Eval) │ │  │
│  │  │(Tesseract)│ │  (STT)   │  │                   │ │  │
│  │  └──────────┘  └──────────┘  └───────────────────┘ │  │
│  └─────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

---

## Tech Stack

### Backend
- **[FastAPI](https://fastapi.tiangolo.com/)** — High-performance async Python web framework
- **[OpenAI API](https://platform.openai.com/)** — GPT-4 for question generation & evaluation, Whisper for speech-to-text
- **[Tesseract OCR](https://github.com/tesseract-ocr/tesseract)** — Optical character recognition for screen content extraction
- **[OpenCV](https://opencv.org/)** — Image preprocessing for improved OCR accuracy
- **[Uvicorn](https://www.uvicorn.org/)** — ASGI server for FastAPI

### Frontend
- **[React 18](https://react.dev/)** — UI library
- **[TypeScript](https://typescriptlang.org/)** — Type-safe JavaScript
- **[Vite](https://vitejs.dev/)** — Fast build tool & dev server
- **[Tailwind CSS](https://tailwindcss.com/)** — Utility-first CSS framework
- **[Lucide React](https://lucide.dev/)** — Icon library
- **Web APIs** — `getDisplayMedia`, `getUserMedia`, `WebSocket`, `SpeechSynthesis`

---

## Getting Started

### Prerequisites

- **Python 3.10+**
- **Node.js 18+** and **npm**
- **Tesseract OCR** installed and available on your system PATH
  - **Windows:** Download from [UB Mannheim](https://github.com/UB-Mannheim/tesseract/wiki)
  - **macOS:** `brew install tesseract`
  - **Linux:** `sudo apt install tesseract-ocr`
- **OpenAI API Key** with access to GPT-4 and Whisper models

### Backend Setup

```bash
# Navigate to the backend directory
cd backend

# Create and activate a virtual environment
python -m venv venv

# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Create your .env file from the example
cp .env.example .env
# Edit .env and add your OpenAI API key
# OPENAI_API_KEY=sk-your-key-here

# Start the backend server
python main.py
```

The backend will start on `http://localhost:8000`.

### Frontend Setup

```bash
# Navigate to the frontend directory
cd frontend

# Install dependencies
npm install

# Start the development server
npm run dev
```

The frontend will start on `http://localhost:5173` (default Vite port).

---

## Usage

1. **Start both servers** (backend and frontend) as described above.
2. **Open the frontend** in your browser at `http://localhost:5173`.
3. **Click "Start Interview Session"** — this will:
   - Connect to the backend via WebSocket
   - Prompt you to share your screen
   - Begin recording your microphone audio
4. **Interact with the AI interviewer:**
   - It will greet you and ask for your name.
   - Introduce your project when prompted.
   - Share your screen and walk through your project — the AI watches and listens.
   - Answer the AI's follow-up questions (spoken aloud via TTS).
5. **Monitor your live evaluation scores** on the dashboard (technical depth, clarity, originality, understanding).
6. **Click "Download Report"** to get a comprehensive markdown evaluation report.
7. **Click "Stop Session"** when finished.

---

## API Reference

### WebSocket

| Endpoint | Description |
|---|---|
| `ws://localhost:8000/ws` | Main WebSocket connection for real-time data streaming |

**Client → Server Messages:**

```json
{ "type": "frame", "data": "<base64-encoded-jpeg>" }
{ "type": "audio", "data": "<base64-encoded-wav>" }
```

**Server → Client Messages:**

```json
{ "type": "session_id", "session_id": "uuid" }
{ "type": "question", "question": "...", "speak": true }
{ "type": "evaluation", "scores": { "technical_depth": 7, "clarity": 8, ... } }
```

### REST Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/report/{session_id}` | Generate and retrieve the evaluation report for a session |
| `GET` | `/health` | Health check endpoint |

---

## Project Structure

```
ai-automated-interviewer/
├── backend/
│   ├── .env.example          # Environment variable template
│   ├── .gitignore            # Python gitignore
│   ├── config.py             # App configuration & OpenAI client setup
│   ├── main.py               # FastAPI app entry point & route registration
│   ├── requirements.txt      # Python dependencies
│   ├── models/
│   │   ├── __init__.py
│   │   └── session.py        # InterviewSession model (OCR, STT, GPT-4 logic)
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── api.py            # REST endpoints (report generation, health check)
│   │   └── websocket.py      # WebSocket endpoint handler
│   └── services/
│       ├── __init__.py
│       └── session_processor.py  # Background session data processing loop
│
├── frontend/
│   ├── index.html            # HTML entry point
│   ├── package.json          # Node.js dependencies & scripts
│   ├── tsconfig.json         # TypeScript configuration
│   ├── vite.config.ts        # Vite configuration
│   ├── tailwind.config.js    # Tailwind CSS configuration
│   ├── postcss.config.js     # PostCSS configuration
│   └── src/
│       ├── main.tsx          # React entry point
│       ├── App.tsx           # Main application component
│       ├── App.css           # Component styles
│       └── index.css         # Global styles (Tailwind directives)
│
├── .gitignore                # Root gitignore
└── README.md                 # This file
```

---

## Contributing

Contributions are welcome! Here's how to get started:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/my-feature`
3. **Commit** your changes: `git commit -m "Add my feature"`
4. **Push** to the branch: `git push origin feature/my-feature`
5. **Open** a Pull Request

### Guidelines

- Follow existing code style and patterns
- Write descriptive commit messages
- Test your changes before submitting a PR
- Update the README if you add new features

---

## License

This project is open source. Feel free to use, modify, and distribute it.

---

<div align="center">

</div>

