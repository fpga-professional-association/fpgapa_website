# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project overview

Static website for the FPGA Professional Association — a volunteer-run organization for FPGA engineers. Hosted on GitHub Pages directly from `main`, served as plain HTML and CSS with no build step.

The full implementation plan is in `BUILD_PLAN.md` (15 sections). Read it before any non-trivial change.

## Hard rules

These are gates, not preferences. PRs that violate them will not merge.

- **No build step.** What is in the repo is what is served. No webpack, vite, npm scripts. No `node_modules` committed.
- **No JavaScript frameworks.** No React, Vue, etc. Vanilla JS only if absolutely required, inline, under 1 KB, and justified in the PR.
- **No CSS frameworks.** No Tailwind, Bootstrap. One `styles.css` at the repo root.
- **No third-party fonts or CDNs.** No Google Fonts, no CDN scripts. System fonts only.
- **No tracking or analytics** that requires cookie consent.
- **Site must work with JavaScript disabled.**

## Performance gates

Per `BUILD_PLAN.md` §10:

- Page weight under 50 KB total per page (HTML + CSS + favicon).
- Lighthouse Performance / Accessibility / Best Practices: 100. SEO: 95+.
- 0 bytes of JavaScript by default.
- WCAG 2.1 AA contrast on every text/background combination.

## File structure

```
fpgapa_website/
├── CLAUDE.md
├── BUILD_PLAN.md           # Full plan
├── README.md
├── LICENSE                 # MIT — applies to code
├── LICENSE-CONTENT         # CC BY 4.0 — applies to written content
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md
│   ├── ISSUE_TEMPLATE/*.md
│   └── workflows/html-validate.yml
├── index.html              # Home
├── about.html
├── resources.html
├── tools.html
├── contribute.html
├── 404.html
├── styles.css              # Single style file. Custom properties for theming.
└── assets/
    ├── favicon.svg
    └── logo.svg
```

Target: under 25 files. Adding more requires justification in the PR.

## Local preview

No build step. To preview:

```bash
# Open in a browser
open index.html

# Or serve over http (recommended; some browser checks need http://)
python3 -m http.server 8080
# then visit http://localhost:8080
```

## CI

`.github/workflows/html-validate.yml` runs on every PR + push to `main`:

- HTML validation (`html-validate`)
- Internal link check (`linkinator`)
- Blocks inline `<script>` tags
- Blocks external CDN / font references

## Editorial rules

Per `BUILD_PLAN.md` §11:

- Declarative voice. No "we believe" / "we think" hedges.
- No exclamation points.
- No rhetorical questions in headers.
- Cite numbers. Numbers without citations get removed.
- No buzzwords (transform, leverage, synergy, best-in-class, cutting-edge, next-generation, revolutionary).
- Active voice over passive.
- Acronyms spelled out on first use (FPGA, HDL, RTL, UVM).
- Vendor names match official capitalization (AMD/Xilinx Vivado, Intel/Altera Quartus Prime, Lattice Radiant/Diamond, Microchip Libero SoC).

## Design system

Per `BUILD_PLAN.md` §6:

- Seven colors total, defined as CSS custom properties on `:root`.
- System fonts only.
- Body 17px / 1.6 line-height.
- Spacing scale: 4 / 8 / 16 / 24 / 32 / 48 / 64.
- Max width: 720px for prose, 960px for landing.
- Components: header, footer, headings, lists, code, link, button, table, definition list. **No cards, carousels, accordions, tabs, modals, or tooltips.**

## Approved page set (v1)

Five pages only. Adding pages requires Travis's approval.

1. `index.html` — Home
2. `about.html` — About
3. `resources.html` — Resources
4. `tools.html` — Tools
5. `contribute.html` — Contribute

Plus `404.html` (custom 404).

## PR workflow

All changes flow through pull requests.

1. Branch from `main`.
2. Edit HTML / CSS / Markdown.
3. Open PR. CI runs automatically.
4. Travis reviews. He is the sole required approver.
5. On approval, merge (squash or rebase). Site auto-deploys.

Branch protection on `main` requires: 1 approving review, status checks pass, linear history. No force pushes.

## Out of scope (defer; do not anticipate)

Per `BUILD_PLAN.md` §15:

- Member directory, job board, event calendar.
- Blog / news section (will land via separate Jekyll PR).
- Search, translations, RSS feed.
- Discord / Slack widgets.
- Donation pages.
- Interactive salary explorer.

If a requested feature touches one of these, refer to the full plan and ask whether it is in scope before implementing.
