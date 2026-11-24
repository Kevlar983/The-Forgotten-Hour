# The Salt Archive

A working title for an original, fully offline mystery puzzle adventure for iPhone and iPad, built with SwiftUI.

> **Status: planning only.** The game design, technical architecture, and phased roadmap are defined here. No app implementation has started, by design.

## Premise

Iris Vale returns to the tidal town of Morrow's Reach to settle her late grandmother's estate. The town is polite, weathered, and strangely incurious about the night, seventeen years ago, when every resident lost the same week of memory. A sealed archive, a bell that rings without a ringer, and a note in Iris's own childhood handwriting pull her into a investigation that the whole town has agreed not to remember.

## Project documents

| Document | Purpose |
|---|---|
| [`.planning/PROJECT.md`](.planning/PROJECT.md) | Project context, core value, constraints, and key decisions |
| [`.planning/GAME-DESIGN.md`](.planning/GAME-DESIGN.md) | World, characters, narrative, puzzle framework, art and audio direction |
| [`.planning/ARCHITECTURE.md`](.planning/ARCHITECTURE.md) | SwiftUI/iOS technical architecture, content pipeline, persistence, accessibility, testing |
| [`.planning/REQUIREMENTS.md`](.planning/REQUIREMENTS.md) | Testable v1 requirements, deferred scope, exclusions, traceability |
| [`.planning/ROADMAP.md`](.planning/ROADMAP.md) | Eight delivery phases with goals, plans, and success criteria |
| [`.planning/STATE.md`](.planning/STATE.md) | Current project memory and next action |
| [`AGENTS.md`](AGENTS.md) | Working rules for future implementation sessions |

## Product constraints

- Native iOS/iPadOS, SwiftUI, Swift 6, iOS 17+.
- Fully offline at runtime: no accounts, servers, telemetry, ads, or external APIs.
- Local, versioned save data with autosave and replayable puzzles.
- English and Italian at launch, with a localization-ready content pipeline.
- Accessible by design: VoiceOver, Dynamic Type, reduced motion, captions, and non-drag alternatives.
- Original story, characters, art direction, puzzles, and audio. No third-party intellectual property.

## Planned shape

Six chapters, 24 main puzzles, 8 optional secret puzzles, two primary endings, and one secret ending. Target main-path playtime is 8–10 hours, with optional content extending it.

## Next GSD action

Implementation begins only after Phase 1 is discussed and planned:

```text
$gsd-discuss-phase 1
```

Then:

```text
$gsd-plan-phase 1
```
