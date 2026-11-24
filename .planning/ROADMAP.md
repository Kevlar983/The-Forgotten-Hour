# Roadmap: The Salt Archive

## Overview

The project moves from a playable vertical slice to a complete, localized, accessible, App Store-ready mystery. Each phase delivers a coherent player-facing capability, not a technical layer in isolation. Content authoring begins only after the puzzle framework and save/progression systems survive the vertical slice.

**Phase numbering:** Integer phases are planned milestone work. Decimal phases are reserved for urgent insertions.

**Project mode:** Vertical MVP. Each phase should produce something end-to-end playable or verifiable.

## Phases

- [ ] **Phase 1: Foundation and Vertical Slice** — Build the SwiftUI shell, content pipeline, design system, and one complete investigation beat.
- [ ] **Phase 2: Investigation Systems** — Add saves, inventory, journal, case board, hints, replay, and content validation.
- [ ] **Phase 3: Chapters 1–2 and Presentation** — Deliver the first two chapters with art, audio, onboarding, and localization foundation.
- [ ] **Phase 4: Chapters 3–4 and Advanced Puzzles** — Add the clinic and undercroft chapters, advanced puzzle families, and continuity validation.
- [ ] **Phase 5: Chapters 5–6 and Endings** — Complete the narrative, optional content, final Ledger, and all three endings.
- [ ] **Phase 6: Accessibility and Localization** — Make every puzzle fully accessible and ship complete English/Italian content.
- [ ] **Phase 7: Polish, Performance, and QA** — Tune feel, battery, memory, difficulty, and automated/manual verification.
- [ ] **Phase 8: Release Readiness** — Prepare assets, privacy/compliance, TestFlight beta, and App Store submission.

## Phase Details

### Phase 1: Foundation and Vertical Slice

**Goal:** A player can launch the app on iPhone and iPad, investigate one illustrated location, solve one complete puzzle, and see evidence recorded — all offline.  
**Mode:** mvp  
**Depends on:** Nothing (first phase)  
**Requirements:** CORE-01, CORE-04, CORE-05, CORE-06, PUZ-04, UX-01, UX-02, UX-05, TECH-01, TECH-02, TECH-04, TECH-07  
**Success Criteria** (what must be TRUE):
1. A clean install launches to an interactive title screen on iPhone and iPad without a network connection.
2. The player can enter one location, inspect at least three hotspots, solve one puzzle with deterministic feedback, and read the resulting evidence in a basic journal.
3. The project builds with Swift 6, uses the planned package boundaries, and has unit tests for the puzzle validator and progression reducer.
4. The app contains no networking framework, analytics SDK, or third-party runtime dependency.
5. The vertical slice runs at the target frame rate and respects Reduce Motion.

**Plans:** 4 plans

Plans:
- [ ] 01-01: Scaffold the Xcode project, local packages, test targets, and CI build
- [ ] 01-02: Implement the design system, scene canvas, accessible hotspot layer, and onboarding beat
- [ ] 01-03: Implement the first puzzle family, deterministic validator, and evidence reveal
- [ ] 01-04: Run the vertical slice on iPhone/iPad, verify offline behavior, and record architecture decisions

### Phase 2: Investigation Systems

**Goal:** A player can leave and resume the game exactly, manage inventory and journal evidence, use hints, replay a solved puzzle, and trust that save data survives corruption and migration.  
**Mode:** mvp  
**Depends on:** Phase 1  
**Requirements:** CORE-02, CORE-03, EXPL-02, EXPL-03, EXPL-04, EXPL-05, PUZ-05, PUZ-06, PUZ-07, TECH-03, TECH-05  
**Success Criteria** (what must be TRUE):
1. The player can use one autosave and three manual slots; killing the app during a puzzle resumes the same puzzle state and hint tier.
2. The player can inspect, combine, and use items, with invalid combinations explained without losing the item.
3. The journal records evidence, people, places, and fragments; the case board stores meaningful links without punishing speculation.
4. Every puzzle has three ordered hints, and the player can complete it with assistance after the third hint.
5. Replaying a solved puzzle works and does not duplicate rewards or journal entries.
6. The content validator fails the build on duplicate IDs, broken references, missing hints, or missing localization keys.

**Plans:** 4 plans

Plans:
- [ ] 02-01: Implement the versioned save schema, atomic store, autosave policy, slots, and migration fixtures
- [ ] 02-02: Implement inventory, journal, chapter recap, and case-board state and views
- [ ] 02-03: Implement the hint engine, assisted solve, replay, and hint history
- [ ] 02-04: Implement the content schema, validator, debug content browser, and CI gate

### Phase 3: Chapters 1–2 and Presentation

**Goal:** A player can play through "Low Water" and "The Weir" — eight main puzzles, two optional puzzles, two major locations, and the first narrative reveals — with finished presentation and a localization-ready pipeline.  
**Mode:** mvp  
**Depends on:** Phase 2  
**Requirements:** EXPL-01, EXPL-06, UX-03, UX-04, LOC-03  
**Success Criteria** (what must be TRUE):
1. Chapters 1 and 2 are playable end to end, including eight main puzzles and two optional puzzles.
2. The player sees illustrated locations, character portraits, document reading, and chapter recaps that do not spoil unresolved questions.
3. Original music, ambience, and sound effects play with captions and separate volume controls.
4. Every user-facing string in the chapters is a localization key; dates, numbers, and plurals use locale-aware formatting.
5. The chapter endings land the intended revelations in playtest reads.

**Plans:** 4 plans

Plans:
- [ ] 03-01: Finalize the narrative bible, Chapter 1–2 scripts, and clue placement map
- [ ] 03-02: Implement Chapter 1–2 scenes, conversations, puzzles, and journal content
- [ ] 03-03: Implement the audio director, adaptive music, ambience, captions, and settings
- [ ] 03-04: Build the localization key pipeline and run pseudo-localization on Chapters 1–2

### Phase 4: Chapters 3–4 and Advanced Puzzles

**Goal:** A player can investigate the clinic and the undercroft, solve the advanced puzzle families, and experience the central reveal with a consistent timeline and fair difficulty.  
**Mode:** mvp  
**Depends on:** Phase 3  
**Requirements:** PUZ-03, PUZ-08, NARR-06, QA-03  
**Success Criteria** (what must be TRUE):
1. Chapters 3 and 4 are playable with eight main puzzles and three optional puzzles.
2. Every advanced family — logic grid, mathematical, spatial mechanism, audio resonance, and timeline deduction — is introduced in isolation before being combined.
3. The narrative timeline, character knowledge, and clue placement pass a continuity review across Chapters 1–4.
4. Playtests at Advanced and Expert tiers show no main-path dead end; hint text is revised where players stall for the wrong reason.
5. The undercroft reveal is understandable from evidence the player has already collected.

**Plans:** 4 plans

Plans:
- [ ] 04-01: Implement the advanced puzzle views, validators, and accessibility alternatives
- [ ] 04-02: Implement Chapter 3 clinic content, dosage/timeline puzzles, and Orr conversations
- [ ] 04-03: Implement Chapter 4 undercroft content, sea-gate mechanism, and resonance puzzle
- [ ] 04-04: Run continuity review and Advanced/Expert playtests; tune hints and clue placement

### Phase 5: Chapters 5–6 and Endings

**Goal:** A player can complete the full story, discover all optional content, solve the final Ledger, and reach each of the three endings under the correct conditions.  
**Mode:** mvp  
**Depends on:** Phase 4  
**Requirements:** PUZ-01, PUZ-02, PUZ-09, NARR-01, NARR-02, NARR-03, NARR-04, NARR-05  
**Success Criteria** (what must be TRUE):
1. All 24 main puzzles and at least 8 optional puzzles are playable, with every main puzzle unlocking evidence or a journal entry.
2. Chapters 5 and 6 complete the arcs of Iris, Mira, Cormac, Orr, Nell, Tomas, and Father Pike; all named characters have distinct voices and motives.
3. Two primary endings and one secret ending are reachable, with epilogues that reflect the player's choice without labeling one as morally correct.
4. The secret ending unlocks only after the required optional investigation and the Keeper's Question meta-puzzle.
5. The final Ledger requires every prior mechanic but remains solvable with full hints; playtesters describe the solution as fair in retrospect.
6. The complete main path lands in the 8–10 hour target range.

**Plans:** 4 plans

Plans:
- [ ] 05-01: Implement Chapter 5 lighthouse, salt-line, and confession content
- [ ] 05-02: Implement Chapter 6 assembly, final Ledger, and ending choice flow
- [ ] 05-03: Implement all optional puzzles, the Keeper's Question, and secret-ending conditions
- [ ] 05-04: Run full narrative read-through, pacing pass, and ending playtests

### Phase 6: Accessibility and Localization

**Goal:** Every scene and puzzle is playable with VoiceOver, large text, reduced motion, captions, and non-drag/non-audio input, and the entire game is complete in English and Italian.  
**Mode:** mvp  
**Depends on:** Phase 5  
**Requirements:** ACC-01, ACC-02, ACC-03, ACC-04, ACC-05, ACC-06, LOC-01, LOC-02, LOC-04  
**Success Criteria** (what must be TRUE):
1. VoiceOver can navigate every scene, hotspot, document, inventory action, puzzle control, and hint without a dead end.
2. Every puzzle has a complete alternative input path and no required free-drag, pinch, precise timing, or audio-only clue.
3. The largest Dynamic Type setting does not clip clues, controls, or journal text.
4. All color-coded states have a non-color cue; contrast and focus order pass an accessibility audit.
5. A full playthrough in Italian has no missing strings, broken placeholders, or truncated layouts.
6. Switching language in-game updates subsequent text without resetting or corrupting progress.

**Plans:** 4 plans

Plans:
- [ ] 06-01: Audit and implement VoiceOver semantics, focus order, and Dynamic Type across all screens
- [ ] 06-02: Implement non-drag, non-audio, and reduced-motion alternatives for every puzzle family
- [ ] 06-03: Complete Italian localization of narrative, puzzles, UI, captions, and accessibility labels
- [ ] 06-04: Run accessibility audit and Italian language QA; fix all release-blocking findings

### Phase 7: Polish, Performance, and QA

**Goal:** The game feels finished, runs within performance and battery budgets, passes automated and manual verification, and has no known progression blockers.  
**Mode:** mvp  
**Depends on:** Phase 6  
**Requirements:** TECH-06, QA-01, QA-02, QA-04  
**Success Criteria** (what must be TRUE):
1. Cold launch is under 2 seconds on the oldest supported device; puzzle interaction holds 60 fps on supported hardware.
2. A 60-minute mixed session shows no runaway battery drain, memory growth, or audio/timer activity when backgrounded.
3. Unit tests cover every validator, hint path, progression gate, save migration, content invariant, and localization key.
4. UI tests cover new game, resume, hint, solve, replay, and all three endings.
5. The supported iPhone/iPad matrix passes in airplane mode with Reduce Motion, Increase Contrast, Bold Text, and largest Dynamic Type.
6. Playtests at all six difficulty tiers show no unresolved dead end or unfair clue.

**Plans:** 4 plans

Plans:
- [ ] 07-01: Profile and optimize launch, scene transitions, memory, and battery; fix regressions
- [ ] 07-02: Complete automated unit/UI/content/localization test coverage and CI gates
- [ ] 07-03: Run device matrix, accessibility settings matrix, and corruption/recovery tests
- [ ] 07-04: Run final difficulty and narrative playtests; resolve release-blocking findings

### Phase 8: Release Readiness

**Goal:** The game is packaged, documented, privacy-compliant, tested with external players, and ready for App Store submission.  
**Mode:** mvp  
**Depends on:** Phase 7  
**Requirements:** UX-06, REL-01, REL-02, REL-03, REL-04  
**Success Criteria** (what must be TRUE):
1. App Store icon, screenshots, preview, description, keywords, support URL, and privacy URL are complete and accurate.
2. The privacy manifest, age rating, export-compliance answer, and offline claims are verified against the shipped build.
3. A TestFlight beta has run for external players and all release-blocking crashes, progression bugs, and localization issues are resolved.
4. The final archive passes validation, installs from TestFlight, and completes a full offline playthrough.
5. The submission checklist is signed off and the App Store listing contains no unverified claims.

**Plans:** 4 plans

Plans:
- [ ] 08-01: Produce App Store assets, metadata, screenshots, preview, and support/privacy pages
- [ ] 08-02: Verify privacy manifest, age rating, export compliance, and offline/no-tracking claims
- [ ] 08-03: Run TestFlight beta, triage feedback, and fix release blockers
- [ ] 08-04: Build final archive, run submission checklist, and submit for review

## Progress

**Execution Order:**  
Phases execute in numeric order: 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation and Vertical Slice | 0/4 | Not started | - |
| 2. Investigation Systems | 0/4 | Not started | - |
| 3. Chapters 1–2 and Presentation | 0/4 | Not started | - |
| 4. Chapters 3–4 and Advanced Puzzles | 0/4 | Not started | - |
| 5. Chapters 5–6 and Endings | 0/4 | Not started | - |
| 6. Accessibility and Localization | 0/4 | Not started | - |
| 7. Polish, Performance, and QA | 0/4 | Not started | - |
| 8. Release Readiness | 0/4 | Not started | - |

---
*Roadmap created: 2026-11-24*  
*Next action: `$gsd-discuss-phase 1`*
