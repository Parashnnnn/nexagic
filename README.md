# NexAgic ⚡
### Autonomous Real-Time AI Research Agent & Chatbot

NexAgic is an enterprise-grade AI research agent designed like a fusion of Perplexity and ChatGPT. Unlike typical chatbots limited by static training cutoff dates, NexAgic actively searches the live web, fetches and extracts deep article content, cross-verifies claims across independent sources, and synthesizes answers with interactive, verified citations `[1]`.

The complete project codebase is arranged in [`backend/`](./backend/) and [`frontend/`](./frontend/).

---

## 🏗️ Architecture Diagram

```mermaid
flowchart TD
    subgraph Frontend["Frontend (Next.js 15 + TypeScript + Tailwind)"]
        UI["Chatbot UI & Empty State"]
        Input["Mode Pill Switcher (Quick / Deep)"]
        Activity["Live Agent Activity Drawer"]
        Markdown["Streaming Markdown & Citation Chips [1]"]
        Sources["Verified Sources Panel"]
    end

    subgraph Backend["Backend (FastAPI + Async Python 3.11+)"]
        API["FastAPI /api/chat/stream"]
        RateLimit["Sliding Window Rate Limiter"]
        Agent["ReAct Agent Loop (loop.py)"]
        Prompts["System Prompts & Injection Defense"]
        DB[(SQLite / PostgreSQL + SQLAlchemy)]
    end

    subgraph Tools["Agent Tools & Services"]
        SearchProv["SearchProvider (Tavily / DDG Fallback)"]
        Fetcher["PageFetcher (SSRF Block + Trafilatura)"]
        Calc["Safe AST Calculator"]
        DateTime["Current Temporal Anchor"]
    end

    subgraph External["External APIs & Live Internet"]
        Claude["Anthropic Claude API (Native Tool Use)"]
        Tavily["Tavily Search API"]
        Web["Live Web Endpoints"]
    end

    Input --> UI
    UI -->|SSE Request| API
    API --> RateLimit
    RateLimit --> Agent
    Agent --> Prompts
    Agent <--> Claude
    Agent --> Tools
    SearchProv <--> Tavily
    SearchProv <--> Web
    Fetcher <--> Web
    Agent -->|Stream Status, Tool Calls, Tokens, Sources| API
    API -->|SSE Events| Activity
    API -->|SSE Events| Markdown
    API -->|SSE Events| Sources
    API --> DB
```

---

## 🚀 Quick Run Commands

### 1. Start the Backend:
```powershell
cd backend
.\.venv\Scripts\python.exe -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

### 2. Start the Frontend:
```powershell
cd frontend
npm run dev
```

Visit **`http://localhost:3000`** in your browser!

---

## 🧪 Test Commands

### Backend Tests (14 passing tests):
```powershell
cd backend
.\.venv\Scripts\python.exe -m pytest tests -v
```

### Frontend Tests (Vitest 3 passing tests):
```powershell
cd frontend
npx vitest run
```

