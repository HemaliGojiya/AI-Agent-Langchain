# 🤖 AI-Agent-Langchain

A **production-ready AI chatbot** powered by LangGraph ReAct agents with a FastAPI backend and Streamlit frontend. Supports multiple LLM providers (Groq & OpenAI), optional real-time web search via Tavily, and fully customizable system prompts.

---

## 📌 Table of Contents

- [Overview](#overview)
- [Technical Architecture](#technical-architecture)
- [Tools & Technologies](#tools--technologies)
- [Project Structure](#project-structure)
- [Setup & Installation](#setup--installation)
  - [Using Pipenv](#using-pipenv)
  - [Using pip + venv](#using-pip--venv)
  - [Using Conda](#using-conda)
- [Environment Variables](#environment-variables)
- [Running the Application](#running-the-application)
- [API Reference](#api-reference)
- [Features](#features)

---

## Overview

This project implements a full-stack GenAI chatbot using a clean 3-phase architecture:

- **Phase 1 — AI Agent:** LangGraph ReAct agent with LLM and optional Tavily web search tool
- **Phase 2 — Backend:** FastAPI REST API with Pydantic schema validation
- **Phase 3 — Frontend:** Streamlit UI for model selection, system prompt configuration, and chat interaction

---

## Technical Architecture

```
┌─────────────────────────────┐
│   Streamlit Frontend (UI)   │  ← Phase 3
│  Model | Prompt | Query     │
└────────────┬────────────────┘
             │ HTTP POST /chat
┌────────────▼────────────────┐
│   FastAPI Backend           │  ← Phase 2
│   Pydantic Validation       │
│   Uvicorn Server :9999      │
└────────────┬────────────────┘
             │
┌────────────▼────────────────┐
│   LangGraph ReAct Agent     │  ← Phase 1
│   LLM (Groq / OpenAI)       │
│   Tools (Tavily Search)     │
└─────────────────────────────┘
```

---

## Tools & Technologies

| Technology | Purpose |
|---|---|
| [LangGraph](https://github.com/langchain-ai/langgraph) | ReAct agent orchestration |
| [LangChain](https://www.langchain.com/) | LLM tools & integrations |
| [FastAPI](https://fastapi.tiangolo.com/) | REST API backend |
| [Streamlit](https://streamlit.io/) | Interactive frontend UI |
| [Groq](https://groq.com/) | LLaMA 3.3 & Mixtral inference |
| [OpenAI](https://openai.com/) | GPT-4o-mini inference |
| [Tavily](https://tavily.com/) | Real-time web search |
| [Uvicorn](https://www.uvicorn.org/) | ASGI server |
| [Pydantic](https://docs.pydantic.dev/) | Request schema validation |

---

## Project Structure

```
AI-Agent-Langchain/
│
├── ai_agent.py       # Phase 1 — LangGraph ReAct agent setup
├── backend.py        # Phase 2 — FastAPI backend server
├── frontend.py       # Phase 3 — Streamlit frontend UI
├── requirements.txt  # pip dependencies
├── Pipfile           # Pipenv dependencies
├── Pipfile.lock      # Pipenv lock file
└── README.md
```

---

## Setup & Installation

### Prerequisites

- Python 3.11+
- API keys for [Groq](https://console.groq.com/), [OpenAI](https://platform.openai.com/), and [Tavily](https://tavily.com/)

---

### Using Pipenv

```bash
# Install Pipenv
pip install pipenv

# Install dependencies
pipenv install

# Activate virtual environment
pipenv shell
```

---

### Using pip + venv

```bash
# Create virtual environment
python -m venv venv

# Activate (macOS/Linux)
source venv/bin/activate

# Activate (Windows)
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

---

### Using Conda

```bash
# Create environment
conda create --name ai-agent python=3.11

# Activate
conda activate ai-agent

# Install dependencies
pip install -r requirements.txt
```

---

## Environment Variables

Create a `.env` file in the root directory (never commit this to Git):

```env
GROQ_API_KEY=your_groq_api_key_here
OPENAI_API_KEY=your_openai_api_key_here
TAVILY_API_KEY=your_tavily_api_key_here
```

> ⚠️ Make sure `.env` is listed in your `.gitignore` to avoid exposing secrets.

If you are **not** using Pipenv, uncomment the following lines at the top of each Python file:

```python
from dotenv import load_dotenv
load_dotenv()
```

---

## Running the Application

Run each phase in a **separate terminal**:

### Phase 1 — Test the AI Agent

```bash
python ai_agent.py
```

### Phase 2 — Start the Backend Server

```bash
python backend.py
```

The FastAPI server starts at `http://127.0.0.1:9999`. Explore the auto-generated Swagger docs at:

```
http://127.0.0.1:9999/docs
```

### Phase 3 — Launch the Frontend

```bash
streamlit run frontend.py
```

> ⚠️ **Make sure the backend is running before launching the frontend.**

---

## API Reference

### `POST /chat`

Interact with the AI agent.

**Request Body:**

```json
{
  "model_name": "llama-3.3-70b-versatile",
  "model_provider": "Groq",
  "system_prompt": "Act as a helpful assistant",
  "messages": ["What is the capital of France?"],
  "allow_search": false
}
```

**Supported Models:**

| Provider | Model |
|---|---|
| Groq | `llama-3.3-70b-versatile` |
| Groq | `mixtral-8x7b-32768` |
| Groq | `llama3-70b-8192` |
| OpenAI | `gpt-4o-mini` |

**Response:** Plain text string with the agent's reply.

---

## Features

- 🧠 **Multi-model support** — Switch between Groq (LLaMA, Mixtral) and OpenAI (GPT-4o-mini) models
- 🔍 **Web search** — Toggle Tavily-powered real-time search on or off per query
- 📝 **Custom system prompts** — Define the agent's behavior and persona from the UI
- ⚡ **ReAct agent** — LangGraph's reasoning + acting loop for intelligent responses
- 🛡️ **Schema validation** — Pydantic ensures clean, typed API requests
- 📖 **Swagger UI** — Auto-generated API docs at `/docs`
