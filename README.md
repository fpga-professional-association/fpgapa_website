# FPGA Professional Association — Website

Static website for the FPGA Professional Association. Plain HTML and CSS, no build step.

## Live site

GitHub Pages deployment pending. Will be linked here once configured.

## Local preview

This site has no build step. To preview:

```bash
# Open directly in a browser
open index.html

# Or serve over http (recommended; some browser checks need http://)
python3 -m http.server 8080
# then visit http://localhost:8080
```

## Structure

See `CLAUDE.md` for the file layout and hard rules. See `BUILD_PLAN.md` for the full implementation plan.

## Contributing

Read `CONTRIBUTING.md` first. Briefly:

1. Fork this repository.
2. Create a branch.
3. Edit HTML / CSS / Markdown.
4. Open a PR with a clear description.
5. CI runs HTML validation and link checking.
6. The maintainer reviews and is the sole required approver.

## License

- Code: MIT — see `LICENSE`.
- Content (text, images, documentation): CC BY 4.0 — see `LICENSE-CONTENT`.

## Code of Conduct

See `CODE_OF_CONDUCT.md`.
