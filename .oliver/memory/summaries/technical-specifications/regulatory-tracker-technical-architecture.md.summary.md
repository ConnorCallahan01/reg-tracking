# Regulatory Tracker - Technical Architecture Documentation

**Source**: repository/technical-specifications/regulatory-tracker-technical-architecture.md
**Type**: Technical Specification
**Upload Date**: 2026-02-10
**Source SHA**: Not provided

## Key Points

- **Three-tier architecture** with client, frontend (Next.js 14), and backend (FastAPI) layers plus asynchronous background processing for AI-powered regulatory analysis
- **Frontend** built with React 19, TypeScript, and Tailwind CSS using Next.js App Router for file-based routing across dashboard, scans, results, and workflow pages
- **Backend API** provides RESTful endpoints for scan management, results retrieval, workflow transitions, and knowledge store with filtering and export capabilities
- **ScanOrchestrator** coordinates a 7-stage pipeline (initializing → retrieving sources → detecting changes → analyzing → scoring → generating artifacts → finalizing) with error handling at each stage
- **Claude Sonnet 4 API integration** analyzes regulatory documents and returns structured JSON with impact assessment, recommended actions, and confidence scores
- **SQLite database** stores scans, results, sources, and workflow status with support for filtering by impact level and review requirements
- **Asynchronous task processing** using Python asyncio launches background scans immediately and returns scan IDs, with real-time status polling and artifact generation (markdown + Word documents)
- **Production scalability path** includes migration to PostgreSQL, distributed task queuing with Celery/Redis, caching layers, and monitoring with Prometheus/Grafana

## Entities and Topics

- **Next.js 14**: Frontend framework with React 19, TypeScript, and Tailwind CSS for UI rendering and API communication
- **FastAPI**: Python web framework handling RESTful API endpoints with Pydantic validation and Uvicorn ASGI server
- **ScanOrchestrator**: Core service managing the 7-stage regulatory analysis pipeline with state transitions and error handling
- **LLMService**: Integrates with Claude Sonnet 4 API for AI-powered document analysis with system and task prompts
- **ChangeDetectionService**: Identifies material regulatory changes using SHA-256 hashing, keyword detection, and demo mode
- **ArtifactGenerator**: Creates markdown and Word document (.docx) outputs with professional formatting and metadata
- **SQLite Database**: File-based relational database storing scans, results, sources, and workflow status at `data/regulatory_tracker.db`
- **Mock Sources Directory**: JSON files simulating regulatory agency data (FinCEN, OCC, Federal Reserve) at `data/mock-sources/`
- **Anthropic Claude API**: External AI service for regulatory document analysis with rate limiting and cost considerations (~$0.10-$0.50 per analysis)
- **Docker Compose**: Containerization with frontend (Node.js 20) and backend (Python 3.11) services sharing volumes for data persistence

## Relevance to Project

This document serves as the authoritative technical blueprint for the Regulatory Tracker application, detailing the complete system architecture, service components, API specifications, and data flow patterns. It provides essential guidance for development, deployment, and future scaling of the AI-powered regulatory compliance platform.