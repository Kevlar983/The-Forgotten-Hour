# Requirements: The Forgotten Hour

**Adventure One:** The Salt Archive  
**Defined:** 2026-11-24  
**Core Value:** Every puzzle must reveal a meaningful piece of the mystery, and every revelation must make the player want to solve the next puzzle.

## v1 Requirements

Requirements for the initial App Store release. Each maps to exactly one roadmap phase.

### Core and Offline

- [ ] **CORE-01**: Player can launch, start a new game, and complete the full story with no network connection, account, or sign-in.
- [ ] **CORE-02**: Player can use one autosave and three manual save slots, and can load any slot from the title screen.
- [ ] **CORE-03**: Player can terminate the app at any point and resume at the same scene, puzzle state, inventory, journal entry, and hint tier.
- [ ] **CORE-04**: Player can play the complete game on both iPhone and iPad with layouts designed for each size class.
- [ ] **CORE-05**: App performs no runtime network calls and contains no server, analytics, advertising, or remote-configuration dependency.
- [ ] **CORE-06**: App collects no personal data, requests no tracking permission, and ships a privacy manifest declaring no collected data.

### Exploration and Investigation

- [ ] **EXPL-01**: Player can explore six chapters across at least 30 illustrated locations, including tide and story-state variants.
- [ ] **EXPL-02**: Player can tap or focus hotspots to inspect scenes, read documents, collect items, start conversations, or begin puzzles.
- [ ] **EXPL-03**: Player can inspect, combine, and use inventory items, with clear feedback when a combination is invalid.
- [ ] **EXPL-04**: Player has an investigation journal that automatically records evidence, people, places, recovered fragments, and chapter recaps.
- [ ] **EXPL-05**: Player can use a case board to pin evidence and label relationships, with meaningful links validated without punishing speculation.
- [ ] **EXPL-06**: Player sees a chapter recap on entry that summarizes known facts without spoiling unresolved questions.

### Puzzles

- [ ] **PUZ-01**: Player can solve 24 main puzzles distributed across six chapters and six difficulty tiers.
- [ ] **PUZ-02**: Player can find and solve at least 8 optional secret puzzles that expand lore and unlock the secret ending path.
- [ ] **PUZ-03**: Player encounters observation, cipher, logic-grid, spatial, mathematical, audio, linguistic, deduction, and meta puzzle families.
- [ ] **PUZ-04**: Player receives deterministic, clear success and failure feedback for every puzzle, independent of animation timing.
- [ ] **PUZ-05**: Player can request three progressive hint tiers for every puzzle: Point, Principle, and Reveal.
- [ ] **PUZ-06**: Player can replay any solved puzzle from the journal or case board without duplicating rewards or journal entries.
- [ ] **PUZ-07**: Player can complete any puzzle with assistance after the full hint trail, and the story continues without penalty.
- [ ] **PUZ-08**: Player is taught each new mechanic in isolation before a puzzle combines it with an earlier mechanic.
- [ ] **PUZ-09**: Player can unlock a final meta-puzzle and the secret ending by completing the optional investigation content.

### Narrative

- [ ] **NARR-01**: Player experiences six narrative chapters with a main-path playtime of 8–10 hours.
- [ ] **NARR-02**: Player meets an original cast of at least nine named characters with distinct voices, motives, and arcs.
- [ ] **NARR-03**: Every main puzzle reveals story, character, or world information recorded in the journal.
- [ ] **NARR-04**: Player can reach two primary endings and one secret ending, each with a distinct epilogue.
- [ ] **NARR-05**: Player can read optional lore, letters, marginalia, and conversations that deepen the town and its history.
- [ ] **NARR-06**: Narrative timeline, character knowledge, and clue placement remain consistent across all chapters and endings.

### Experience and Presentation

- [ ] **UX-01**: Player sees an original hand-illustrated visual identity with layered environments, readable documents, and character portraits.
- [ ] **UX-02**: Player experiences elegant transitions that respect Reduce Motion and never block interaction.
- [ ] **UX-03**: Player hears an original adaptive soundtrack, ambience, and sound effects, with separate volume controls.
- [ ] **UX-04**: Player can read captions or transcripts for all meaningful sounds and dialogue.
- [ ] **UX-05**: Player learns investigation, inventory, journal, and hint mechanics through embedded onboarding in Chapter 1.
- [ ] **UX-06**: Player sees no advertisement, upsell, energy timer, or artificial wait between story beats.

### Accessibility

- [ ] **ACC-01**: Player can navigate every scene, hotspot, document, inventory action, puzzle, and hint using VoiceOver.
- [ ] **ACC-02**: Player can use Dynamic Type and a dedicated reading view at the largest supported text sizes without clipped clues.
- [ ] **ACC-03**: Player can distinguish all puzzle states without color alone; color-coded clues also use shape, pattern, label, or position.
- [ ] **ACC-04**: Player can complete every puzzle without free-drag, pinch, precise timing, or hearing an audio-only clue.
- [ ] **ACC-05**: Player can pause at any time; no puzzle imposes a fail timer or penalizes leaving the app.
- [ ] **ACC-06**: Player can configure Reduce Motion, Reduce Visual Effects, text speed, captions, and volume from an in-game settings screen.

### Localization

- [ ] **LOC-01**: Player can play the complete game in English or Italian, including all narrative, puzzle, UI, and accessibility text.
- [ ] **LOC-02**: Player sees no hardcoded user-facing string in any view, puzzle, or error path.
- [ ] **LOC-03**: Player sees locale-correct dates, times, numbers, and plurals through the content and string-catalog pipeline.
- [ ] **LOC-04**: Player can switch language in-game, with the change applied to subsequent text without corrupting save state.

### Technical Quality

- [ ] **TECH-01**: App is built with SwiftUI and Swift 6, targeting iOS/iPadOS 17 or later.
- [ ] **TECH-02**: Game logic, puzzle validation, content, persistence, design system, and audio are separated into testable modules with thin views.
- [ ] **TECH-03**: Player save data uses a versioned schema with tested migrations and a recovery path for corrupted autosaves.
- [ ] **TECH-04**: Every puzzle validator and progression reducer is pure, deterministic, and covered by unit tests.
- [ ] **TECH-05**: Build fails on duplicate content IDs, broken references, missing hints, unreachable puzzles, or missing localization keys.
- [ ] **TECH-06**: Player experiences 60 fps puzzle interaction, sub-2-second cold launch, and no audio/timer activity while the app is inactive.
- [ ] **TECH-07**: App ships without third-party runtime dependencies unless an explicit architecture decision approves one.

### QA and Playtesting

- [ ] **QA-01**: Team can run unit tests for puzzle validators, hint logic, progression, content invariants, localization, and save migrations.
- [ ] **QA-02**: Team can run UI tests for new game, resume, hint, solve, replay, and each ending.
- [ ] **QA-03**: Team has playtested every difficulty tier and tuned hints so no main-path puzzle is a dead end.
- [ ] **QA-04**: Team has validated the game on the supported iPhone/iPad device matrix, in airplane mode, with accessibility settings enabled.

### Release

- [ ] **REL-01**: Team has produced App Store icon, screenshots, preview, description, keywords, and support/privacy URLs.
- [ ] **REL-02**: App has an accurate age rating, privacy manifest, export-compliance answer, and no unresolved App Store policy issues.
- [ ] **REL-03**: Team has shipped a TestFlight beta, collected feedback, and resolved release-blocking crashes and progression bugs.
- [ ] **REL-04**: Team has completed the App Store submission checklist, including offline verification, localization review, and final archive validation.

## v2 Requirements

Deferred to a future release. Tracked, but not part of the current roadmap.

### Languages and Platforms

- **LANG-01**: Player can play in French, German, and Spanish with fully authored localization.
- **PLAT-01**: Player can play on macOS through a native Catalyst or SwiftUI macOS target.

### Comfort and Personalization

- **PERS-01**: Player can enable an iCloud-backed save that syncs progress across iPhone and iPad.
- **PERS-02**: Player can select high-contrast, large-print, and dyslexia-friendly reading themes.
- **PERS-03**: Player can adjust puzzle difficulty assists independently of the hint system.

### Optional Content

- **CONT-01**: Player can access post-launch side stories set in Morrow's Reach.
- **CONT-02**: Player can unlock a New Game Plus mode with author commentary and alternate clue annotations.
- **ACHV-01**: Player can earn local achievements and optional Game Center recognition without affecting story access.

## Out of Scope

Explicitly excluded from v1. Documented to prevent scope creep.

| Feature | Reason |
|---|---|
| Online accounts, cloud save, multiplayer | Requires servers and identity; contradicts the offline, no-account promise. |
| Ads, in-app purchases, subscriptions, battle passes | The product is a complete premium game; monetization would compromise pacing and trust. |
| Android, web, Windows | The first milestone is native iOS/iPadOS only. |
| User-generated puzzles or level editor | Moderation, sharing, and network dependencies conflict with the product constraints. |
| Voice acting | Cost and localization burden exceed v1 scope; text, music, and sound design carry the narrative. |
| 3D, AR, motion controls, photoreal art | Incompatible with the hand-illustrated 2D direction and battery targets. |
| Real-time combat, fail states, timers | The game is a deduction adventure; pressure should come from the mystery, not reflexes. |
| Analytics, telemetry, crash-reporting SDKs | No personal data leaves the device; QA uses manual playtests and local test artifacts. |
| In-game purchases of hints or content | Hints are free and complete; no player should be blocked by payment. |
| User accounts and profiles | No identity is needed to play or save. |

## Traceability

Which phase covers which requirement. Every v1 requirement is mapped exactly once.

| Requirement | Phase | Status |
|-------------|-------|--------|
| CORE-01 | Phase 1 | Pending |
| CORE-02 | Phase 2 | Pending |
| CORE-03 | Phase 2 | Pending |
| CORE-04 | Phase 1 | Pending |
| CORE-05 | Phase 1 | Pending |
| CORE-06 | Phase 1 | Pending |
| EXPL-01 | Phase 3 | Pending |
| EXPL-02 | Phase 2 | Pending |
| EXPL-03 | Phase 2 | Pending |
| EXPL-04 | Phase 2 | Pending |
| EXPL-05 | Phase 2 | Pending |
| EXPL-06 | Phase 3 | Pending |
| PUZ-01 | Phase 5 | Pending |
| PUZ-02 | Phase 5 | Pending |
| PUZ-03 | Phase 4 | Pending |
| PUZ-04 | Phase 1 | Pending |
| PUZ-05 | Phase 2 | Pending |
| PUZ-06 | Phase 2 | Pending |
| PUZ-07 | Phase 2 | Pending |
| PUZ-08 | Phase 4 | Pending |
| PUZ-09 | Phase 5 | Pending |
| NARR-01 | Phase 5 | Pending |
| NARR-02 | Phase 5 | Pending |
| NARR-03 | Phase 5 | Pending |
| NARR-04 | Phase 5 | Pending |
| NARR-05 | Phase 5 | Pending |
| NARR-06 | Phase 4 | Pending |
| UX-01 | Phase 1 | Pending |
| UX-02 | Phase 1 | Pending |
| UX-03 | Phase 3 | Pending |
| UX-04 | Phase 3 | Pending |
| UX-05 | Phase 1 | Pending |
| UX-06 | Phase 8 | Pending |
| ACC-01 | Phase 6 | Pending |
| ACC-02 | Phase 6 | Pending |
| ACC-03 | Phase 6 | Pending |
| ACC-04 | Phase 6 | Pending |
| ACC-05 | Phase 6 | Pending |
| ACC-06 | Phase 6 | Pending |
| LOC-01 | Phase 6 | Pending |
| LOC-02 | Phase 6 | Pending |
| LOC-03 | Phase 3 | Pending |
| LOC-04 | Phase 6 | Pending |
| TECH-01 | Phase 1 | Pending |
| TECH-02 | Phase 1 | Pending |
| TECH-03 | Phase 2 | Pending |
| TECH-04 | Phase 1 | Pending |
| TECH-05 | Phase 2 | Pending |
| TECH-06 | Phase 7 | Pending |
| TECH-07 | Phase 1 | Pending |
| QA-01 | Phase 7 | Pending |
| QA-02 | Phase 7 | Pending |
| QA-03 | Phase 4 | Pending |
| QA-04 | Phase 7 | Pending |
| REL-01 | Phase 8 | Pending |
| REL-02 | Phase 8 | Pending |
| REL-03 | Phase 8 | Pending |
| REL-04 | Phase 8 | Pending |

**Coverage:**
- v1 requirements: 58 total
- Mapped to phases: 58
- Unmapped: 0

---
*Requirements defined: 2026-11-24*  
*Last updated: 2026-11-24 after initial roadmap mapping*
