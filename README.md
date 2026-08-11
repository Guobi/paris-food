# find-paris-restaurant

A Claude Code skill that finds and recommends restaurants in Paris based on a person's requirements, using live web search rather than a fixed dataset.

## What it does

1. **Gathers requirements** — cuisine, neighborhood, budget, party size/occasion, date/time, dietary constraints, ambiance, and a response language (Korean, Japanese, Simplified Chinese, or Hindi). Only asks about what's missing and matters.
2. **Searches live sources** — restricted to three sites for candidates and info:
   - [leguideparisien.com](https://leguideparisien.com/restaurants/)
   - [timeout.fr](https://www.timeout.fr/paris/restaurant/50-meilleurs-restaurants)
   - [sortiraparis.com](https://www.sortiraparis.com/)
3. **Presents 2-4 ranked recommendations** in the chat, in French and the user's chosen language, each with address, hours, a photo (if found), price range, why it fits, and any time-sensitive notes (reservations, closed days).
4. **Saves a bilingual HTML file** (`restaurants-<slug>.html`) rendered from [template.html](find-paris-restaurant-plugin/skills/find-paris-restaurant/template.html), with a two-column French / chosen-language layout.

## Usage

Just ask Claude Code for a restaurant recommendation in Paris, e.g.:

> "Find me a cheap ramen spot near the Marais open on a Sunday"

> "Romantic bistro for two near Saint-Germain-des-Prés, mid-range budget, want it in Japanese"

> "Vegan-friendly restaurant near the Louvre for a business lunch on a weekday"

Claude will ask any missing clarifying questions, search, and reply with recommendations plus a saved HTML file.

## Installation

This repo is itself a Claude Code plugin marketplace ([.claude-plugin/marketplace.json](.claude-plugin/marketplace.json)) hosting the `find-paris-restaurant` plugin ([find-paris-restaurant-plugin/](find-paris-restaurant-plugin/)). Install and update it with the built-in `/plugin` commands — no manual file copying needed.

### Install

From any Claude Code session:

```shell
/plugin marketplace add Guobi/paris-food
/plugin install find-paris-restaurant@paris-food
```

If the install summary says `Run /reload-plugins to activate.`, run that command. The skill is then available as `/find-paris-restaurant:find-paris-restaurant`, and Claude will also invoke it automatically for restaurant-recommendation requests.

Claude Code copies the plugin into a local cache at `~/.claude/plugins/cache/` rather than running it from this repo — it doesn't touch the user's own project or `~/.claude/skills/`. Each installed version gets its own cache directory; old versions are cleaned up automatically ~14 days after an update.

### Update

When a new version is pushed to this repo:

```shell
/plugin marketplace update paris-food
```

Claude Code checks the plugin's `version` field in `plugin.json` and pulls the latest if it changed.

### Working on this repo locally

Since the skill now lives under `find-paris-restaurant-plugin/` instead of `.claude/skills/`, it isn't auto-loaded — you need to explicitly load or install it to use it inside this repo:

**Test flag (no install, current session only):**

```bash
claude --plugin-dir ./find-paris-restaurant-plugin
```

**Install from the local marketplace** (mirrors the real install, but points at this working copy instead of GitHub):

```shell
/plugin marketplace add .
/plugin install find-paris-restaurant@paris-food
```

Either way it shows up namespaced as `/find-paris-restaurant:find-paris-restaurant` rather than the plain `find-paris-restaurant` name it had before the move.

## Files

- [find-paris-restaurant-plugin/.claude-plugin/plugin.json](find-paris-restaurant-plugin/.claude-plugin/plugin.json) — the plugin manifest (name, description, version)
- [SKILL.md](find-paris-restaurant-plugin/skills/find-paris-restaurant/SKILL.md) — the skill definition Claude follows
- [template.html](find-paris-restaurant-plugin/skills/find-paris-restaurant/template.html) — HTML template for the generated recommendation page; keep the two-column layout and `{{PLACEHOLDER}}` naming convention intact when editing

## Constraints

- Only the three sources above may be used for candidates/info — don't add others without updating `SKILL.md`.
- Recommendations must reflect current hours/availability (live search), not cached or invented data.
