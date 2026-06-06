# Portfolio Content & Deploy Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Populate the existing static portfolio scaffold with Huy Nguyen's real content (IT & Automation Specialist positioning, Experience timeline, 3 real projects, "Also built" list, real skills, `hiiamhuy@uw.edu`), regenerate the résumé PDF, and deploy to GitHub Pages.

**Architecture:** The static scaffold (HTML/CSS/JS, theme toggle, smooth-scroll nav, résumé md→pdf) already exists and is committed. This plan (1) appends CSS for two new section types — an Experience timeline and an "Also built" list — and a gradient card banner that replaces broken placeholder images; (2) rewrites `index.html` body content; (3) rewrites `resume.md` and regenerates `resume.pdf`; (4) verifies and deploys. No build step, no dependencies.

**Tech Stack:** HTML5, CSS3 (custom properties), vanilla JS (unchanged), GitHub Pages. Pandoc (optional) for the résumé PDF.

**Project root:** `/home/hiiamhuy/Documents/github/hiiamhuy.github.io`

**Note on testing:** Static site, no application logic to unit-test. "Verification" = open in a browser, confirm rendering/responsiveness, and run grep gates for leftover placeholders. Commit after each task.

**Spec:** `docs/superpowers/specs/2026-06-06-portfolio-design.md`

---

### Task 1: Add CSS for Experience timeline, gradient card banner, and "Also built" list

**Files:**
- Modify: `style.css` (append at end, after line 100)

- [ ] **Step 1: Append the new styles to `style.css`**

Add these blocks at the end of the file (after the existing `@media` block). They reuse the existing CSS custom properties (`--accent`, `--surface`, `--border`, `--muted`, `--text`, `--gap`). The `.card__media` rule adds a gradient banner so cards look intentional without screenshots (real images can be dropped in later by replacing the banner markup with an `<img>`).

```css

/* EXPERIENCE */
.timeline { display: flex; flex-direction: column; gap: 1.5rem; }
.timeline__item { border-left: 2px solid var(--border); padding-left: 1.1rem; }
.timeline__head { display: flex; flex-wrap: wrap; justify-content: space-between; align-items: baseline; gap: 0.3rem; }
.timeline__role { margin: 0; font-size: 1.05rem; }
.timeline__org { color: var(--accent); }
.timeline__dates { color: var(--muted); font-size: 0.85rem; white-space: nowrap; }
.timeline__points { margin: 0.4rem 0 0; padding-left: 1.1rem; color: var(--muted); }
.timeline__earlier { color: var(--muted); font-size: 0.9rem; margin-top: 0.5rem; }

/* CARD BANNER (placeholder until real screenshots are added) */
.card__media {
  aspect-ratio: 16 / 9; display: flex; align-items: flex-end; padding: 0.8rem;
  background: linear-gradient(135deg, color-mix(in srgb, var(--accent) 35%, var(--surface)), var(--surface));
}
.card__media span { font-weight: 700; color: var(--text); font-size: 0.95rem; }

/* ALSO BUILT */
.also__list { list-style: none; padding: 0; margin: 0; display: grid; gap: 0.8rem; }
.also__list li { color: var(--muted); padding-left: 1.2rem; position: relative; }
.also__list li::before { content: "\25B9"; position: absolute; left: 0; color: var(--accent); }
.also__list strong { color: var(--text); font-weight: 600; }
```

- [ ] **Step 2: Verify CSS is valid**

Run: `npx --yes csstree-validator style.css 2>/dev/null || echo "validator unavailable — skip"`
Expected: no errors reported (or the skip message if the validator isn't installed). Either is acceptable.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: add timeline, card banner, and also-built styles"
```

---

### Task 2: Rewrite `index.html` with real content

**Files:**
- Modify: `index.html` (full body replacement)

- [ ] **Step 1: Replace the entire contents of `index.html`**

Write the file exactly as below. This sets the IT & Automation Specialist positioning, adds the Experience and "Also built" sections, fills the 3 real projects (gradient banners instead of `<img>`), real skills, and changes every email to `hiiamhuy@uw.edu`. The LinkedIn links keep a `data-edit="linkedin"` marker because the URL is still pending — leave it until the owner provides the URL (then replace `href="#"` and remove the marker).

```html
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Huy Nguyen — IT &amp; Automation Specialist</title>
  <meta name="description" content="Huy Nguyen — IT &amp; Automation Specialist. UW-IT service operations, web publishing, and AI/automation projects." />
  <link rel="icon" href="assets/favicon.svg" type="image/svg+xml" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <header class="nav">
    <a class="nav__brand" href="#top">HN</a>
    <nav class="nav__links">
      <a href="#experience">Experience</a>
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
      <p class="hero__title">IT &amp; Automation Specialist</p>
      <p class="hero__tagline">I keep systems running and automate the tedious parts — from UW-IT service operations to AI-driven workflows.</p>
      <div class="hero__actions">
        <a class="btn btn--primary" href="resume.pdf" download>Download Résumé</a>
        <a class="btn" href="#projects">View Projects</a>
      </div>
      <ul class="hero__links">
        <li><a href="https://github.com/hiiamhuy">GitHub</a></li>
        <li><a href="mailto:hiiamhuy@uw.edu">Email</a></li>
        <li><a href="#" data-edit="linkedin">LinkedIn</a></li>
      </ul>
    </section>

    <!-- ABOUT -->
    <section class="about" id="about">
      <h2 class="section__title">About</h2>
      <p>
        I'm a University of Washington graduate (BS Informatics, BA American Ethnic
        Studies) with roughly a decade at UW-IT spanning Tier-1 support, web
        publishing, and service operations. Today I focus on building AI and
        automation tooling — speech-to-text pipelines, local LLM workflows, and
        integrations that take repetitive work off people's plates. ITIL 4
        Foundations certified, and equally comfortable in a ticket queue or a
        terminal.
      </p>
    </section>

    <!-- EXPERIENCE -->
    <section class="experience" id="experience">
      <h2 class="section__title">Experience</h2>
      <div class="timeline">

        <article class="timeline__item">
          <div class="timeline__head">
            <h3 class="timeline__role">Senior Computer Specialist <span class="timeline__org">— UW-IT Service Center</span></h3>
            <span class="timeline__dates">2019 – Present</span>
          </div>
          <ul class="timeline__points">
            <li>Manage incidents to restore service quickly; author reference and FAQ articles for the Service Center knowledge base.</li>
            <li>Designed and programmed ServiceNow forms and efficiency shortcuts; trained students in ServiceNow, email, web hosting, and account services.</li>
          </ul>
        </article>

        <article class="timeline__item">
          <div class="timeline__head">
            <h3 class="timeline__role">Senior Computer Specialist, User Consulting Support <span class="timeline__org">— UW-IT</span></h3>
            <span class="timeline__dates" data-edit="ucs-dates">[dates]</span>
          </div>
          <ul class="timeline__points">
            <li>Managed web publishing for sites.uw.edu, Pantheon, and UW Shared Web Publishing, ensuring accessibility and usability.</li>
            <li>Provided platform support and contributed internal documentation and best practices for web publishing workflows.</li>
          </ul>
        </article>

        <article class="timeline__item">
          <div class="timeline__head">
            <h3 class="timeline__role">Identity &amp; Access Management Student Lead <span class="timeline__org">— UW-IT Service Center</span></h3>
            <span class="timeline__dates">2014 – 2018</span>
          </div>
          <ul class="timeline__points">
            <li>Approved non-standard account requests and maintained the Person Registry for all UW affiliations.</li>
            <li>Developed scripts for batch account creation; triaged hardware/software issues and trained student consultants.</li>
          </ul>
        </article>

        <article class="timeline__item">
          <div class="timeline__head">
            <h3 class="timeline__role">Student IT Consultant <span class="timeline__org">— UW-IT Service Center</span></h3>
            <span class="timeline__dates">2013 – 2015</span>
          </div>
          <ul class="timeline__points">
            <li>Provided Tier-1 support for UW affiliates; handled 50+ daily calls and emails for email, web hosting, account, and network issues.</li>
            <li>Student lead for Unix/Mailman list management and pager/teleconferencing account operations.</li>
          </ul>
        </article>

        <p class="timeline__earlier">Earlier: Web Intern, OCA-Greater Seattle (2015) · Digital Connector, Cisco (2010–2011).</p>
      </div>
    </section>

    <!-- PROJECTS -->
    <section class="projects" id="projects">
      <h2 class="section__title">Featured Projects</h2>
      <div class="projects__grid">

        <article class="card">
          <div class="card__media" aria-hidden="true"><span>Call Center QA</span></div>
          <div class="card__body">
            <h3 class="card__title">Call Center QA — Transcription &amp; Analysis</h3>
            <p class="card__desc">Automated pipeline that transcribes customer-service calls and scores quality with a local LLM. WhisperX handles speech-to-text and speaker diarization; Ollama (Llama 3.2) grades calls against a rubric and routes them to review queues — running on UW's Hyak cluster via SLURM and GPU containers.</p>
            <ul class="card__tags">
              <li>Python</li><li>WhisperX</li><li>Ollama</li><li>SLURM</li><li>Docker</li>
            </ul>
            <div class="card__links">
              <a href="https://github.com/hiiamhuy/uwitsc-call-analysis">Repo</a>
            </div>
          </div>
        </article>

        <article class="card">
          <div class="card__media" aria-hidden="true"><span>MRBS → TRMNL</span></div>
          <div class="card__body">
            <h3 class="card__title">MRBS → TRMNL E-Ink Integration</h3>
            <p class="card__desc">FastAPI service that pushes classroom booking schedules to e-ink displays. MRBS webhooks trigger real-time updates and a background scheduler keeps per-room and lobby displays current across ~30 rooms.</p>
            <ul class="card__tags">
              <li>FastAPI</li><li>SQLAlchemy</li><li>MySQL</li><li>Docker</li>
            </ul>
            <div class="card__links">
              <a href="https://github.com/hiiamhuy/Yanko-MRBS-FASTAPI">Repo</a>
            </div>
          </div>
        </article>

        <article class="card">
          <div class="card__media" aria-hidden="true"><span>Storage Analysis</span></div>
          <div class="card__body">
            <h3 class="card__title">Pantheon Storage Analysis</h3>
            <p class="card__desc">Bash tool that audits storage for Pantheon-hosted sites. Auto-detects WordPress, Drupal, or generic platforms, breaks down usage across dev/test/live environments, and exports to JSON, CSV, HTML, and Markdown.</p>
            <ul class="card__tags">
              <li>Bash</li><li>Terminus</li><li>WP-CLI</li><li>Drush</li>
            </ul>
            <div class="card__links">
              <a href="https://github.com/hiiamhuy/storage-analysis">Repo</a>
            </div>
          </div>
        </article>

      </div>
    </section>

    <!-- ALSO BUILT -->
    <section class="also" id="also">
      <h2 class="section__title">Also built</h2>
      <ul class="also__list">
        <li><strong>QuickBooks + OCR bookkeeping automation</strong> — n8n workflow that creates accounting entries from invoices and receipts, extracting vendor/date/amount via OCR and handling duplicates and exceptions.</li>
        <li><strong>OpenWeb UI call-center assistant</strong> — self-hosted, fully offline AI support tool using RAG with Ollama, deployed with Docker and Tailscale for secure access.</li>
        <li><strong>Homelab storage builds</strong> — TrueNAS (ZFS) and unRAID servers with SMB/NFS sharing, plus a 3-2-1 backup strategy using Rclone and offsite cloud storage.</li>
      </ul>
    </section>

    <!-- SKILLS -->
    <section class="skills" id="skills">
      <h2 class="section__title">Skills</h2>
      <div class="skills__groups">
        <div class="skills__group">
          <h3>Languages</h3>
          <ul><li>Python</li><li>JavaScript</li><li>HTML / CSS</li><li>SQL</li><li>Bash</li></ul>
        </div>
        <div class="skills__group">
          <h3>AI &amp; Automation</h3>
          <ul><li>WhisperX</li><li>Ollama</li><li>RAG</li><li>n8n</li><li>LM Studio</li><li>OpenWeb UI</li></ul>
        </div>
        <div class="skills__group">
          <h3>Platforms &amp; Tools</h3>
          <ul><li>Docker</li><li>Linux</li><li>Pantheon</li><li>ServiceNow</li><li>Tailscale</li><li>Git</li></ul>
        </div>
      </div>
    </section>
  </main>

  <footer class="footer" id="contact">
    <h2 class="section__title">Get in touch</h2>
    <ul class="footer__links">
      <li><a href="mailto:hiiamhuy@uw.edu">hiiamhuy@uw.edu</a></li>
      <li><a href="https://github.com/hiiamhuy">github.com/hiiamhuy</a></li>
      <li><a href="#" data-edit="linkedin">LinkedIn</a></li>
    </ul>
    <p class="footer__note">ITIL 4 Foundations certified · © 2026 Huy Nguyen. Built with plain HTML &amp; CSS.</p>
  </footer>

  <script src="main.js"></script>
</body>
</html>
```

- [ ] **Step 2: Verify it renders in a browser**

Run: `xdg-open index.html` (or open the file manually)
Expected: hero shows "IT & Automation Specialist" and the new tagline; an Experience timeline with four roles + an "earlier" line; three project cards with gradient banners (no broken images) linking to the correct repos; an "Also built" list; the three skill groups; footer shows `hiiamhuy@uw.edu` and the ITIL note. Theme toggle and nav links still work. No console errors.

- [ ] **Step 3: Grep gate — confirm no scaffold leftovers and the right email**

Run: `grep -n "Project One\|placeholder.png\|film35\|huynguyen206\|Short 2–3 sentence" index.html || echo "CLEAN"`
Expected: `CLEAN`

Run: `grep -c "hiiamhuy@uw.edu" index.html`
Expected: `2` (hero + footer)

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "content: real hero, experience, projects, skills, and uw.edu email"
```

---

### Task 3: Rewrite `resume.md` and regenerate `resume.pdf`

**Files:**
- Modify: `resume.md`
- Modify: `resume.pdf` (regenerated)

- [ ] **Step 1: Replace the entire contents of `resume.md`**

```markdown
# Huy Nguyen
IT & Automation Specialist · Seattle, WA
hiiamhuy@uw.edu · github.com/hiiamhuy

## Summary
IT and automation specialist with ~10 years at UW-IT across Tier-1 support, web
publishing, and service operations. Builds AI/automation tooling — speech-to-text
pipelines, local LLM workflows, and integrations that remove repetitive work.
ITIL 4 Foundations certified.

## Experience

**Senior Computer Specialist — UW-IT Service Center** (2019 – Present)
- Manage incidents to restore normal service operations quickly, minimizing impact.
- Author reference and FAQ articles for the Service Center knowledge base.
- Designed and programmed ServiceNow forms and efficiency shortcuts.
- Trained students in ServiceNow, email, web hosting, and account services.

**Senior Computer Specialist, User Consulting Support — UW-IT** ([dates])
- Managed web publishing for sites.uw.edu, Pantheon, and UW Shared Web Publishing.
- Provided platform support and authored internal best-practice documentation.

**Identity & Access Management Student Lead — UW-IT Service Center** (2014 – 2018)
- Approved non-standard account requests; maintained the Person Registry for all UW affiliations.
- Developed scripts for batch account creation; trained student consultants.

**Student IT Consultant — UW-IT Service Center** (2013 – 2015)
- Provided Tier-1 support; handled 50+ daily calls/emails for email, web, account, and network issues.
- Student lead for Unix/Mailman list management and pager/teleconferencing operations.

Earlier: Web Intern, OCA-Greater Seattle (2015) · Digital Connector, Cisco (2010–2011).

## Projects
- **Call Center QA — Transcription & Analysis** — WhisperX speech-to-text + speaker diarization and Ollama (Llama 3.2) rubric scoring on UW Hyak (SLURM/GPU). Python, Docker.
- **MRBS → TRMNL E-Ink Integration** — FastAPI service pushing classroom schedules to e-ink displays via webhooks + scheduler. SQLAlchemy, MySQL, Docker.
- **Pantheon Storage Analysis** — Bash tool with platform auto-detection (WordPress/Drupal) and multi-format export.
- **QuickBooks + OCR automation** — n8n workflow for receipt OCR and automated bookkeeping entries.
- **OpenWeb UI call-center assistant** — offline RAG assistant with Ollama, Docker, and Tailscale.

## Skills
Python, JavaScript, HTML/CSS, SQL, Bash · WhisperX, Ollama, RAG, n8n, LM Studio,
OpenWeb UI · Docker, Linux, Pantheon, ServiceNow, Tailscale, Git · TrueNAS, unRAID

## Education
University of Washington — BS Informatics (2018)
University of Washington — BA American Ethnic Studies (2018)

## Certification
ITIL 4 Foundations
```

- [ ] **Step 2: Regenerate `resume.pdf`**

Run: `pandoc resume.md -o resume.pdf && ls -la resume.pdf`
Expected: `resume.pdf` exists with non-zero size.

If pandoc (or its PDF engine) is unavailable, fallback: open `resume.md` in VS Code with the "Markdown PDF" extension and export to `resume.pdf`, or print-to-PDF from a browser preview. Then re-run `ls -la resume.pdf` and confirm a non-zero file.

- [ ] **Step 3: Verify the download link**

Run: reload `index.html`, click "Download Résumé".
Expected: the regenerated `resume.pdf` opens/downloads and shows the real content with `hiiamhuy@uw.edu`.

- [ ] **Step 4: Commit**

```bash
git add resume.md resume.pdf
git commit -m "content: rewrite résumé with real experience and uw.edu email"
```

---

### Task 4: Deploy to GitHub Pages

**Files:** none (remote setup)

- [ ] **Step 1: Confirm remote**

Run: `git remote -v`
Expected: either an `origin` pointing at `github.com:hiiamhuy/hiiamhuy.github.io`, or no output.

If there is NO remote, create/link it. With `gh` installed and authed as `hiiamhuy`:
`gh repo create hiiamhuy.github.io --public --source=. --remote=origin --push`
Otherwise create the repo manually on github.com (named exactly `hiiamhuy.github.io`), then:
`git remote add origin git@github.com:hiiamhuy/hiiamhuy.github.io.git`

- [ ] **Step 2: Push `main`**

Run: `git push -u origin main`
Expected: branch `main` pushed to origin.

- [ ] **Step 3: Enable Pages**

In the repo: Settings → Pages → Source = "Deploy from a branch", Branch = `main`, folder = `/ (root)` → Save. (For a `*.github.io` user repo this is often auto-enabled — verify regardless.)

- [ ] **Step 4: Verify the live site**

Wait ~1–2 min, then visit `https://hiiamhuy.github.io`.
Expected: site loads over HTTPS; all sections render; résumé downloads; the three repo links resolve to the correct repos; theme toggle works. Optionally run Chrome DevTools → Lighthouse and aim for 90+ on Accessibility and Best Practices.

- [ ] **Step 5: Final commit (only if post-deploy fixes were needed)**

```bash
git add -A
git commit -m "fix: post-deploy polish"
git push
```

---

## Self-Review Notes

- **Spec coverage:** positioning/hero (T2) ✓; About (T2) ✓; Experience timeline incl. placeholder date (T1 CSS, T2 markup) ✓; 3 featured projects with real repo URLs (T2) ✓; "Also built" list (T1 CSS, T2 markup) ✓; skills three groups (T2) ✓; footer/contact with `hiiamhuy@uw.edu` + ITIL note (T2) ✓; email change everywhere incl. résumé (T2, T3) ✓; drop `film35` (T2 grep gate) ✓; résumé md→pdf rewrite (T3) ✓; deploy (T4) ✓.
- **Placeholders:** Two intentional, spec-listed placeholders remain marked with `data-edit`: LinkedIn URL (`data-edit="linkedin"`) and the User Consulting Support dates (`data-edit="ucs-dates"` / `[dates]`). These are explicitly non-blocking open items in the spec, not plan gaps. No "TBD"/"implement later" steps.
- **Consistency:** class names match across CSS (T1) and HTML (T2): `timeline`, `timeline__item`, `timeline__head`, `timeline__role`, `timeline__org`, `timeline__dates`, `timeline__points`, `timeline__earlier`, `card__media`/`span`, `also__list`. Existing classes (`card`, `card__body`, `section__title`, `data-theme`, `theme-toggle`) reused unchanged.
- **Banner vs. image:** cards intentionally use a gradient `.card__media` banner instead of `<img src="placeholder.png">`, eliminating broken images; real screenshots can replace the banner later (noted in spec open items).
```