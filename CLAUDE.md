# FouFou Landing Page — Claude Context

Single-file bilingual (Hebrew/English) marketing landing page for the FouFou app. Separate repo and separate concern from the app itself — the freeze/bug-fix-only rules that apply to [FouFou](../../FouFou/CLAUDE.md) (prod) do **not** apply here.

**Full handoff detail lives in `LANDING_HANDOFF.md`** in this same directory — read it before any substantial change (brand assets, screenshot pipeline, deployment steps, verification checklist). This file only holds the facts most likely needed at a glance.

- **Target domain:** fofou.city
- **Links to:** https://eitanfisher2026.github.io/FouFou/ (the live prod app)
- **Owner:** Eitan Fisher
- Sibling of [FouFou-dev](../../FouFou-dev/CLAUDE.md) / [FouFou](../../FouFou/CLAUDE.md) / [foufou-build](../../foufou-build/CLAUDE.md) — same person's project, but this repo has no Firebase/backend of its own; it's a static page.

## Stack
Single `index.html` — no framework, no build step. Plain HTML/CSS/JS.

## i18n — how language switching works
- Priority: `?lang=he`/`?lang=en` URL param → `localStorage['foufou.lang']` → browser language → default Hebrew.
- Every piece of copy is duplicated with `data-lang-he` / `data-lang-en` attributes; CSS shows the one matching `<html lang="...">`.
- Default language is Hebrew, RTL.

## Design tokens (CSS variables at the top of `<style>`)
```
--peach-1: #F5946B   --peach-2: #F8B287   --peach-3: #FBD9B4   (hero gradient, must match the app's splash screen)
--coral: #E76F51     --coral-deep: #D14B30   --saffron: #F4A261
--turquoise: #2A9D8F --plum: #6B2D5C
--cream: #FFF4E6 (page bg)  --paper: #FFFBF2 (cards/nav)  --ink: #2A1F1A  --ink-soft: #5C4A41
```
Neo-brutalist style: hard 4px-offset shadows (no blur), 2px solid `--ink` borders everywhere.

## Known open TODOs (see `LANDING_HANDOFF.md` for full checklist)
- Cat logo is a hand-rebuilt SVG approximation, not the real logo asset — replace if the original is found in the FouFou app repo.
- No real favicon/apple-touch-icon yet, no OG image for social sharing, no analytics installed.

## Standing workflow rules (shared across Eitan's projects)
- **Decide and act, don't ask process questions** — carry an agreed change through without pausing for permission at each step, unless genuinely irreversible.
- Communicate in product-level terms, not code details — see the global `~/.claude/CLAUDE.md`.
