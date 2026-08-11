# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status

This repository has no build tooling, tests, or application source — it's a workspace for a single Claude Code skill, `find-restaurant`, plus its generated output.

## Structure

- `.claude/skills/find-restaurant/SKILL.md` — the skill definition: gathers requirements (cuisine, neighborhood, budget, party size, date/time, dietary constraints, ambiance, and a response language — Korean, Japanese, Simplified Chinese, or Hindi), searches live web sources for candidates, and presents ranked recommendations.
- `.claude/skills/find-restaurant/template.html` — HTML template used to render recommendations as a bilingual (French + user's chosen language) standalone page.
- `restaurants-*.html` — generated output files from running the skill; not meant to be hand-edited, safe to delete/regenerate.

## Working on the skill

- Restaurant candidates and info must come only from these three sources: leguideparisien.com, timeout.fr, and sortiraparis.com. Don't add other sources without updating SKILL.md.
- When editing template.html, keep the two-column (French / chosen language) layout and the `{{PLACEHOLDER}}` naming convention SKILL.md relies on when filling it in.
- If real application code, build tooling, or tests are added later, re-run `/init` (or ask Claude to update this file) to document build/lint/test commands and architecture.
