# Portfolio Website — Design Spec

**Date:** 2026-06-06 (revised)
**Owner:** Huy Nguyen (hiiamhuy@uw.edu, github.com/hiiamhuy)
**Goal:** A polished single-page portfolio for a job search, hosted on GitHub Pages, that honors Huy's ~10-year UW-IT career *and* his recent AI/automation projects.

## Positioning

Headline identity: **IT & Automation Specialist** — a blend that reflects the
real résumé (a long IT/web-publishing career at UW-IT) while foregrounding the
modern dev/AI work (WhisperX/Ollama, RAG, n8n, FastAPI). Not framed as a pure
software-engineer site, not framed as a pure helpdesk site.

## Status

The static scaffold already exists and is committed (HTML/CSS/JS, résumé
md→pdf pipeline, theme toggle, smooth-scroll nav). This revision covers two
things: (1) populating real content, and (2) one structural addition — an
**Experience** section the original scaffold did not have.

## Stack (unchanged)

- Plain `index.html` + `style.css` + small `main.js`. No build step, no deps.
- Single page, dark default theme with a persisted light/dark toggle.
- The site doubles as a clean, accessible front-end work sample.

## Hosting & URL

- **Repo:** `hiiamhuy.github.io` (account: `hiiamhuy`)
- **Live URL:** `https://hiiamhuy.github.io` (root user-Pages URL)
- **Deploy:** GitHub Pages from `main` branch root. No build config.

## Page Structure (single scroll, anchored nav)

1. **Hero** — "Huy Nguyen / IT & Automation Specialist". Tagline:
   *"I keep systems running and automate the tedious parts — from UW-IT service
   operations to AI-driven workflows."* Buttons: Download Résumé, View Projects.
   Links: GitHub, Email (`hiiamhuy@uw.edu`), LinkedIn.
2. **About** — 2–3 sentences: UW Informatics + American Ethnic Studies grad
   (2018); ~10 years at UW-IT across Tier-1 support → web publishing → service
   operations; now builds AI/automation tooling. ITIL 4 Foundations certified.
3. **Experience** (NEW) — compact timeline from the résumé:
   - Senior Computer Specialist, UW-IT Service Center — 2019–present
   - Senior Computer Specialist, User Consulting Support, UW-IT — *(dates: placeholder)*
   - Identity & Access Management Student Lead, UW-IT Service Center — 2014–2018
   - Student IT Consultant, UW-IT Service Center — 2013–2015
   - One-line "earlier": Web Intern, OCA-Greater Seattle (2015); Digital
     Connector, Cisco (2010–2011).
   Each role: 1–2 condensed bullets of impact.
4. **Featured Projects** — 3 cards, each with a real repo:
   - **Call Center QA — Transcription & Analysis** — automated call
     transcription + LLM quality scoring. WhisperX (speech-to-text + speaker
     diarization) and Ollama (Llama 3.2) on UW Hyak via SLURM/GPU containers;
     rubric-based scoring routes calls to review queues. Tags: Python, WhisperX,
     Ollama, SLURM, Docker. Repo: https://github.com/hiiamhuy/uwitsc-call-analysis
   - **MRBS → TRMNL E-Ink Integration** — FastAPI service that pushes classroom
     booking schedules to e-ink displays via MRBS webhooks + a background
     scheduler. Tags: FastAPI, SQLAlchemy, MySQL, Docker. Repo:
     `https://github.com/hiiamhuy/Yanko-MRBS-FASTAPI` *(URL to confirm)*
   - **Pantheon Storage Analysis** — Bash tool with platform auto-detection
     (WordPress/Drupal/generic) and multi-format export (JSON/CSV/HTML/MD).
     Tags: Bash, Terminus, WP-CLI/Drush. Repo:
     https://github.com/hiiamhuy/storage-analysis
5. **Also built** (secondary list, no repo links) — compact text list:
   - n8n QuickBooks + receipt-OCR bookkeeping automation
   - OpenWeb UI offline call-center assistant (RAG + Ollama, Docker + Tailscale)
   - Homelab builds — TrueNAS (ZFS), unRAID, Synology 3-2-1 backup
6. **Skills** — three groups:
   - *Languages*: Python, JavaScript, HTML/CSS, SQL, Bash
   - *AI & Automation*: WhisperX, Ollama, RAG, n8n, LM Studio, OpenWeb UI
   - *Platforms & Tools*: Docker, Linux, Pantheon, ServiceNow, Tailscale, Git
7. **Footer / Contact** — `hiiamhuy@uw.edu`, GitHub, LinkedIn, ITIL 4 note.

## Content Changes From Scaffold

- Replace **all** `huynguyen206@gmail.com` references with `hiiamhuy@uw.edu`
  (hero links, footer, `mailto:`, `resume.md`).
- Drop the `film35` cross-account assumption — single account `hiiamhuy`.
- Add the Experience section (new markup; reuse a simple list/timeline style
  consistent with existing CSS variables, no new framework).
- Rewrite `resume.md` from the real résumé content and regenerate `resume.pdf`.

## Résumé Handling

- `resume.md` is the source of truth; rewritten with real experience, projects,
  skills, education, ITIL 4 cert, and `hiiamhuy@uw.edu`.
- `resume.pdf` regenerated from it (`pandoc resume.md -o resume.pdf`, or a
  Markdown-to-PDF fallback). Hero "Download Résumé" links the PDF.

## Design Quality (unchanged targets)

- Responsive/mobile-first, semantic + accessible HTML, good contrast,
  keyboard-navigable. Target strong Lighthouse Accessibility/Best-Practices.

## Open Items / Placeholders (non-blocking)

- **LinkedIn URL** — real profile link (replace `data-edit="linkedin"`
  placeholders; remove if none).
- **MRBS repo URL** — confirm `github.com/hiiamhuy/Yanko-MRBS-FASTAPI`.
- **User Consulting Support dates** — fill the date range.
- **Location** — assume Seattle, WA unless corrected.
- **Project screenshots** — placeholders until real images provided.
- **resume.pdf** — confirm PDF regeneration tool available locally.

## Out of Scope (YAGNI)

- No framework, build step, analytics, contact form, or blog.
- Custom domain deferred (can add a `CNAME` later).
- No GitHub-API auto-pulling of repos (curation chosen instead).
