---
name: production-polish
description: Use this skill whenever building, reviewing, or finishing any UI, app, feature, or prototype — especially AI-generated ones — to make it feel complete and production-ready instead of like a demo. Trigger this any time the user is building software (web apps, components, dashboards, forms, tools) and hasn't explicitly said "just a rough draft," any time they say something feels "off," "unfinished," "not quite right," "missing something," or ask for something to be made "production ready," "polished," or "ready to ship," and proactively at the end of any non-trivial build even if they don't ask — treat a first pass as a draft, not a deliverable, and run this pass before presenting it as done.
---

# Production Polish

Nothing that feels unfinished is unfinished because of one missing thing. It's always a pile of small omissions — a missing empty state here, an inconsistent radius there, generic error copy, a button with no pressed feedback — each too small to name individually but which add up to "something's off." This skill is a systematic pass to catch that pile before calling something done.

Treat this as a checklist to run near the end of a build, not a philosophy to read once. Go through each section against the actual thing you built.

## 1. Map every state (the single biggest cause of "unfinished")

The most common failure in AI-assisted builds is designing only the happy path: valid data, fast network, one user, nothing goes wrong. Real usage is never only that. For every screen, list, or component, check it against these states and give each an intentional design — not just whatever happens to render by default:

**Data states**
- **Empty, first use** — user has never created anything yet. This is often someone's first impression of the whole feature; treat it as a real screen, not a fallback. Explain what the area is for and give one clear action to take.
- **Empty, cleared** — user deleted everything. Different tone than first-use; they know what this is, so don't re-explain it from scratch.
- **Empty, no results** — a search or filter matched nothing. Say so plainly and offer a way out (clear filters, try another term), not just "no data."
- **Loading** — use a skeleton that roughly matches the real layout, not a generic spinner or blank flash.
- **Loaded** — the happy path. This is the one that always gets built.
- **Error** — be specific about what happened and what to do next (usually a retry action). Never phrase it as if it's the user's fault, and never show a raw stack trace or status code with nothing else.
- **Partial failure** — some data loaded, some didn't. Show what succeeded instead of hiding everything behind one error.
- **Long/extreme content** — what does the layout do with a 200-item list, a 3000-character name, a number with 9 digits? If you haven't tried it, assume it breaks.

**Interaction states**
- Hover (desktop only — don't assume it exists on touch)
- Focus (keyboard navigation must work, and the focus ring must be visible)
- Active/pressed (a real click needs visible, immediate feedback)
- Disabled (with a visible reason, not just a greyed-out mystery)
- Selected (current tab, active filter, checked item — visually distinct)

**Form states**
- Validation that appears at a sensible time (usually on blur, not only on submit)
- A clear, field-level error message next to the field it belongs to
- A distinct in-progress state for the submit action so double-clicks can't fire it twice

If you only have time to fix one category, fix this one. It's the highest-leverage, most commonly skipped.

## 2. Sweat the physical details

These are individually tiny, invisible when done right, and glaring when skipped — that's exactly why they read as "I can't explain what's wrong but something is."

- **Spacing rhythm** — pick a spacing scale (e.g. multiples of 4px) and stick to it. A 16px gap next to a 14px gap in a visually similar spot is invisible on its own but reads as sloppy in aggregate.
- **Nested corners** — when a rounded element sits inside another rounded element, the outer radius should roughly equal the inner radius plus the padding between them. Mismatched nested radii are one of the most common reasons a card or button "feels off" without an obvious cause.
- **Depth via shadows, not hard borders** — a soft, layered shadow reads as depth on any background; a flat border often looks like a stray line, especially in dark mode.
- **Feedback on every interactive element** — clicks, taps, and drags should visibly respond within a fraction of a second: a slight scale-down on press, a color shift, something. Silence after an action reads as broken, not calm.
- **Animation restraint** — use transitions (interruptible) for anything driven by user interaction, and save one-shot keyframe animations for sequences that play once. Entrances can be a little more expressive (staggering related pieces in over ~100ms apart reads as intentional); exits should be quicker and more subdued than entrances. Don't let anything animate automatically on first page load unless it's meant to.
- **Numbers that update in place** (counters, prices, timers) should use tabular/monospaced figure widths so the layout doesn't jitter as digits change.
- **Text wrapping** on headings and short lines should avoid leaving one orphaned word on its own line.
- **Minimum tap/click target** — interactive elements need a real hit area of roughly 40–44px even if the visible icon or label is smaller; don't let two small hit areas overlap.
- **Typography restraint** — 3–4 font sizes/weights, reused consistently, beats a different size for every element.
- **Consistent meaning-to-style mapping** — if red means "destructive" in one place, it should mean that everywhere; don't reuse the same color for two different meanings.

## 3. Write real copy, not placeholder copy

Generic system-sounding text is one of the fastest tells that something is a draft: "Error occurred," "No data," "Item 1," "Submit." A few seconds of specificity changes the whole feel:

- Name things for what they actually are ("No projects yet" beats "Nothing here")
- Give buttons a verb tied to the actual action ("Save changes," "Create your first project") instead of a generic label ("Submit," "OK")
- Error messages should say what happened and what to do about it, in plain language, without blaming the user or leaking internals
- A "you're all caught up" or fully-cleared empty state can afford to feel like a small reward, not just an absence

## 4. Handle the edges, not just the middle

Before calling a feature done, actually try (don't just imagine) what happens with:
- Zero items, exactly one item, and far more items than the design assumed
- The longest plausible input a real user would type
- A slow, dropped, or failed network request — including a double-click or double-submit
- A user who lacks permission to see or do something
- Two people editing or viewing the same thing at once, if that's possible in this product

If any of these produce a blank screen, a crash, a console error, or silence, that's the gap that will get noticed first once real people use it, even though it never comes up in a demo where you already know what to click.

## 5. Keep a small, consistent system instead of one-off styling

Reuse the same button, input, and card components everywhere rather than restyling per screen — inconsistency compounds fast across a whole app in a way it never does on a single screen in isolation. If a pattern (a modal, a list row, a settings toggle) appears more than once, it should be the same component wearing different data, not a fresh implementation each time.

## 6. When "production ready" means more than the UI

If this is heading toward real users rather than a demo, "feels finished" and "is actually production-ready" are related but different bars. The gap that shows up after launch is usually structural, not visual: things that work fine with one trusting user in a demo and break under real conditions. Flag it if the project would benefit from a deeper pass on:
- Server-side validation (never trust checks that only happen in the browser)
- Auth and permission edge cases (expired sessions, two users, wrong role)
- Secrets never shipped to the client (API keys, tokens)
- Retries/timeouts around external calls instead of assuming they always succeed
- Data actually persisting across sessions/reloads, not just living in memory

This skill's checklist above covers the "feels complete" bar. This section is a nudge to ask, not to silently assume — this deeper pass is a different, larger piece of work and worth calling out as optional rather than doing unprompted.

## Running the pass

When you use this skill on something you (or the user) built, work through sections 1–5 against the actual screens/components involved, then report back concisely:

- Group findings by section (States, Details, Copy, Edge cases, Consistency)
- For each thing you changed or flagged, give a short **before → after**, e.g.: `Empty task list → showed nothing` → `Empty task list → "No tasks yet" + Create task button`
- Skip sections where nothing needed fixing — don't pad the report with "looks good" rows
- End with anything from section 6 worth flagging, only if this looks like it's headed to real users

### Final checklist

- [ ] Every list/screen has an intentional empty state (and the right empty-state *variant*)
- [ ] Loading uses a skeleton or clear indicator, never a blank flash
- [ ] Errors are specific, blame-free, and offer a next step
- [ ] Forms validate at a sensible time and show field-level errors
- [ ] Every clickable element gives immediate visual feedback on interaction
- [ ] Hover, focus, active, disabled, and selected states all exist where relevant
- [ ] Spacing and corner radii are consistent, including nested elements
- [ ] Nothing animates unprompted on first load
- [ ] Numbers that update don't shift surrounding layout
- [ ] Copy is specific to the context, not generic system text
- [ ] Tried it with zero items, one item, way too many items, and a very long input
- [ ] Tried it with a failed/slow network and a double-click
- [ ] Repeated UI patterns reuse the same component, not one-off styling
