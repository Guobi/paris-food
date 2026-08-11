# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status

This repository has no build tooling, tests, or application source — it's a workspace for a single Claude Code plugin, `find-paris-restaurant`, plus its generated output. The repo doubles as that plugin's marketplace so it can be installed via `/plugin`.

## Structure

- `.claude-plugin/marketplace.json` — marketplace manifest listing the `find-paris-restaurant` plugin, so users can `/plugin marketplace add` this repo.
- `find-paris-restaurant-plugin/.claude-plugin/plugin.json` — the plugin manifest (name, description, version).
- `find-paris-restaurant-plugin/skills/find-paris-restaurant/SKILL.md` — the skill definition: gathers requirements (cuisine, neighborhood, budget, party size, date/time, dietary constraints, ambiance, and a response language — Korean, Japanese, Simplified Chinese, or Hindi), searches live web sources for candidates, and presents ranked recommendations.
- `find-paris-restaurant-plugin/skills/find-paris-restaurant/template.html` — HTML template used to render recommendations as a bilingual (French + user's chosen language) standalone page.
- `restaurants-*.html` — generated output files from running the skill; not meant to be hand-edited, safe to delete/regenerate.

## Working on the skill

- Restaurant candidates and info must come only from these three sources: leguideparisien.com, timeout.fr, and sortiraparis.com. Don't add other sources without updating SKILL.md.
- When editing template.html, keep the two-column (French / chosen language) layout and the `{{PLACEHOLDER}}` naming convention SKILL.md relies on when filling it in.
- Bump `version` in `find-paris-restaurant-plugin/.claude-plugin/plugin.json` when shipping a change, so users get it via `/plugin marketplace update`.
- If real application code, build tooling, or tests are added later, re-run `/init` (or ask Claude to update this file) to document build/lint/test commands and architecture.
