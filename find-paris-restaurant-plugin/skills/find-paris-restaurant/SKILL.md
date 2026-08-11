---
name: find-paris-restaurant
description: Find a good restaurant in Paris matching a person's requirements (cuisine, budget, neighborhood, occasion, dietary needs, etc.) using live web search. Use when the user asks for a restaurant recommendation, where to eat, or help picking a place to dine in Paris.
---

# Find a restaurant

Help the user find a restaurant in Paris that fits their stated requirements, using live web search rather than a fixed dataset — recommendations must reflect current hours, reviews, and availability.

## 1. Gather requirements

Before searching, make sure you know (ask only for what's missing and matters — don't interrogate for details the user won't care about):

- **Cuisine or dish** (e.g. bistro, Japanese, vegan, natural wine bar)
- **Neighborhood / arrondissement** or proximity to a landmark/address
- **Budget** (€, €€, €€€, €€€€, or a per-person price range)
- **Party size and occasion** (date night, group, business lunch, family with kids)
- **Date/time** they plan to go, and whether they need a reservation
- **Dietary constraints** (vegetarian, vegan, gluten-free, allergies, halal/kosher)
- **Ambiance preferences** (quiet, lively, outdoor seating, view)
- **Preferred language** for the response (Korean, Japanese, Simplified Chinese, or Hindi) — always ask this one, even if everything else is already specific enough to search

If the user's request is already specific enough to search (e.g. "a cheap ramen spot near the Marais open on a Sunday"), don't ask clarifying questions — go straight to searching. Only ask when a missing detail would materially change the result (e.g. budget or neighborhood).

When you do need to ask, use the `AskUserQuestion` tool (multiple-choice, one question per missing requirement) rather than asking in free text — it's faster for the user to answer.

## 2. Search

Only use these sources for restaurant candidates and information — do not pull from other sites:

- https://leguideparisien.com/restaurants/
- https://www.timeout.fr/paris/restaurant/50-meilleurs-restaurants
- https://www.sortiraparis.com/

Use WebFetch (or WebSearch scoped to these sites, e.g. `site:sortiraparis.com [cuisine] [neighborhood]`) to find current candidates. Favor:

- Checking that a place is still open (restaurants close/change often) and matches the stated budget and dietary needs
- Cross-referencing more than one of the three sources when possible, especially for hours and whether a reservation is required

Run multiple targeted searches rather than one broad query — e.g. separately search each site for "best [cuisine] restaurant [neighborhood] Paris" and "[cuisine] Paris [dietary constraint]" if needed.

## 3. Present results

Write the final recommendations in French and in the language the user chose (Korean, Japanese, Simplified Chinese, or Hindi).

Give 2-4 ranked options, not an exhaustive list. For each, include:

- Name and neighborhood
- Address
- Working hours
- A photo of the restaurant (find a URL from the source page, its Google/Maps listing, or official website — omit the image if none can be found, don't invent one)
- Why it fits their requirements (one line)
- Approximate price range
- Anything time-sensitive: needs reservation, closed certain days, etc.
- A source or link if available

If nothing fits all constraints well, say so plainly and offer the closest tradeoffs rather than forcing a match.

## 4. Save an HTML version

In addition to the chat reply, save the results as a standalone HTML file so the user can view or share it:

1. Copy [template.html](template.html) as a starting point.
2. Duplicate the `.restaurant` block once per recommended restaurant, filling in both columns — the left (`.col.fr`) in French, the right (`.col.lang`) in the user's chosen language, including labels ("Adresse"/local translation, "Horaires"/local translation, etc.).
3. Fill `{{PHOTO_URL}}` with a real photo URL found during search; if none is available, remove that restaurant's `<img>` tag rather than leaving a placeholder or fake URL.
4. Set `{{TITLE_FR}}` / `{{TITLE_LANG}}` to something like "Nos recommandations" / the equivalent in the user's language.
5. Save the finished file in the current working directory as `restaurants-<short-slug>.html` (e.g. `restaurants-marais-ramen.html`), and tell the user the file path.
