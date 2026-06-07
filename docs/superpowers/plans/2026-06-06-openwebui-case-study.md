# OpenWeb UI Case Study + Infra Repo Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans or superpowers:subagent-driven-development to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a case-study page (with a static-SVG RAG flow diagram) for the offline OpenWeb UI call-center assistant, promote it to a 4th featured card, and create a docs-only `hiiamhuy/offline-rag-assistant` GitHub repo to link.

**Architecture:** Same static-site pattern — a new case page under `projects/` sharing `style.css`/`main.js` via `../`, a Mermaid diagram pre-rendered to SVG with `mermaid-cli` + system Chrome, plus a small separate docs-only repo built as a sibling folder.

**Tech Stack:** HTML5/CSS3, `@mermaid-js/mermaid-cli` (one-time SVG), pandoc (résumé), git/gh.

**Portfolio root:** `/home/hiiamhuy/Documents/github/hiiamhuy.github.io`
**Infra repo root:** `/home/hiiamhuy/Documents/github/offline-rag-assistant`
**Spec:** `docs/superpowers/specs/2026-06-06-openwebui-case-study-design.md`

**Note on testing:** Static site / docs. "Verification" = grep gates + browser checks. Commit after each task.

---

### Task 1: Render the RAG flow diagram to SVG

**Files:** Create `projects/diagrams/openweb-rag.mmd` and generated `projects/diagrams/openweb-rag.svg`

- [ ] **Step 1: Create `projects/diagrams/openweb-rag.mmd`:**

```
graph TD
    A["Agent's question<br/>(via Tailscale)"] --> B["Open WebUI"]
    B --> C["Embed query<br/>all-MiniLM-L6-v2"]
    C --> D["Hybrid retrieval<br/>BM25 + vector"]
    D --> E[("Knowledge base<br/>ChromaDB")]
    E --> F["Rerank, top-3 chunks"]
    F --> G["Local Ollama model"]
    B --> G
    G --> H["Grounded answer<br/>to the agent"]

    subgraph Offline["Self-hosted, fully offline, no data leaves the network"]
        B
        C
        D
        E
        F
        G
    end
    style Offline fill:#0f2a1f,stroke:#15803d,color:#fff
    style E fill:#1f2937,stroke:#4b5563,color:#fff
```

- [ ] **Step 2: Render** (reuse the existing `puppeteer-config.json` at repo root):

```bash
cd /home/hiiamhuy/Documents/github/hiiamhuy.github.io/projects/diagrams
npx -y @mermaid-js/mermaid-cli@11 -i openweb-rag.mmd -o openweb-rag.svg -t dark -b transparent -p ../../puppeteer-config.json
ls -la openweb-rag.svg
```
Expected: `openweb-rag.svg` exists, non-zero size.

- [ ] **Step 3: Visually verify** — screenshot to PNG and inspect:
```bash
cd /home/hiiamhuy/Documents/github/hiiamhuy.github.io
google-chrome --headless --no-sandbox --disable-gpu --window-size=1000,720 --screenshot=/tmp/openweb-rag.png "file://$(pwd)/projects/diagrams/openweb-rag.svg"
```
Confirm the flow is legible (light text on dark, the offline subgraph boundary visible). If a node label shows stray HTML entities, simplify that label and re-render.

- [ ] **Step 4: Commit**
```bash
git add projects/diagrams/openweb-rag.mmd projects/diagrams/openweb-rag.svg
git commit -m "feat: pre-render OpenWeb UI RAG flow diagram to SVG"
```

---

### Task 2: Build `projects/openweb-ui.html`

**Files:** Create `projects/openweb-ui.html`

- [ ] **Step 1: Create the page** using the shared case template (from `2026-06-06-case-studies.md`) with:
  - `{TITLE}` = "Offline RAG Call-Center Assistant"
  - `{LABEL}` = "RAG Assistant"
  - `{SUBTITLE}` = "A self-hosted, fully-offline AI assistant that answers agents from the internal knowledge base — using Open WebUI's hybrid-search RAG over a local Ollama model."
  - `{TAGS}` = `<li>Open WebUI</li><li>Ollama</li><li>RAG</li><li>Docker</li><li>Tailscale</li>`
  - `{REPO_URL}` = `https://github.com/hiiamhuy/offline-rag-assistant`
  - Sections (first-person, ~400–500 words):
    1. `<h2>The problem</h2>` — agents need fast, accurate answers from a large internal knowledge base mid-call; hunting through wikis is slow, and a plain chatbot would confidently invent UW-specific procedures.
    2. `<h2>Why I built it</h2>` — a self-hosted assistant that answers *from* the knowledge base, runs entirely offline so call/account context never leaves the network, and is reachable by agents over Tailscale.
    3. `<h2>How it works</h2>` — Open WebUI front-end + a local Ollama model; the knowledge base is ingested into Open WebUI's RAG; on each question it retrieves the most relevant chunks and feeds them to the model for a grounded answer. Explain the deliberate choice of **hybrid search + reranking**: keyword (BM25) catches exact strings like NetIDs and error codes that pure semantic search misses, vector search catches paraphrases, and the reranker reorders so the single best chunk lands in the prompt.
    4. RAG flow diagram:
       ```html
       <figure class="diagram">
         <img src="diagrams/openweb-rag.svg" alt="RAG flow: an agent's question comes in over Tailscale to Open WebUI, the query is embedded, hybrid BM25+vector retrieval hits the ChromaDB knowledge base, results are reranked to the top-3 chunks, and a local Ollama model produces a grounded answer — all self-hosted and offline." />
         <figcaption>Question to grounded answer — embedding, hybrid retrieval, rerank, and local generation, all on-prem.</figcaption>
       </figure>
       ```
    5. `<h3>RAG configuration</h3>` — an HTML `<table>`:
       | Setting | Value |
       |---|---|
       | Embeddings | `all-MiniLM-L6-v2` (local SentenceTransformers) |
       | Vector store | ChromaDB |
       | Retrieval | Hybrid — BM25 keyword + semantic vector |
       | Reranking | Enabled (best chunk first) |
       | Chunking | ~1000 chars, ~100 overlap |
       | Top-k | 3 |
       | Generation | Local model via Ollama |
       | Access | Tailscale (private network) |
       | Privacy | Fully offline — no third-party API |
    6. `<h2>Outcome &amp; what I learned</h2>` — gave agents real-time, grounded answers and cut time spent hunting the knowledge base; the big lesson was that **retrieval quality caps answer quality** — chunking, hybrid weighting, and reranking matter as much as the model — plus the practicalities of running a private, fully-offline RAG stack.
  - Footer: Repo button → the repo URL; "← Back to portfolio" → `../index.html#projects`.

- [ ] **Step 2: Verify** —
  - `grep -c "diagrams/openweb-rag.svg" projects/openweb-ui.html` → `1`
  - `grep -c "offline-rag-assistant" projects/openweb-ui.html` → at least `1`
  - Open in a browser: diagram renders, table styled, nav/theme work.
- [ ] **Step 3: Commit**
```bash
git add projects/openweb-ui.html
git commit -m "feat: OpenWeb UI offline-RAG case study page"
```

---

### Task 3: Promote to a 4th featured card + remove from "Also built" (`index.html`)

**Files:** Modify `index.html`

- [ ] **Step 1: Add a 4th card** to the projects grid, immediately after the Pantheon Storage `</article>` (before the closing `</div>` of `.projects__grid`):

```html
        <article class="card">
          <div class="card__media" aria-hidden="true"><span>RAG Assistant</span></div>
          <div class="card__body">
            <h3 class="card__title">Offline RAG Call-Center Assistant</h3>
            <p class="card__desc">Self-hosted AI assistant that answers agents from the internal knowledge base. Open WebUI runs hybrid-search RAG (BM25 + semantic vector, with reranking) over a local Ollama model — fully offline, reachable over Tailscale, so no data leaves the network.</p>
            <ul class="card__tags">
              <li>Open WebUI</li><li>Ollama</li><li>RAG</li><li>Docker</li><li>Tailscale</li>
            </ul>
            <div class="card__links">
              <a href="https://github.com/hiiamhuy/offline-rag-assistant">Repo</a>
              <a href="projects/openweb-ui.html">Case study →</a>
            </div>
          </div>
        </article>
```

- [ ] **Step 2: Remove the OpenWeb UI line from "Also built."** Delete this `<li>` from the `.also__list`:
```html
        <li><strong>OpenWeb UI call-center assistant</strong> — self-hosted, fully offline AI support tool using RAG with Ollama, deployed with Docker and Tailscale for secure access.</li>
```
(The QuickBooks and Homelab items stay.)

- [ ] **Step 3: Verify** —
  - `grep -c "class=\"card\"" index.html` → `4`
  - `grep -c "projects/openweb-ui.html" index.html` → `1`
  - `grep -c "OpenWeb UI call-center assistant" index.html` → `0`
  - `grep -c "also__list" index.html` → still present; `grep -c "<li><strong>" index.html` within also-list → 2 items remain.
- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "feat: promote OpenWeb UI to a featured project card"
```

---

### Task 4: Enhance résumé line + regenerate PDF

**Files:** Modify `resume.md`, regenerate `resume.pdf`

- [ ] **Step 1: Replace the OpenWeb UI project line** in `resume.md`:
  - Old: `- **OpenWeb UI call-center assistant** — offline RAG assistant with Ollama, Docker, and Tailscale.`
  - New: `- **Offline RAG call-center assistant** — Open WebUI + local Ollama with hybrid-search RAG (BM25 + vector + reranking) over the internal knowledge base; Docker, Tailscale, fully offline.`
- [ ] **Step 2: Regenerate** — `pandoc resume.md -o resume.pdf && ls -la resume.pdf` (non-zero).
- [ ] **Step 3: Verify** — `grep -c "hybrid-search RAG" resume.md` → `1`.
- [ ] **Step 4: Commit**
```bash
git add resume.md resume.pdf
git commit -m "content: note hybrid-search RAG on assistant résumé line"
```

---

### Task 5: Create the docs-only `offline-rag-assistant` repo (local files)

**Files:** Create `/home/hiiamhuy/Documents/github/offline-rag-assistant/README.md` and `/LICENSE`

- [ ] **Step 1: Make the folder and `README.md`** with this content (no secrets, no compose, no data):

```markdown
# Offline RAG Call-Center Assistant

A self-hosted, fully-offline AI assistant that helps support agents by answering
their questions **from an internal knowledge base** in real time — built on
[Open WebUI](https://openwebui.com/) and a local [Ollama](https://ollama.com/)
model. This repository documents the architecture and configuration; it contains
no deployment secrets or internal data.

> 📄 Full write-up: https://hiiamhuy.github.io/huy-portfolio/projects/openweb-ui.html

## The problem

Call-center agents need fast, accurate answers from a large internal knowledge
base while they're on a call. Searching wikis by hand is slow, and a plain
chatbot would confidently invent organization-specific procedures. The assistant
had to be **accurate, grounded in the real docs, and private** — call and account
context can't be sent to a third-party cloud API.

## Architecture

| Layer | Technology |
|---|---|
| Front-end / RAG | Open WebUI (self-hosted) |
| LLM runtime | Ollama (local model) |
| Embeddings | `all-MiniLM-L6-v2` (local SentenceTransformers) |
| Vector store | ChromaDB |
| Retrieval | Hybrid — BM25 keyword + semantic vector |
| Reranking | Enabled (best chunk first) |
| Chunking | ~1000 chars, ~100 overlap |
| Top-k | 3 |
| Deployment | Docker, self-hosted |
| Access | Tailscale (private network) |
| Connectivity | Fully offline — no third-party API |

## How the RAG works

1. **Ingest** — the knowledge base is loaded into Open WebUI, chunked, embedded
   with a local model, and stored in ChromaDB.
2. **Retrieve** — each agent question runs **hybrid search**: BM25 keyword matching
   (for exact terms like NetIDs and error codes) plus semantic vector search (for
   paraphrases), then a reranker orders the results so the best chunk is first.
3. **Generate** — the top chunks are fed to the local Ollama model, which answers
   grounded in the retrieved context rather than from memory.

Everything runs on local hardware and is reachable only over Tailscale, so no
call data ever leaves the network.

## Why hybrid search + reranking

Pure semantic search misses exact tokens (a specific NetID, an error code);
pure keyword search misses paraphrased questions. Combining both — then reranking
— gets the single most relevant chunk into the prompt, which is what actually
drives answer quality in RAG.

## Outcome

- Real-time, grounded answers for agents instead of manual knowledge-base searches.
- Fully private: embeddings **and** generation run locally.
- A clear lesson: in RAG, retrieval quality (chunking, hybrid weighting, reranking)
  caps answer quality as much as the model does.

## License

MIT — see [LICENSE](LICENSE).
```

- [ ] **Step 2: Create `LICENSE`** (MIT, author "Huy Nguyen", year 2026) — standard MIT text.

- [ ] **Step 3: Init git and commit** (do NOT push yet — push happens in Task 6 after confirmation):
```bash
cd /home/hiiamhuy/Documents/github/offline-rag-assistant
git init -b main
git add README.md LICENSE
git commit -m "docs: offline RAG call-center assistant architecture"
```

- [ ] **Step 4: Verify** — `grep -c "no third-party API" README.md` → `1`; `grep -ci "secret\|password\|token\|\.env" README.md` → `0`.

---

### Task 6: Verify + deploy

**Files:** none (push only)

- [ ] **Step 1: Portfolio gate** (from portfolio root):
```bash
cd /home/hiiamhuy/Documents/github/hiiamhuy.github.io
ls projects/openweb-ui.html && ls projects/diagrams/openweb-rag.svg
grep -c "class=\"card\"" index.html   # expect 4
```
Expected: both files exist; 4 cards.

- [ ] **Step 2: Push the portfolio** (auto-deploys):
```bash
git push
```

- [ ] **Step 3: Create + push the infra repo** (CONFIRM with the owner first — this publishes a new public repo). Verify the name is free, then:
```bash
cd /home/hiiamhuy/Documents/github/offline-rag-assistant
gh repo create hiiamhuy/offline-rag-assistant --public --source=. --remote=origin --description "Offline RAG call-center assistant (Open WebUI + Ollama) — architecture & docs" --push
```
If the name is taken, fall back to `openwebui-callcenter-assistant` and update the two portfolio links (`index.html` card + `projects/openweb-ui.html` footer/repo) before pushing the portfolio.

- [ ] **Step 4: Verify live** — after ~1 min:
```bash
base="https://hiiamhuy.github.io/huy-portfolio"
curl -s -o /dev/null -w "%{http_code} case page\n" "$base/projects/openweb-ui.html"
curl -s -o /dev/null -w "%{http_code} diagram\n" "$base/projects/diagrams/openweb-rag.svg"
curl -s "$base/" | grep -c "projects/openweb-ui.html"   # 1
gh repo view hiiamhuy/offline-rag-assistant --json url -q .url
```
Expected: 200s, card link present, repo URL printed.

---

## Self-Review Notes

- **Spec coverage:** RAG flow diagram pre-rendered to SVG (T1) ✓; case page with sections, diagram, RAG-config table (T2) ✓; promote to 4th featured card + remove from Also built (T3) ✓; optional résumé enhancement (T4) ✓; docs-only repo README+LICENSE, no secrets (T5) ✓; deploy portfolio + create/push public repo with confirmation + fallback name (T6) ✓; confirmed RAG facts (all-MiniLM-L6-v2, ChromaDB, hybrid+rerank, 1000/100, top-3, Ollama, Tailscale, offline) used consistently in T2 table, T1 diagram, and T5 README.
- **Placeholders:** none. No "TBD"/"implement later". Repo-name fallback is an explicit branch, not a placeholder.
- **Consistency:** reuses the shared case template + `.diagram`/`.case`/`card__tags`/`btn` classes; diagram `<img src="diagrams/openweb-rag.svg">` matches the file built in T1; repo URL `github.com/hiiamhuy/offline-rag-assistant` identical across T2 page, T3 card, and T5/T6 repo; the "4 cards" count in T3/T6 matches adding one card to the existing three.
```