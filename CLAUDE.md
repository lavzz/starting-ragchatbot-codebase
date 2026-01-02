# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a full-stack RAG (Retrieval-Augmented Generation) chatbot for querying course materials. It uses ChromaDB for vector storage, Claude Sonnet for AI generation, and a static HTML/JS frontend.

## Development Commands

```bash
# Install dependencies
uv sync

# Run the backend server (from project root)
cd backend && uv run uvicorn app:app --reload --port 8000

# Or use the provided script (Git Bash on Windows)
./run.sh
```

Server runs at `http://localhost:8000` (Web UI) and `http://localhost:8000/docs` (FastAPI docs).

## Environment Setup

Copy `.env.example` to `.env` and add your Anthropic API key:
```
ANTHROPIC_API_KEY=your-key-here
```

## Architecture

### Backend (`backend/`)
- **app.py**: FastAPI application with CORS, routes (`/api/query`, `/api/courses`), and static file serving
- **rag_system.py**: Main orchestrator coordinating document processing, vector search, and AI generation
- **vector_store.py**: ChromaDB wrapper with two collections: `course_catalog` (metadata) and `course_content` (chunks)
- **ai_generator.py**: Claude API integration with tool-use loop for AI-driven searches
- **document_processor.py**: Parses course documents, extracts metadata, chunks text (800 chars, 100 overlap)
- **search_tools.py**: Tool interface and `CourseSearchTool` for semantic search with filtering
- **session_manager.py**: In-memory conversation history per session (2 exchanges max)
- **config.py**: Configuration management (API keys, model settings)
- **models.py**: Pydantic models (Course, Lesson, CourseChunk)

### Frontend (`frontend/`)
Static HTML/CSS/JS chat interface. Uses marked.js for markdown rendering and Fetch API for backend communication.

### Data Flow
```
User Query → FastAPI → RAGSystem → [Vector Search] + [Claude with Tools] → Response + Sources
```

### Key Configuration Values
- Embedding model: all-MiniLM-L6-v2
- AI model: claude-sonnet-4-20250514
- Chunk size: 800 chars, overlap: 100 chars
- Max search results: 5
- Temperature: 0 (deterministic)

## Course Documents

Place course material files in `docs/` directory. Supported formats: TXT, PDF, DOCX. Documents are automatically loaded on server startup.
