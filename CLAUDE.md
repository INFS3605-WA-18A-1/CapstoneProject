# CLAUDE.md

Guidance for AI agents working in this repository.

## Project

INFS3605 (UNSW) capstone project, team W18A-1: designing an AI solution for SMB founders so decision making can be distributed within the organisation. This repo currently holds the team's **documentation site** (MkDocs + Material theme), published to GitHub Pages at <https://infs3605-wa-18a-1.github.io/CapstoneProject/>.

Team members (each has a contribution log): Kevin, John, Ho Nam, Lena, Sophia.

## Layout

```
.github/workflows/
  docs.yml          # push to main -> mkdocs gh-deploy (publishes gh-pages branch)
  docs-test.yml     # push to dev / PRs to main or dev -> mkdocs build --strict
docs-site/          # MkDocs project root — run mkdocs commands from here
  mkdocs.yml        # site config, theme, nav tree
  overrides/        # theme custom_dir (Jinja2 template overrides)
    home.html       # landing-page template (hero section), used by docs/index.md
  docs/             # Markdown source; every page must be listed in nav
    index.md        # front matter `template: home.html`; its body is NOT rendered
    stylesheets/extra.css   # custom CSS (landing-page .mdx-* hero styles)
    contribution-logs/<member>.md
requirements.txt    # pinned Python deps (repo root)
```

## Commands

Run from `docs-site/` with the virtualenv at the repo root:

```bash
python -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt   # from repo root, first time
cd docs-site
mkdocs serve           # live preview at http://127.0.0.1:8000
mkdocs build --strict  # what CI runs — must pass before pushing
```

`docs-site/site/` is build output and is gitignored. Never commit it and never push to `gh-pages` by hand; CI owns that branch.

## Conventions

- **Adding a page:** create the `.md` under `docs-site/docs/` AND add it to `nav:` in `mkdocs.yml`. `--strict` fails on broken links and missing nav targets.
- Top-level `nav` entries render as tabs (`navigation.tabs`). Keep the top level short.
- File names: lowercase kebab-case (`market-research-report.md`). Each page starts with a single `# Title` H1.
- Enabled Markdown extensions: `admonition`, `pymdownx.details`, `pymdownx.superfences`, `toc` (permalinks). Adding extensions like `attr_list`, `md_in_html`, `pymdownx.emoji` is fine if a page needs them — they ship with the pinned packages.
- **Styling:** put site-wide CSS in `docs/stylesheets/extra.css` and use Material's CSS variables (`--md-primary-fg-color`, etc.) so light/dark (`default`/`slate`) schemes both work. Landing-page-only rules that hide the sidebars live inline in `overrides/home.html`.
- **Theme overrides:** extend Material templates with `{% extends "main.html" %}` and override blocks; don't copy Material's templates wholesale.
- The home page's text comes from `site_name` / `site_description` in `mkdocs.yml`; "Get started" links to the next page in `nav`.

## Dependencies

- Versions are pinned in `requirements.txt` (mkdocs 1.6.1, mkdocs-material 9.7.7). CI uses Python 3.12.
- **Do not upgrade to MkDocs 2.0.** It removes the plugin/theme-override system Material depends on (Material prints a warning about this on every build — that warning is expected and harmless).

## Git workflow

- `main` is the published branch (every push deploys). `dev` is the working branch; PRs into `main`/`dev` run the strict build check.
- Run `mkdocs build --strict` locally before committing doc changes.
