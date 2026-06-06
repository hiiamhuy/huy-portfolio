# Portfolio Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page, hand-curated software-dev portfolio and deploy it to GitHub Pages at `https://hiiamhuy.github.io`.

**Architecture:** Static site — one `index.html` with semantic sections (hero, about, projects, skills, footer), one `style.css`, one tiny `main.js` (smooth-scroll nav + light/dark toggle). Project content is hardcoded in HTML (no JS dependency, better SEO/accessibility, works as a work sample). Résumé kept as `resume.md` (source of truth) and exported to `resume.pdf` for download.

**Tech Stack:** HTML5, CSS3 (custom properties, fl/grid, media queries), vanilla JS, GitHub Pages. No build step, no dependencies. Pandoc (optional) for résumé PDF.

**Note on testing:** This is a static site with no application logic to unit-test. "Verification" steps here mean: open the file in a browser, visually confirm rendering, check responsive behavior, and run a Lighthouse audit. Commit after each task.

**Project root:** `/home/hiiamhuy/Documents/github/hiiamhuy.github.io`

---

### Task 1: Scaffold project files

**Files:**
- Create: `.gitignore`
- Create: `README.md`
- Create: `assets/projects/.gitkeep`

- [ ] **Step 1: Create `.gitignore`**

```
# OS / editor cruft
.DS_Store
Thumbs.db
*.swp
.vscode/
.idea/
```

- [ ] **Step 2: Create `README.md`**

```markdown
# hiiamhuy.github.io

Personal portfolio site. Live at https://hiiamhuy.github.io

Plain static HTML/CSS/JS — no build step. Edit `index.html` and `style.css`
directly; push to `main` to deploy via GitHub Pages.

- `resume.md` — résumé source of truth
- `resume.pdf` — generated download (`pandoc resume.md -o resume.pdf`)
```

- [ ] **Step 3: Keep the assets dir tracked**

Run: `mkdir -p assets/projects && touch assets/projects/.gitkeep && touch assets/favicon.svg`

- [ ] **Step 4: Verify structure**

Run: `ls -R . | grep -v .git`
Expected: shows `.gitignore`, `README.md`, `assets/projects/.gitkeep`, `assets/favicon.svg`, `docs/...`

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "chore: scaffold portfolio project files"
```

---

### Task 2: Build the HTML structure

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write `index.html`**

Sample/placeholder content is used (name, links, projects) — Task 6 replaces it with the owner's real content. Structure and class names here must match the CSS in Task 3 and the JS in Task 4.

```html
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Huy Nguyen — Software Developer</title>
  <meta name="description" content="Software developer portfolio — selected projects and résumé." />
  <link rel="icon" href="assets/favicon.svg" type="image/svg+xml" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <header class="nav">
    <a class="nav__brand" href="#top">HN</a>
    <nav class="nav__links">
      <a href="#projects">Projects</a>
      <a href="#skills">Skills</a>
      <a href="#contact">Contact</a>
      <a class="nav__resume" href="resume.pdf" download>Résumé</a>
      <button id="theme-toggle" class="nav__theme" aria-label="Toggle color theme">◐</button>
    </nav>
  </header>

  <main id="top">
    <!-- HERO -->
    <section class="hero">
      <p class="hero__eyebrow">Hi, I'm</p>
      <h1 class="hero__name">Huy Nguyen</h1>
      <p class="hero__title">Software Developer</p>
      <p class="hero__tagline">I build clean, reliable web applications and developer tools.</p>
      <div class="hero__actions">
        <a class="btn btn--primary" href="resume.pdf" download>Download Résumé</a>
        <a class="btn" href="#projects">View Projects</a>
      </div>
      <ul class="hero__links">
        <li><a href="https://github.com/hiiamhuy">GitHub</a></li>
        <li><a href="mailto:huynguyen206@gmail.com">Email</a></li>
        <li><a href="#" data-edit="linkedin">LinkedIn</a></li>
      </ul>
    </section>

    <!-- ABOUT -->
    <section class="about" id="about">
      <h2 class="section__title">About</h2>
      <p>
        Short 2–3 sentence intro goes here. Who you are, what you build, and what
        kind of role you're looking for. (Replaced in Task 6.)
      </p>
    </section>

    <!-- PROJECTS -->
    <section class="projects" id="projects">
      <h2 class="section__title">Featured Projects</h2>
      <div class="projects__grid">

        <!-- PROJECT CARD TEMPLATE (duplicate per project, 4–6 total) -->
        <article class="card">
          <div class="card__media">
            <img src="assets/projects/placeholder.png" alt="Screenshot of Project One" loading="lazy" />
          </div>
          <div class="card__body">
            <h3 class="card__title">Project One</h3>
            <p class="card__desc">One or two line description of what it does and why it's notable.</p>
            <ul class="card__tags">
              <li>TypeScript</li>
              <li>React</li>
              <li>Node</li>
            </ul>
            <div class="card__links">
              <a href="https://github.com/hiiamhuy/project-one">Repo</a>
              <a href="#">Live demo</a>
            </div>
          </div>
        </article>

        <article class="card">
          <div class="card__media">
            <img src="assets/projects/placeholder.png" alt="Screenshot of Project Two" loading="lazy" />
          </div>
          <div class="card__body">
            <h3 class="card__title">Project Two</h3>
            <p class="card__desc">One or two line description.</p>
            <ul class="card__tags">
              <li>Python</li>
              <li>Flask</li>
            </ul>
            <div class="card__links">
              <a href="https://github.com/film35/project-two">Repo</a>
            </div>
          </div>
        </article>

        <article class="card">
          <div class="card__media">
            <img src="assets/projects/placeholder.png" alt="Screenshot of Project Three" loading="lazy" />
          </div>
          <div class="card__body">
            <h3 class="card__title">Project Three</h3>
            <p class="card__desc">One or two line description.</p>
            <ul class="card__tags">
              <li>Go</li>
            </ul>
            <div class="card__links">
              <a href="https://github.com/hiiamhuy/project-three">Repo</a>
            </div>
          </div>
        </article>

      </div>
    </section>

    <!-- SKILLS -->
    <section class="skills" id="skills">
      <h2 class="section__title">Skills</h2>
      <div class="skills__groups">
        <div class="skills__group">
          <h3>Languages</h3>
          <ul><li>JavaScript / TypeScript</li><li>Python</li><li>Go</li></ul>
        </div>
        <div class="skills__group">
          <h3>Frameworks</h3>
          <ul><li>React</li><li>Node / Express</li><li>Flask</li></ul>
        </div>
        <div class="skills__group">
          <h3>Tools</h3>
          <ul><li>Git</li><li>Docker</li><li>Linux</li></ul>
        </div>
      </div>
    </section>
  </main>

  <footer class="footer" id="contact">
    <h2 class="section__title">Get in touch</h2>
    <ul class="footer__links">
      <li><a href="mailto:huynguyen206@gmail.com">huynguyen206@gmail.com</a></li>
      <li><a href="https://github.com/hiiamhuy">github.com/hiiamhuy</a></li>
      <li><a href="#" data-edit="linkedin">LinkedIn</a></li>
    </ul>
    <p class="footer__note">© 2026 Huy Nguyen. Built with plain HTML & CSS.</p>
  </footer>

  <script src="main.js"></script>
</body>
</html>
```

- [ ] **Step 2: Verify it renders**

Run: `xdg-open index.html` (or open the file in a browser)
Expected: page shows nav, hero with name/buttons, three project cards (broken image placeholders are fine for now), skills, footer. No console errors except missing `placeholder.png`/`style.css` until later tasks.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add portfolio HTML structure"
```

---

### Task 3: Style the site

**Files:**
- Create: `style.css`

- [ ] **Step 1: Write `style.css`**

Uses CSS custom properties so the `data-theme` attribute (toggled in Task 4) swaps colors. Class names match Task 2 exactly.

```css
:root {
  --max-width: 920px;
  --accent: #6ea8fe;
  --radius: 12px;
  --gap: 1.5rem;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

html[data-theme="dark"] {
  --bg: #0f1115;
  --surface: #181b22;
  --text: #e8eaed;
  --muted: #9aa0aa;
  --border: #2a2e37;
}
html[data-theme="light"] {
  --bg: #ffffff;
  --surface: #f5f6f8;
  --text: #1a1d22;
  --muted: #5b616b;
  --border: #e2e5ea;
}

* { box-sizing: border-box; }
body {
  margin: 0;
  background: var(--bg);
  color: var(--text);
  line-height: 1.6;
}
a { color: var(--accent); text-decoration: none; }
a:hover { text-decoration: underline; }

/* NAV */
.nav {
  position: sticky; top: 0; z-index: 10;
  display: flex; justify-content: space-between; align-items: center;
  padding: 0.9rem 1.25rem;
  background: color-mix(in srgb, var(--bg) 85%, transparent);
  backdrop-filter: blur(8px);
  border-bottom: 1px solid var(--border);
}
.nav__brand { font-weight: 700; color: var(--text); }
.nav__links { display: flex; align-items: center; gap: 1.1rem; }
.nav__resume { border: 1px solid var(--accent); padding: 0.3rem 0.7rem; border-radius: 999px; }
.nav__theme { background: none; border: none; color: var(--text); font-size: 1.1rem; cursor: pointer; }

/* LAYOUT */
main, .footer { max-width: var(--max-width); margin: 0 auto; padding: 0 1.25rem; }
section { padding: 4rem 0 2rem; }
.section__title { font-size: 1.5rem; margin-bottom: 1.25rem; }

/* HERO */
.hero { padding-top: 5rem; }
.hero__eyebrow { color: var(--muted); margin: 0; }
.hero__name { font-size: clamp(2.2rem, 6vw, 3.5rem); margin: 0.2rem 0; }
.hero__title { font-size: 1.3rem; color: var(--accent); margin: 0; }
.hero__tagline { color: var(--muted); max-width: 40ch; }
.hero__actions { display: flex; gap: 0.8rem; margin: 1.5rem 0; flex-wrap: wrap; }
.hero__links { display: flex; gap: 1.2rem; list-style: none; padding: 0; }

.btn {
  display: inline-block; padding: 0.6rem 1.1rem; border-radius: var(--radius);
  border: 1px solid var(--border); color: var(--text); font-weight: 600;
}
.btn:hover { text-decoration: none; border-color: var(--accent); }
.btn--primary { background: var(--accent); color: #0f1115; border-color: var(--accent); }

/* PROJECTS */
.projects__grid {
  display: grid; gap: var(--gap);
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
}
.card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); overflow: hidden;
  display: flex; flex-direction: column; transition: transform .15s ease, border-color .15s ease;
}
.card:hover { transform: translateY(-3px); border-color: var(--accent); }
.card__media img { width: 100%; aspect-ratio: 16/9; object-fit: cover; display: block; background: var(--border); }
.card__body { padding: 1rem; display: flex; flex-direction: column; gap: 0.6rem; flex: 1; }
.card__title { margin: 0; }
.card__desc { color: var(--muted); margin: 0; flex: 1; }
.card__tags { display: flex; flex-wrap: wrap; gap: 0.4rem; list-style: none; padding: 0; margin: 0; }
.card__tags li { font-size: 0.75rem; padding: 0.15rem 0.55rem; border: 1px solid var(--border); border-radius: 999px; color: var(--muted); }
.card__links { display: flex; gap: 1rem; }

/* SKILLS */
.skills__groups { display: grid; gap: var(--gap); grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); }
.skills__group ul { list-style: none; padding: 0; color: var(--muted); }
.skills__group h3 { margin-bottom: 0.5rem; }

/* FOOTER */
.footer { padding: 3rem 1.25rem 4rem; border-top: 1px solid var(--border); margin-top: 3rem; }
.footer__links { display: flex; flex-wrap: wrap; gap: 1.2rem; list-style: none; padding: 0; }
.footer__note { color: var(--muted); font-size: 0.85rem; }

@media (max-width: 520px) {
  .nav__links { gap: 0.7rem; font-size: 0.9rem; }
}
```

- [ ] **Step 2: Verify styling**

Run: reload `index.html` in the browser.
Expected: dark theme, sticky blurred nav, large hero, cards in a responsive grid that hover-lift, pill-shaped tags. Resize the window narrow — grid collapses to one column, nav stays usable.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: add portfolio styling with light/dark theme vars"
```

---

### Task 4: Add interactivity (`main.js`)

**Files:**
- Create: `main.js`

- [ ] **Step 1: Write `main.js`**

```js
// Smooth scroll for in-page anchor links
document.querySelectorAll('a[href^="#"]').forEach((link) => {
  link.addEventListener('click', (e) => {
    const target = document.querySelector(link.getAttribute('href'));
    if (target) {
      e.preventDefault();
      target.scrollIntoView({ behavior: 'smooth' });
    }
  });
});

// Theme toggle with persistence
const root = document.documentElement;
const toggle = document.getElementById('theme-toggle');
const saved = localStorage.getItem('theme');
if (saved) root.setAttribute('data-theme', saved);

toggle?.addEventListener('click', () => {
  const next = root.getAttribute('data-theme') === 'dark' ? 'light' : 'dark';
  root.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);
});
```

- [ ] **Step 2: Verify behavior**

Run: reload `index.html`.
Expected: clicking nav links (Projects/Skills/Contact) smooth-scrolls. Clicking the ◐ button flips dark↔light and the choice survives a page reload. No console errors.

- [ ] **Step 3: Commit**

```bash
git add main.js
git commit -m "feat: add smooth-scroll nav and persistent theme toggle"
```

---

### Task 5: Résumé source + PDF

**Files:**
- Create: `resume.md`
- Create: `resume.pdf` (generated)

- [ ] **Step 1: Create `resume.md` skeleton**

Owner pastes their real résumé content here in Task 6. This is a valid starter so the PDF export works.

```markdown
# Huy Nguyen
Software Developer · huynguyen206@gmail.com · github.com/hiiamhuy

## Summary
One-paragraph professional summary.

## Experience
**Role — Company** (dates)
- Accomplishment with impact/metric.

## Projects
- **Project One** — what it does, tech used.

## Skills
JavaScript/TypeScript, Python, Go, React, Node, Docker, Git, Linux.

## Education
Degree — Institution (year)
```

- [ ] **Step 2: Generate the PDF**

Run (if pandoc + a LaTeX engine are installed):
`pandoc resume.md -o resume.pdf`
Expected: `resume.pdf` created.

If pandoc is NOT available, fallback: open `resume.md` in VS Code with the "Markdown PDF" extension and export, OR print-to-PDF from a browser preview. Confirm `resume.pdf` exists:
Run: `ls -la resume.pdf`
Expected: file present, non-zero size.

- [ ] **Step 3: Verify the download link works**

Run: reload `index.html`, click "Download Résumé".
Expected: `resume.pdf` opens/downloads.

- [ ] **Step 4: Commit**

```bash
git add resume.md resume.pdf
git commit -m "feat: add résumé source and generated PDF"
```

---

### Task 6: Populate real content

This is the content-gathering task. Collect from the owner and replace all placeholder text. **Do not invent facts** — ask the owner for each item.

**Files:**
- Modify: `index.html`
- Modify: `resume.md` (then regenerate `resume.pdf`)
- Add: `assets/projects/*.png` screenshots

- [ ] **Step 1: Collect content from the owner**

Gather:
- Tagline + 2–3 sentence About text.
- LinkedIn URL (replace both `data-edit="linkedin"` links; remove if none).
- 4–6 featured projects: title, description, tech tags, repo URL (hiiamhuy or film35), live-demo URL (omit the demo link if none), screenshot image.
- Real skills lists.
- Full résumé content for `resume.md`.

- [ ] **Step 2: Replace placeholders in `index.html`**

For each project card: update `card__title`, `card__desc`, `card__tags`, `card__links` href(s), and the `<img src>` + `alt`. Add or remove `<article class="card">` blocks so there are 4–6. Update hero tagline, About paragraph, skills, and LinkedIn hrefs. Remove any `data-edit="linkedin"` placeholder you didn't fill.

- [ ] **Step 3: Add screenshots**

Place each image in `assets/projects/` and point the card `<img src>` at it. Recommended ~1200×675 (16:9) PNG/JPG.

- [ ] **Step 4: Update résumé + regenerate PDF**

Edit `resume.md` with real content, then re-run the Task 5 Step 2 export.

- [ ] **Step 5: Verify final content**

Run: reload `index.html`.
Expected: all real text, working repo links (click each — they resolve to the right account), screenshots load, no leftover "Project One"/placeholder strings.
Run: `grep -n "placeholder\|Project One\|data-edit" index.html`
Expected: no matches (or only intentional ones).

- [ ] **Step 6: Commit**

```bash
git add -A
git commit -m "content: add real projects, about, skills, and résumé"
```

---

### Task 7: Deploy to GitHub Pages

**Files:** none (remote setup)

- [ ] **Step 1: Create the repo on GitHub**

The repo MUST be named exactly `hiiamhuy.github.io` and owned by the `hiiamhuy` account (this triggers root-URL user Pages).
Run (if `gh` is installed and authed as hiiamhuy):
`gh repo create hiiamhuy.github.io --public --source=. --remote=origin`
Otherwise: create it manually at github.com, then:
`git remote add origin git@github.com:hiiamhuy/hiiamhuy.github.io.git`

- [ ] **Step 2: Push**

```bash
git push -u origin main
```
Expected: branch `main` pushed to origin.

- [ ] **Step 3: Enable Pages**

In the repo: Settings → Pages → Build and deployment → Source = "Deploy from a branch", Branch = `main`, folder = `/ (root)` → Save.
(For a `*.github.io` user repo, Pages is often auto-enabled on push — verify the setting regardless.)

- [ ] **Step 4: Verify live site**

Wait ~1–2 min, then visit `https://hiiamhuy.github.io`.
Expected: site loads with HTTPS, all sections render, résumé downloads, project links work, theme toggle works.
Run a Lighthouse audit (Chrome DevTools → Lighthouse) — aim for 90+ on Accessibility and Best Practices.

- [ ] **Step 5: Final commit (if any fixes)**

```bash
git add -A
git commit -m "fix: post-deploy polish"
git push
```

---

## Self-Review Notes

- **Spec coverage:** hosting/URL (T1,T7) ✓, stack/file layout (T1–T5) ✓, all 5 page sections (T2) ✓, résumé md→pdf (T5) ✓, design quality/responsive/theme (T3,T4) ✓, deploy flow (T7) ✓, curated cross-account projects (T2,T6) ✓, excluded sections honored (no blog/timeline) ✓.
- **Placeholders:** sample content in T2 is intentional and explicitly removed in T6 (with a `grep` gate). No "TBD"/"implement later" steps.
- **Consistency:** class names (`card`, `card__title`, `nav__theme`, `data-theme`, `theme-toggle`) match across HTML (T2), CSS (T3), and JS (T4).
