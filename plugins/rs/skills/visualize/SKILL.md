---
name: visualize
description: >
  One visual answer: show the answer mainly as styled cards, charts, tables, and
  diagrams instead of plain text, choosing the form that fits the content.
  Applies to the next answer only. Use when the user says "visualize", "show it
  visually", "show me", "as cards", "make a chart", "graph it", "put it in a
  table", "compare visually", or asks for options, rankings, or numbers they
  want to see at a glance.
---

# visualize

Answer with visuals first and short text second. Applies to **one answer only**.

## Which answer

- If the user wrote a question with the command, answer it now: $ARGUMENTS
- If nothing was written with the command, apply this to your answer to the user's next message. Confirm in a few words, for example "OK, next answer will be visual."

After that one answer, go back to how you answered before (normal, or brief if brief mode is on).

## Pick the form from the content

| Content                                   | Form                                                     |
|-------------------------------------------|----------------------------------------------------------|
| Options to choose from (places, products, tools) | One card each: name, rating, 2 to 5 key facts, a short verdict |
| Numbers over time                         | Line or area chart                                       |
| Numbers per category, rankings            | Bar chart, sorted                                        |
| Parts of a whole                          | Stacked bar or donut                                     |
| Several items × several attributes        | Comparison table, best value per row highlighted         |
| Steps, processes, how things connect      | Flow or box diagram                                      |
| Key figures                               | Stat tiles: one big number, a label, a change indicator  |

Combine forms when it helps, for example stat tiles above a chart. Use real data from the conversation or your knowledge. Never invent numbers to fill a chart; if data is missing or estimated, say so.

**Nothing worth showing** (a yes/no question, a single fact, a short definition): answer in plain text and add one line saying a visual would not add anything here.

## claude.ai: Visualizer widgets

If the `mcp__visualize__show_widget` tool is available:

1. Call `mcp__visualize__read_me` once per conversation, silently, before the first widget. Load the modules you need (`chart`, `diagram`, `interactive`, `mockup`, `art`).
2. Call `mcp__visualize__show_widget` with an HTML fragment (or an `<svg>`). It renders inline in the chat.
3. Do not use markdown tables or the Artifact tool, unless the user asks for something to keep or share.

### Widget rules

- Send a fragment only: no `<!DOCTYPE>`, `<html>`, `<head>`, or `<body>`.
- The outer background stays transparent. Dark backgrounds go on inner elements such as cards.
- Never use `position: fixed`. The iframe sizes itself to the content.
- Must look right in light and dark mode: give every card its own explicit dark background and light text, or use the provided CSS variables (`--text-primary`, `--surface-2`, `--border`, ...).
- No HTML comments. No font size below 11px.
- `<style>` and `<script>` are allowed. Scripts run after streaming ends, so cards can be built from a JS data array.
- External libraries only from cdnjs.cloudflare.com, cdn.jsdelivr.net, unpkg.com, or esm.sh. Fonts only from Google Fonts.
- Explanations go in the normal chat text, not inside the widget.

### Style: dark neon glow

The user explicitly wants this style. It overrides the default flat, minimal style guide from `read_me`.

- Cards on a dark background (around `#0f1220` to `#161a2e`) with rounded corners (12 to 16px) and a 1px border in the accent color at low opacity.
- Neon accents: cyan `#22d3ee`, magenta `#e879f9`, lime `#a3e635`, amber `#fbbf24`. One accent per card or series, used consistently.
- Glow with `box-shadow`, for example `0 0 18px rgba(34,211,238,.35)`. Stronger glow and a slight lift (`transform: translateY(-2px)`) on hover, with a short transition.
- Chart lines and bars in the accent colors, with a soft glow (`filter: drop-shadow(...)`). Gridlines faint, labels readable.
- Clear hierarchy: big bold title, ★ ratings or key number prominent, details smaller and muted.
- Responsive grid for cards (`display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr))`).

### Clickable follow-ups

Where a natural next question exists, make the card or chart item clickable with `sendPrompt("...")`, for example a restaurant card sends "Tell me more about Café Luna". Show a small hint such as "Details →" so the user knows it is clickable. Skip it when no useful follow-up exists.

## Claude Code and other text-only places

If the Visualizer tool is not available (Claude Code terminal, or claude.ai without it), use text visuals in the chat. No colors or glow are possible here.

- **Cards:** box-drawing characters, one box per item.
  ```
  ╭─ Café Luna ────────────── ★★★★☆ 4.3 ─╮
  │ 🍝 Italian · €€ · 0.4 km             │
  │ 🟢 Open now · terrace                │
  │ Best for: quiet dinner               │
  ╰──────────────────────────────────────╯
  ```
- **Charts:** Unicode bars, sorted, with values.
  ```
  Berlin   ████████████████░░░░  82
  Vienna   ████████████░░░░░░░░  61
  Graz     ██████░░░░░░░░░░░░░░  30
  ```
- **Tables:** markdown tables for comparisons.
- **Color cues:** 🟢🟡🔴 for good / medium / bad, ▲▼ for change, ★ for ratings.
- **Diagrams:** boxes and arrows (`┌─┐ │ └─┘ → ↓`).

Keep boxes aligned, and keep lines short enough for a normal terminal width (about 80 characters).

## Text around the visual

- One or two sentences before or after the visual: the main takeaway or recommendation.
- Do not repeat in text what the visual already shows.
- Keep exact things exact: names, numbers, code, and commands stay complete and unchanged.
- Answer in the language the user writes in.

## Before sending

1. Does the form match the content (cards for options, charts for numbers)?
2. Is every number real, or clearly marked as an estimate?
3. Widget: fragment only, transparent outer background, no comments, no `position: fixed`, readable in light and dark mode?
4. Is any text repeating what the visual shows? Delete it.
