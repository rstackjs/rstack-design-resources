# AGENTS.md

## Repository nature

This is an **assets-only repository** — brand and marketing visuals for the Rstack family (Rspack, Rsbuild, Rspress, Rsdoctor, Rslib, Rslint, Rstest). There is no source code, no `package.json`, no build / test / lint pipeline. Contributions are file additions, replacements, and renames.

`cspell.json` is the only tooling config and exists solely to allowlist project names for spellcheckers run elsewhere.

## Deploy model

The repo is served verbatim via Cloudflare at `https://assets.rspack.rs/`. **The file path in the repo IS the URL path** — `rspack/rspack-logo.png` becomes `https://assets.rspack.rs/rspack/rspack-logo.png`. No build step rewrites paths, so renames and deletions are immediately user-visible (subject to the CDN's `max-age=14400` / 4h cache TTL).

Implication: treat path changes as breaking. Prefer additive PRs over renames once a URL has been shared.

## Layout convention

Per-package top-level directories (`rspack/`, `rsbuild/`, `rspress/`, `rsdoctor/`, `rslib/`, `rslint/`, `rstest/`, plus `rstack/` for cross-family visuals and `web-infra/` for the parent org). Inside each:

- **Root of the package dir** — primary, externally-linked deliverables (logo, banner, OG image, favicon, navbar logo).
- **`<pkg>/assets/`** — ad-hoc / temporary media tied to a specific moment: blog post images, release-note illustrations, docs screenshots, profiling captures, animated demos (`.gif`, `.mp4`). Not a stable surface — files here may be added freely without the same naming rigor as the package root.
- **`others/`** — source design files (`.sketch`) used to author multiple deliverables.

## Naming conventions (load-bearing)

- `<pkg>-logo[-WxH].{svg,png,ai,sketch,fig}` — primary brand mark; size suffix optional (`rsbuild-logo-512x512.png`).
- `<pkg>-banner-v<MAJOR>-<MINOR>[-<PATCH>][-<PRE>].png` — **release announcement banners**. Use kebab-case for version dots: `v0-3`, `v1-0-alpha`, `v0-10-0`. (A few historical files use `vX.Y` with a literal dot — do not propagate that style.)
- `<pkg>-og-image.png` — current OpenGraph card at the package root.
- `navbar-logo-{dark,light}.png` — in-app navigation usage.
- `favicon-<WxH>.png` — site favicons.

When in doubt, grep the sibling packages (`rspack/`, `rsbuild/`) for precedent before inventing a new name — they have the most history.

## Mandatory: TinyPNG before merge

**Every added or updated PNG must be compressed via [TinyPNG](https://tinypng.com/) before the PR is merged.** This is enforced by the PR template checklist (`.github/PULL_REQUEST_TEMPLATE.md`) and is the single hard rule of the repo. Replacement-with-compressed-version PRs are routine and expected if a contributor forgets.

## Workflow notes

- `main` is the default branch; PRs go against `rstackjs/rstack-design-resources`.
- Commit message convention from recent history: `chore(<pkg>): <imperative>` (e.g. `chore(rstest): add v0.10.0 banner`). Use `feat(<pkg>):` only for genuinely new asset families.
- The repo has no CI checks beyond GitHub's defaults — review is human and visual.

