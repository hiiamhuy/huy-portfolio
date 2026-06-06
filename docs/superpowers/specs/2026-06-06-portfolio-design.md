# Portfolio Website — Design Spec

**Date:** 2026-06-06
**Owner:** hiiamhuy (huynguyen206@gmail.com)
**Goal:** A simple, polished, single-page software/web-dev portfolio for a resume and a specific job application, hosted on GitHub Pages, buildable in one day.

## Summary

A hand-curated, single static page that showcases 4–6 of the owner's best
GitHub projects (drawn from two accounts, `hiiamhuy` and `film35`), plus a
downloadable résumé. No build step, no framework — the site itself doubles as
a work sample demonstrating clean, accessible front-end code.

## Hosting & URL

- **Repo name:** `hiiamhuy.github.io` (on the `hiiamhuy` account)
- **Live URL:** `https://hiiamhuy.github.io` (user/organization Pages — clean root URL, no subpath)
- **Deploy:** GitHub Pages serving `main` branch root. No build config required.

## Stack

- Plain `index.html` + `style.css` + a small `main.js`.
- No dependencies, no `node_modules`, no build pipeline.
- Rationale: lowest risk for a one-day deadline; nothing to break; instant deploy.

## File Layout

```
hiiamhuy.github.io/
├── index.html
├── style.css
├── main.js              # tiny: smooth-scroll nav, theme toggle (optional)
├── resume.md            # source of truth for the résumé
├── resume.pdf           # generated from resume.md; linked for download
├── assets/
│   ├── projects/        # project screenshots
│   └── favicon.svg
└── docs/superpowers/specs/2026-06-06-portfolio-design.md
```

## Page Structure (single scroll, anchored nav)

1. **Hero** — name, one-line title ("Software Developer"), short tagline.
   Links: GitHub, email, LinkedIn, and **Download Résumé (PDF)**.
2. **About** — 2–3 sentence intro.
3. **Featured Projects** — 4–6 curated cards. Each card: screenshot, title,
   1–2 line description, tech tags, **Repo** link, optional **Live demo** link.
   Projects may come from either the `hiiamhuy` or `film35` account; links are
   cross-account.
4. **Skills** — grouped lists (languages, frameworks, tools).
5. **Contact / Footer** — email + social links.

Sections explicitly excluded (YAGNI for one-day scope): blog, experience
timeline, testimonials.

## Résumé Handling

- `resume.md` is the editable source of truth, kept in the repo.
- `resume.pdf` is generated from it (e.g. `pandoc resume.md -o resume.pdf`,
  or a VS Code Markdown-to-PDF extension).
- The Hero links the **PDF** for download (recruiters expect PDF, not raw `.md`).

## Design Quality

- Responsive (mobile-first), works cleanly on phone and desktop.
- Semantic, accessible HTML; good color contrast; keyboard-navigable.
- Tasteful default theme (single accent color, generous whitespace, system
  font stack or one Google font). Optional light/dark toggle.
- Target a strong Lighthouse score since the site is itself a work sample.

## Deploy Flow

1. Build files locally.
2. `git add` + commit.
3. Create the `hiiamhuy.github.io` repo on GitHub (account: hiiamhuy).
4. `git push` to `main`.
5. Settings → Pages → source = `main` branch / root → site goes live at
   `https://hiiamhuy.github.io`.

## Content Needed From Owner (gathered during build)

- 4–6 featured repos: URL, which account, one-sentence description, tech tags,
  live-demo URL if any.
- Screenshots for each featured project (placeholders used until provided).
- Email, LinkedIn (and any other links) for hero/footer.
- `resume.md` content.

## Out of Scope

- Custom domain (can be added later via a `CNAME` file).
- Backend, contact form submission (mailto link only).
- Auto-pulling repos from the GitHub API (curation chosen instead).
