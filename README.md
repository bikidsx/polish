# production-polish

**An agent skill that catches the states, details, and edge cases that make AI-built software feel unfinished.**

You know the feeling: the app works, the demo goes fine, and it still feels like something's missing. It's never one bug — it's a pile of small omissions (a missing empty state, mismatched corner radii, generic error copy, a button with no click feedback) that are each too small to name but add up to "this isn't done."

This skill is that pass. Point it at a build and it runs a systematic sweep before you call it finished.

## What it checks

- **States** — empty (first-use / cleared / no-results), loading, error, partial failure, and every interaction state (hover, focus, active, disabled, selected) — the single biggest source of "unfinished" in AI-generated UI
- **Physical details** — spacing rhythm, nested corner radii, real click/tap feedback, restrained motion, tap target sizing, numbers that don't jitter the layout
- **Copy** — swaps generic system text ("Error occurred," "No data") for specific, human copy
- **Edge cases** — zero items, way too many items, slow/broken network, double-clicks, long input strings
- **Consistency** — one reused component instead of one-off styling per screen
- **The bigger gap** — a nudge (not an unprompted rewrite) toward the deeper backend work — auth, validation, secrets — when "feels done" and "is production-ready" diverge

## Install

Works anywhere Claude supports Agent Skills — Claude.ai, Claude Code, Claude Cowork.

**Claude Code / other CLI agents:**
```bash
npx skills add bikidsx/polish
```

**Claude.ai:**
Download [`production-polish.skill`](./production-polish.skill) from this repo and add it from Settings → Capabilities → Skills.

## Usage

It's built to trigger on its own — any time you're building or finishing a UI, feature, or prototype, or you say something feels "off" or ask for it to be "production ready." You can also call it explicitly:

```
/production-polish
```

Run it against something you've already shipped and it'll come back with a before → after table, grouped by category, plus a final checklist — not a wall of "looks good."

## License

Apache License