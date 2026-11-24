# The Salt Archive

## What This Is

The Salt Archive is an original, fully offline mystery puzzle adventure for iPhone and iPad. The player controls Iris Vale, a paper conservator who returns to an isolated tidal town and uncovers why its residents collectively lost one week of memory seventeen years ago. It is built for players who want an atmospheric, fair, hand-crafted deduction game that can be played in short sessions or long investigations.

## Core Value

Every puzzle must reveal a meaningful piece of the mystery, and every revelation must make the player want to solve the next puzzle.

If the story, puzzles, and sense of discovery do not reinforce one another, the game has failed regardless of its production values.

## Requirements

### Validated

(None yet — this is a greenfield project. Requirements become validated only after a shipped, playtested build.)

### Active

- [ ] A complete six-chapter mystery with an original cast, two primary endings, and one secret ending.
- [ ] 24 main puzzles across six difficulty tiers, plus 8 optional secret puzzles.
- [ ] Puzzle types spanning observation, logic, mathematics, visual/spatial reasoning, language, audio, and deduction.
- [ ] Three-tier progressive hints, replayable solved puzzles, and an assisted-solve path that never hard-blocks the story.
- [ ] Illustrated exploration scenes, inventory, investigation journal, case board, and optional conversations.
- [ ] Fully offline play with local autosave, three manual save slots, and exact resume after termination.
- [ ] Native iPhone and iPad layouts with accessible controls and readable typography.
- [ ] English and Italian at launch, with all narrative, puzzle, and UI content localized.
- [ ] No accounts, network calls, telemetry, ads, analytics, or third-party runtime services.
- [ ] App Store-ready privacy posture, performance, and submission assets.

### Out of Scope

- Online accounts, cloud sync, multiplayer, and social features — the game must work forever without a server.
- Ads, in-app purchases, subscriptions, and live operations — the product is a complete premium experience.
- Android, web, and desktop versions — the first milestone is native iOS/iPadOS only.
- User-generated content, level editors, and community puzzle sharing — moderation and network dependencies are incompatible with the offline promise.
- Photorealistic 3D, AR, and motion-controlled gameplay — the art direction is hand-illustrated and intentionally stylized.
- Voice acting at launch — localized text, adaptive music, and sound design deliver the atmosphere without a large cast-recording budget.
- A mandatory tutorial that spoils the mystery — onboarding is embedded in the first investigation.

## Context

The project is intentionally starting from a clean, planning-first repository. An unrelated Expo arcade prototype exists elsewhere in the workspace and is not part of this product.

The design draws on the pleasure of classic mystery puzzle adventures: a dense sense of place, clues that reward attention, and a narrative that changes how earlier scenes are understood. It does not reuse any existing game's characters, plot, puzzles, art, names, or assets.

The target player:

- Enjoys deduction, environmental storytelling, and "one more clue" sessions.
- Wants a complete premium game with no ads, energy timers, or account friction.
- May play primarily on iPhone in 10–20 minute sessions, or on iPad in longer sittings.
- May rely on VoiceOver, larger text, reduced motion, captions, or non-drag input.

## Constraints

- **Technology**: Native SwiftUI, Swift 6, iOS/iPadOS 17 or later. Apple frameworks only at runtime unless a dependency is explicitly approved.
- **Offline**: The app must cold-start and complete every puzzle with airplane mode enabled.
- **Content**: 24 main puzzles, 8 optional puzzles, 6 chapters, 2 primary endings, 1 secret ending. Scope is fixed for v1; additions require a milestone decision.
- **Localization**: English and Italian are launch languages. No user-facing string may be hardcoded in a view.
- **Accessibility**: Every puzzle must have a complete non-drag, non-audio-only, screen-reader-accessible path.
- **Art**: Original hand-illustrated 2D assets. No generated asset may imitate a living artist or an existing franchise.
- **Data**: All progress is local. No personal data leaves the device, and no analytics SDK is embedded.
- **Quality**: Puzzle validators, save migrations, and content integrity are covered by automated tests; difficulty is validated through playtests.
- **Release**: App Store submission requires a privacy manifest, accurate age rating, screenshots, and TestFlight validation.

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Native SwiftUI rather than a cross-platform engine | The game is UI-, text-, and illustration-heavy; SwiftUI gives the best iPhone/iPad accessibility and battery behavior with no runtime bridge. | — Pending |
| Data-driven puzzle content in bundled JSON | Separates narrative/puzzle authoring from UI code, enables build-time validation, and keeps localization deterministic. | — Pending |
| Versioned Codable save files instead of a database | Game progress is a small, deterministic snapshot. Atomic files are easier to migrate, test, back up, and reason about than a schema-heavy local database. | — Pending |
| No network entitlement or analytics | The offline promise is a product feature and a privacy commitment; removing the capability prevents accidental dependencies. | — Pending |
| Three-tier hints with an assisted-solve path | Difficulty should create satisfying resistance, never a dead end. Assistance is recorded but never punished. | — Pending |
| Two primary endings plus one secret ending | The central question is whether remembrance is mercy or justice; a hidden reconciliation path rewards complete investigation. | — Pending |
| English and Italian from the content pipeline onward | Retrofitting localization into narrative-heavy games is costly; both languages are authored and validated together. | — Pending |
| Planning gate before implementation | Game design, architecture, and roadmap must be coherent before code makes irreversible structural choices. | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition:**
1. Requirements invalidated? Move to Out of Scope with a reason.
2. Requirements validated? Move to Validated with a phase reference.
3. New requirements emerged? Add to Active.
4. Decisions to log? Add to Key Decisions.
5. "What This Is" still accurate? Update if drifted.

**After each milestone:**
1. Full review of all sections.
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state.

---
*Last updated: 2026-11-24 after project initialization*
