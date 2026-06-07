# OpenWeb UI Case Study + Infra Repo — Design Spec

**Date:** 2026-06-06
**Owner:** Huy Nguyen (hiiamhuy@uw.edu)
**Goal:** Give the OpenWeb UI offline call-center assistant a real presence: a docs-only GitHub repo to link, a full case-study page (with a RAG flow diagram), and promotion to a featured project on the homepage.

**Parent context:** Extends the portfolio at https://hiiamhuy.github.io/huy-portfolio/ and the case-study pattern in `2026-06-06-case-studies-design.md`.

## Confirmed RAG facts (owner's actual setup)

Open WebUI's built-in RAG, fully local: default **`all-MiniLM-L6-v2`** SentenceTransformers embeddings → **ChromaDB** vector store → **hybrid search (BM25 keyword + semantic vector) with reranking** → **top-3** chunks (default chunk size 1000 / overlap 100) injected into a **local Ollama** model. Reachable over Tailscale; nothing leaves the network.

## Deliverable A — Repo `hiiamhuy/offline-rag-assistant` (docs only)

A separate public GitHub repo, built as a sibling folder
`/home/hiiamhuy/Documents/github/offline-rag-assistant/`, containing:

- **`README.md`** — the documentation/architecture writeup: overview; the problem;
  the stack (Open WebUI + Ollama + Docker + Tailscale, fully offline); the **RAG
  configuration** (the facts above, as a table); deployment + access model
  (self-hosted, offline, Tailscale); and outcomes. Links back to the portfolio
  case study.
- **`LICENSE`** — MIT.

**Hard constraint:** no secrets, no internal UW knowledge-base content, no real
data, no `.env`. This repo documents the *design*, not deployment artifacts
(owner chose "README/docs only" — no compose files).

The repo will be created on GitHub and pushed (outward-facing — confirm before
the public push).

## Deliverable B — Portfolio changes (huy-portfolio repo)

### B1. New case-study page `projects/openweb-ui.html`

Uses the shared case template (`../` paths, theme toggle, `.case` styles).
First-person tone, ~400–500 words. Sections:
1. **The problem** — call-center agents need fast, accurate answers from a large
   internal knowledge base mid-call; searching wikis by hand is slow, and a plain
   chatbot would hallucinate UW-specific procedures.
2. **Why I built it** — a self-hosted assistant that answers *from* the knowledge
   base, runs entirely offline (privacy — call/account context never leaves the
   network), and is reachable by agents over Tailscale.
3. **How it works** — Open WebUI front-end + local Ollama model; the knowledge
   base is ingested into Open WebUI's RAG; on a question it retrieves the relevant
   chunks and feeds them to the model for a grounded answer. Explain **why hybrid
   search + reranking** (keyword catches exact terms like NetIDs/error codes that
   pure semantic search misses; the reranker puts the best chunk first).
4. **RAG flow diagram** — the pre-rendered SVG (see B3).
5. **RAG configuration table** — embeddings, vector store, retrieval mode, chunking,
   top-k, LLM, access, privacy boundary.
6. **Outcome & what I learned** — improved agent productivity / faster resolutions;
   learned how retrieval quality (chunking, hybrid weighting, reranking) drives
   answer quality, and how to run a private, fully-offline RAG stack.

### B2. Promote to a 4th featured card (`index.html`)

- Add a 4th `<article class="card">` to the Featured Projects grid:
  banner `<span>RAG Assistant</span>`; title "Offline RAG Call-Center Assistant";
  description (Open WebUI + local Ollama, hybrid-search RAG over the knowledge
  base, fully offline, Tailscale access); tags `Open WebUI`, `Ollama`, `RAG`,
  `Docker`, `Tailscale`; links: Repo → `https://github.com/hiiamhuy/offline-rag-assistant`
  and `Case study →` → `projects/openweb-ui.html`.
- **Remove** the "OpenWeb UI call-center assistant" line from the "Also built"
  list (it's now a featured card). The other two "Also built" items remain.

### B3. RAG flow diagram (`projects/diagrams/openweb-rag.mmd` → `.svg`)

A Mermaid flowchart, pre-rendered to static SVG with `mermaid-cli` + system
Chrome (`-t dark -b transparent`), embedded in a `.diagram` wrapper — same
pipeline as the call-analysis diagrams. Flow:

agent question (via Tailscale) → Open WebUI → embed query (all-MiniLM-L6-v2) →
hybrid retrieval from ChromaDB knowledge base (BM25 + vector) → rerank → top-3
chunks → local Ollama model → grounded answer back to agent; a subgraph/boundary
marks "self-hosted · offline."

### B4. Résumé (optional, light)

In `resume.md`, enhance the OpenWeb UI project line to name the hybrid-RAG detail
(e.g. "offline RAG (hybrid search + reranking) assistant with Open WebUI, Ollama,
Docker, Tailscale"); regenerate `resume.pdf`.

## Out of Scope (YAGNI)

- No compose files / deployment artifacts in the repo (docs only).
- No runtime diagram rendering / CDN (static SVG only).
- No screenshots required (the existing offline tool isn't publicly demoable).

## Open Items (non-blocking)

- Confirm `offline-rag-assistant` repo name is free before creating (fallback:
  `openwebui-callcenter-assistant`).
- Outcome metrics are qualitative (résumé wording) — no hard numbers claimed.
