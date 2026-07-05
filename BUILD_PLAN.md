# FPGA Professional Association — Website Build Plan

**Audience:** Claude Code (implementation agent)
**Owner:** Site maintainer (sole approver of all PRs)
**Repository:** `https://github.com/fpga-professional-association/fpgapa_website`
**Hosting:** GitHub Pages (free, served directly from `main` branch)
**License:** MIT (code) / CC BY 4.0 (content)

---

## 1. Project Goals

Build a public, open-source website for the FPGA Professional Association that:

1. **Looks professional and respects engineers' intelligence.** No marketing fluff, no stock photos of people in suits shaking hands, no "Transform Your Career" hero copy.
2. **Loads instantly.** Pure static HTML/CSS. No JavaScript frameworks. No build step. No `node_modules`.
3. **Is trivially contributable.** Any engineer who knows basic HTML/CSS should be able to fork, edit, and submit a PR in under 10 minutes. No tooling barriers.
4. **Is hosted entirely on GitHub.** GitHub Pages from the `main` branch. Optional custom domain configured via DNS later.
5. **Enforces single-approver workflow.** All changes flow through pull requests. Branch protection requires the maintainer's approval before merge.

### Non-Goals (explicitly out of scope for v1)

- Login / member portal / authentication
- Payment processing or membership signup forms
- Database-backed content
- JavaScript SPA frameworks (React, Vue, etc.)
- CSS frameworks (Tailwind, Bootstrap)
- Build pipelines (Webpack, Vite, npm)
- Analytics tracking that requires consent banners
- Newsletter widgets or popups

If a feature requires a backend or build step, it does not belong in v1. The signup flow lives on a separate landing page tool; this site links to it.

---

## 2. Design Philosophy

The visual reference points are not marketing sites. They are:

- `go.dev` — clean, content-first, engineer-respecting
- `rust-lang.org` — technical credibility, restrained color
- `kernel.org` — utterly functional, zero pretense
- `news.ycombinator.com` — proves that "ugly but fast" beats "pretty but slow" for technical audiences
- `tailscale.com/kb` — modern but understated typography
- `sive.rs` — content over chrome

The site should feel like documentation, not a product landing page.

### Core design rules

1. **Content first.** Visual elements support content; they do not replace it.
2. **Generous whitespace, no decoration.** No gradients, no shadow effects on buttons, no animated hero illustrations, no parallax scroll.
3. **One accent color.** Used sparingly for links and key actions only.
4. **System fonts or one carefully chosen pairing.** No web font stacks that block rendering.
5. **Readable line lengths.** Body text capped at ~70ch.
6. **Mobile works, but desktop is the priority.** FPGA engineers read this on a workstation.
7. **No dark patterns.** No popups, no exit-intent modals, no "subscribe to continue."

---

## 3. Tech Stack

| Layer            | Choice                                                                 | Why                                                                |
|------------------|------------------------------------------------------------------------|--------------------------------------------------------------------|
| Markup           | Hand-written HTML5                                                     | Anyone can read and edit it. No abstraction.                       |
| Styling          | Single `styles.css` file. CSS custom properties for theming.           | No preprocessor. No framework. Diffs are readable in PRs.          |
| Interactivity    | Vanilla JS only if absolutely required (e.g., mobile menu toggle)      | No framework dependency. Site must work with JS disabled.          |
| Hosting          | GitHub Pages, served from `main` branch, `/` root                      | Free. Zero ops. Built-in HTTPS.                                    |
| Build            | None                                                                   | What is in the repo is what is served.                             |
| Domain           | `<org>.github.io` initially; custom domain via `CNAME` file later      | Defer DNS until launch is validated.                               |
| Forms            | None on-site. CTA buttons link to external signup page.                | No backend required.                                               |

### Why not Jekyll / Hugo / Astro?

Jekyll is GitHub Pages' default and tempting for a blog. Defer it. v1 has no blog. When a news/blog section is added later, Jekyll can be introduced in a separate PR — its file structure is compatible with this plan. Do not pre-build for content that does not exist yet.

---

## 4. Repository Structure

```
fpgapa_website/
├── README.md
├── LICENSE
├── LICENSE-CONTENT
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── CNAME                      # Empty for now; populated when custom domain set
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md
│   ├── ISSUE_TEMPLATE/
│   │   ├── content_correction.md
│   │   ├── design_suggestion.md
│   │   └── new_resource.md
│   └── workflows/
│       └── html-validate.yml
├── index.html
├── about.html
├── resources.html
├── tools.html
├── contribute.html
├── contact.html
├── 404.html
├── styles.css
├── assets/
│   ├── favicon.svg
│   ├── favicon-32.png
│   ├── og-image.png
│   └── logo.svg
└── docs/
    └── (initially empty)
```

File count target for v1: under 25 files.

---

## 5. Site Map (v1)

Five pages. No more.

1. **`/` (index.html)** — What FPGA PA is, who it's for, what's available now.
2. **`/about.html`** — Mission, operating model (volunteer-driven), how decisions get made.
3. **`/resources.html`** — Index of free resources.
4. **`/tools.html`** — CovertEDA introduction and link to its repo. Placeholder for future tools.
5. **`/contribute.html`** — How to get involved.

Header navigation appears on every page. Same five links, same order, no dropdown menus.

Since v1: `ai-augmented-se.html`, `curriculum-fit.html`, and `blog.html` (plus posts under `blog/`) shipped as maintainer-approved additions. Header navigation now also links to Blog (see §15 on the blog decision).

---

## 6. Design System

### 6.1 Color palette

Define as CSS custom properties on `:root`. Total of seven values. No more.

```css
:root {
  --color-bg: #fdfdfc;
  --color-fg: #1a1a1a;
  --color-fg-muted: #555555;
  --color-border: #e5e5e3;
  --color-accent: #1f4e8c;
  --color-accent-hover: #163a6a;
  --color-code-bg: #f4f4f1;
}
```

Dark mode is optional for v1. If included, use `prefers-color-scheme: dark` only. No toggle UI in v1.

### 6.2 Typography

System font stack for body. No Google Fonts. No `@font-face`. No CDN.

```css
:root {
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
               "Helvetica Neue", Arial, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, "SF Mono", Menlo,
               Consolas, "Liberation Mono", monospace;
  --font-serif: Charter, "Bitstream Charter", "Sitka Text", Cambria, serif;
}
```

- Body: `--font-sans`, 17px base, 1.6 line-height.
- Headings: `--font-sans`, weight 600, line-height 1.25.
- Code, technical identifiers, CLI commands, file paths: `--font-mono`.

Type scale (modular, ratio 1.2):

| Element | Size  |
|---------|-------|
| h1      | 2.0rem |
| h2      | 1.5rem |
| h3      | 1.25rem |
| body    | 1.0rem |
| small   | 0.875rem |

### 6.3 Layout

- Max content width: 720px for prose; 960px for landing.
- Centered single column. No sidebars in v1.
- Spacing scale: 4 / 8 / 16 / 24 / 32 / 48 / 64 px. Use only these values.
- Header: plain `<header>` with logo wordmark left, nav links right. Border-bottom, no shadow.
- Footer: simple, three lines. License, GitHub repo link, contact email.

### 6.4 Components

Keep this list short. Anything not in it requires maintainer approval.

- Header / nav
- Footer
- Heading + body section
- Bulleted / numbered lists
- Inline code (`<code>`)
- Code blocks (`<pre><code>`)
- Link
- Button (styled link, used for primary CTAs)
- Table
- Definition list

**No cards. No carousels. No accordions. No tabs. No modals. No tooltips.**

---

## 7. Page Content Specifications

Tone: declarative, factual, no exclamation points, no rhetorical questions in headers.

### 7.1 `index.html` — Home

**Title:** `FPGA Professional Association`
**Meta description:** A volunteer-driven professional association for FPGA engineers. Career resources, open-source tools, and community.

**Hero** (text only, no image):
- H1: "FPGA Professional Association"
- Subtitle: "A volunteer-run organization for FPGA engineers — career resources, open-source tools, and a community that takes the profession seriously."
- Two buttons: "Get involved" → `/contribute.html` and "See our tools" → `/tools.html`.

**The problem we're addressing** (~120 words):
Brief, factual statement of the FPGA workforce gap, the GUI productivity problem, and the lack of vendor-neutral career resources. Cite specific numbers (67,000 projected unfilled positions by 2030; 87% project failure rate from verification issues). No drama.

**What's available now** (3 short blocks, no icons):
- *Open-source tooling.* CovertEDA — replacement GUI for Vivado, Quartus, Radiant, and Libero.
- *Free resources.* Salary data, interview preparation, security references.
- *Community.* Volunteer-led, contribution-driven, no paywalled gatekeeping.

**How we operate** (~80 words):
Volunteer-driven. Decisions made transparently. Open source. No corporate ownership. No venture funding. No upselling.

### 7.2 `about.html` — About

1. **What we do** — One paragraph mission statement.
2. **Who this is for** — FPGA engineers at all career stages.
3. **How we operate** — Volunteer model. Transparent decision-making. Open-source by default.
4. **What we are not** — Not a certification body, not a vendor partner, not a recruiting agency, not a lobbying organization, not a paywalled site.
5. **Origin** — Brief.

Total length: ~600 words. No bios in v1.

### 7.3 `resources.html` — Resources

Plain index of links. Each entry: title, one-line description, link. Group by category: Career, Technical references, Tooling, Community.

### 7.4 `tools.html` — Tools

Lead section: CovertEDA. Three short paragraphs (vendor GUI problem, what CovertEDA does, status + repo link). Future tools placeholder.

### 7.5 `contribute.html` — Contribute

The most important page after the home page.

1. Why contribute.
2. Ways to contribute (improve this site, contribute to CovertEDA, write a resource, volunteer for a working group, start a regional chapter).
3. PR workflow for this site.
4. What we won't merge.

State: the association is volunteer-run; no contributor is paid; all contributions are credited.

---

## 8. GitHub Configuration

### 8.1 Repository settings

- Visibility: Public.
- Default branch: `main`.
- Branch protection on `main`:
  - Require pull request before merging.
  - Require 1 approving review (the maintainer).
  - Dismiss stale approvals on new commits.
  - Require status checks to pass: HTML validation workflow.
  - Require linear history (squash or rebase merges only).
  - Restrict who can push to matching branches: admins only.
- Issues: Enabled.
- Discussions: Enabled (light moderation).
- Wiki: Disabled.
- Projects: Enabled if useful for tracking; not required.

### 8.2 GitHub Pages

- Source: Deploy from a branch.
- Branch: `main` / `/` (root).
- Custom domain: Leave blank initially.
- Enforce HTTPS: Yes.

### 8.3 CI workflow

`.github/workflows/html-validate.yml` runs on every PR:

- Validates all `.html` files using `html-validate` (npx, no committed `node_modules`).
- Checks for broken internal links.
- Fails on inline JavaScript.
- Fails on external font / CDN references.

Workflow file under 50 lines.

### 8.4 Templates

PR template includes a checklist (no tracking, no external dependencies, factual / cited content). Three issue templates (content correction, design suggestion, new resource).

---

## 9. Accessibility Requirements

Non-negotiable.

- WCAG 2.1 AA contrast on every text/background combination.
- Fully keyboard-navigable (tab order matches visual order; visible focus indicators).
- Semantic HTML (`<nav>`, `<main>`, `<article>`, `<header>`, `<footer>`).
- `alt` text on every image (or `alt=""` if purely decorative).
- `lang="en"` on `<html>`.
- Viewport meta set correctly for mobile.
- Working "skip to main content" link.

---

## 10. Performance Targets

These are gates.

- Page weight: under 50 KB total per page (HTML + CSS + favicon).
- Largest Contentful Paint: under 0.5s on broadband.
- JavaScript size: 0 bytes by default.
- Lighthouse Performance: 100. Accessibility: 100. Best Practices: 100. SEO: 95+.

If a change drops any score below target, the PR does not merge.

---

## 11. Content Guidelines

Enforced in review.

1. Write declaratively.
2. No exclamation points.
3. No rhetorical questions in headers.
4. Cite numbers.
5. No buzzwords (transform, leverage, synergy, best-in-class, industry-leading, cutting-edge, next-generation, revolutionary).
6. Short sentences over long.
7. Active voice over passive.
8. Acronyms spelled out on first use (FPGA, HDL, RTL, UVM).
9. Vendor names match official capitalization (AMD/Xilinx Vivado, Intel/Altera Quartus Prime, Lattice Radiant/Diamond, Microchip Libero SoC).
10. Link directly to sources.

---

## 12. Implementation Phases

### Phase 1 — Skeleton

- Create repository.
- Add `LICENSE`, `LICENSE-CONTENT`, `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`.
- Add `.github/` templates and workflow.
- Configure branch protection.
- Push placeholder `index.html`.
- Enable GitHub Pages.

### Phase 2 — Design system

- Build `styles.css` with full design system from §6.
- Render style guide showing every component.

### Phase 3 — Pages

- Build all five pages per §7 specifications.

### Phase 4 — Polish

- Custom 404 page.
- `og-image.png` (text-only).
- `favicon.svg`.
- README updated with live URL.

### Phase 5 — Launch

- Announce on LinkedIn, r/FPGA, mailing list.
- Open repository for community PRs.

---

## 13. Acceptance Criteria

- All five pages render correctly in current Chrome, Firefox, Safari.
- Site is fully usable with JavaScript disabled.
- Total page weight under 50 KB on every page.
- Lighthouse scores meet §10 targets.
- All links resolve.
- HTML validates.
- Accessibility lint passes.
- Branch protection on `main` is active and the maintainer is the sole required reviewer.
- `README.md` documents how to clone, edit, and contribute.
- `CONTRIBUTING.md` includes the content guidelines from §11.
- No third-party fonts, analytics, or CDN assets anywhere.
- PR template is in place.
- CI workflow runs and blocks merges that fail validation.

---

## 14. Open Questions for the Maintainer

1. **Domain.** Is a custom domain registered? If so, what is it?
2. **Logo / wordmark.** Is there an existing wordmark? If not, the placeholder is a simple monospace `FPGA PA` until one is designed.
3. **Email contact.** What address goes in the footer and on the contact page?
4. **License confirmation.** MIT for code and CC BY 4.0 for content acceptable?
5. **Salary survey publication target.** When will the survey be ready to link from the resources page?
6. **Newsletter / signup.** What is the canonical signup URL the home-page CTA should point to?

---

## 15. Out of Scope, Save for Later

These are real ideas that should not be built now.

- Member directory
- Job board
- Event calendar
- Search
- Translations
- RSS feed
- Discord/Slack widget
- Donation / sponsorship page
- Interactive salary explorer
- Member spotlight pages

Each of these earns its own plan document when its time comes. Do not anticipate them in the v1 architecture.

**Blog, resolved:** this plan originally deferred the blog to a future Jekyll migration. The maintainer approved shipping it earlier instead, as static HTML pages under `blog.html` / `blog/`, with no build step, consistent with the rest of the site. A Jekyll migration remains possible later if post volume outgrows hand-written HTML.

---

*End of plan.*
