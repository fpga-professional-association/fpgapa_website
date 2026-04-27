# Contributing

Thanks for considering a contribution. This project is volunteer-run and contributions of any size are welcome — typo fixes, copy edits, new resources, design improvements.

## Quick start

1. Fork this repository.
2. Clone your fork locally.
3. Create a branch:
   ```
   git checkout -b your-change
   ```
4. Edit files. Preview locally by opening `index.html` in your browser.
5. Commit, push to your fork, and open a pull request against `main`.

CI validates HTML and checks internal links automatically. The maintainer reviews all PRs and is the sole required approver. Most reviews land within seven days.

## What we will not merge

- Marketing language: *transform*, *leverage*, *best-in-class*, *industry-leading*, *cutting-edge*, *next-generation*, *revolutionary*.
- Vendor-promotional content that breaks vendor neutrality.
- Tracking, analytics, or third-party scripts that would require cookie consent.
- JavaScript frameworks (React, Vue, etc.) or CSS frameworks (Tailwind, Bootstrap).
- External font / CDN references (Google Fonts, jsDelivr, unpkg, etc.).
- Numeric claims without a citation.
- Decorative images that do not add information.
- New top-level pages without prior discussion.

## Editorial style

- **Declarative voice.** State what is true. Avoid "we believe" / "we think" hedges.
- **No exclamation points.** The information is enough.
- **No rhetorical questions in headers.** Use a statement.
- **Cite numbers.** Numbers without citations get removed in review.
- **Short sentences over long.** A period is cheaper than a comma.
- **Active voice over passive.**
- **Acronyms spelled out on first use** (FPGA, HDL, RTL, UVM).
- **Vendor names match official capitalization:** AMD/Xilinx Vivado, Intel/Altera Quartus Prime, Lattice Radiant/Diamond, Microchip Libero SoC.
- **Link directly to sources.** Do not paraphrase a primary source when the primary source can be cited.

## Tech rules

These are not preferences; they are gates.

- No build step. No `node_modules` committed.
- One CSS file: `styles.css` at the repo root.
- No JavaScript frameworks. Site must work with JavaScript disabled.
- Total page weight under 50 KB.
- WCAG 2.1 AA accessibility.
- Lighthouse 100 / 100 / 100 / 95+ on every page.

`CLAUDE.md` and `BUILD_PLAN.md` document the architecture and design system in detail.

## Code of Conduct

By contributing, you agree to follow `CODE_OF_CONDUCT.md`.
