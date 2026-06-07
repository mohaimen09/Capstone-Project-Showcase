# Enquiro — Multi-Agent AI Research Platform

> **Source code is not public.** This repository serves as a project overview and documentation showcase.

**Live demo:** https://enquiro-gilt.vercel.app

---

## What it is

Enquiro is a multi-user web platform that guides researchers and students from a raw research idea to a complete, exportable academic paper draft. It uses a pipeline of specialized AI agents — built on LangGraph — to handle every step: topic selection, literature sourcing, methodology planning, and paper writing. Users stay in control at every checkpoint; the agents do the heavy lifting.

Beyond the main pipeline, Enquiro includes a Research Studio for paper-centric exploration (AI-generated podcasts, structured debates, narrated video explainers) and six standalone tools for common research tasks.

---

## Features

### Three-Phase Research Pipeline

| Phase | What happens |
|---|---|
| **Phase 1 — Topic Discovery** | Submit a raw idea. Four agents (TopicProposer, TopicCritic, TopicChallenger, TopicJudge) debate and score candidates. Accept one. OutlineWriter structures a full paper outline. |
| **Phase 2 — Research Planning** | A panel of three agents votes on the best methodology. SourceScout and SourceReconciler query OpenAlex and Semantic Scholar for real papers with DOIs. EvidencePlanner builds a source pack. MethodologyWriter produces a timeline, risk assessment, and next actions. |
| **Phase 3 — Writing and Export** | SectionWriter generates all six paper sections, streamed live. SectionReviewer quality-gates Introduction and Methodology (auto-revision if score < 7/10). Export to PDF, DOCX, or LaTeX (ACM, IEEE, Generic). |

Every step has a Regenerate checkpoint — outputs are versioned and stored.

### Research Studio

| Feature | Description |
|---|---|
| Studio Chat | Conversational Q&A grounded in saved papers, with traceable citations |
| AI Podcast | Host + Expert + Skeptic discuss a paper — full script and downloadable audio |
| AI Debate | Agents argue both sides of a research proposition; real-time Tavily evidence; moderator delivers verdict with audio |
| Paper to Video | Any paper converted to a narrated, scene-by-scene explainer video (MP4) |

### Standalone Tools

| Tool | Description |
|---|---|
| Paper Finder | Multi-agent parallel search across OpenAlex and Semantic Scholar |
| Research Chat | Conversational academic search with cited answers |
| Paper Summarizer | Summarize by DOI, URL, or uploaded PDF |
| BibTeX Exporter | Generate citation-ready `.bib` files from paper metadata |
| Literature Review Writer | Full literature review section from a topic query, streamed live |
| Reverse Reference Finder | Find papers that cite a given paper — 6-phase parallel pipeline |

---

## Architecture

```
Browser (React + Vite)
        |
        | HTTPS / SSE
        v
FastAPI Backend (Fly.io, Docker)
        |
   +---------+-----------+
   |                     |
LangGraph Pipelines   Supabase PostgreSQL
(15 agent graphs)     (auth + data + RLS)
        |
   LLM Chain:
   Groq (5 keys) -> Cerebras -> Gemini
   (~2.5M tokens/day total capacity)
        |
   External APIs:
   OpenAlex, Semantic Scholar,
   Crossref, Unpaywall, Tavily
        |
   Async rendering:
   GitHub Actions -> Cloudflare R2
   (audio via Kokoro TTS, video via ffmpeg)
```

**Frontend:** React 18, TypeScript, Vite, Tailwind CSS, shadcn/ui, native EventSource (SSE)

**Backend:** Python, FastAPI, LangGraph, LangChain, asyncio

**Database:** Supabase PostgreSQL with Row-Level Security

**LLMs:** Groq Llama 3.3 70B (primary, 5-key rotation) → Cerebras Llama 3.3 70B → Google Gemini 2.0 Flash

**Storage:** Cloudflare R2

**Rendering:** GitHub Actions (Pillow + ffmpeg + Kokoro TTS)

---

## Tech Stack

| Layer | Technologies |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS, shadcn/ui |
| Backend | Python, FastAPI, LangGraph, LangChain |
| Database | Supabase (PostgreSQL + Auth + Row Level Security) |
| LLM inference | Groq, Cerebras, Google Gemini |
| Academic data | OpenAlex, Semantic Scholar, Crossref, Unpaywall, Tavily |
| Storage | Cloudflare R2 |
| Hosting | Vercel (frontend), Fly.io (backend), Supabase (database) |
| Async rendering | GitHub Actions |

---

## Access Control

- Email/password signup with email verification
- Admin approval gate before account activation
- Two tiers: Basic and Researcher (gates advanced tools)
- Per-user quota management for LLM-heavy features
- Admin panel for user approval, tier control, and quota adjustment

---

## Screenshots

*Screenshots and demo GIFs to be added.*

---

## Author

**Mohaimen Al Mamun**  
**North South University**
