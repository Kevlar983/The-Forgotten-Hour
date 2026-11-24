# Agent Instructions — The Forgotten Hour

## Current status

**Phase 1 — Foundation and Vertical Slice.** The game is *The Forgotten Hour*; *The Salt Archive* is Adventure One. The design, architecture, and roadmap are approved. Implement only the current planned phase and validate its gates before advancing.

## Required reading before any change

Read, in this order:

1. `.planning/PROJECT.md`
2. `.planning/GAME-DESIGN.md`
3. `.planning/ARCHITECTURE.md`
4. `.planning/REQUIREMENTS.md`
5. `.planning/ROADMAP.md`
6. `.planning/STATE.md`

If a requested change conflicts with these documents, surface the conflict and update the planning artifacts first.

## Product invariants

- Native SwiftUI, Swift 6, iOS/iPadOS 17+.
- Fully offline: no accounts, network calls, telemetry, ads, analytics, or server dependencies.
- Original IP only: no existing characters, plot, puzzle text, art, audio, names, or franchise references.
- English and Italian are first-class launch languages; no hardcoded user-facing strings.
- Every puzzle has a screen-reader path, a non-drag path, and a non-audio-only path.
- Hints are progressive and never punish the player for using them.
- Save data is local, versioned, and migratable.
- No third-party runtime dependency without an explicit decision in `PROJECT.md`.

## Implementation rules

- Put pure game logic in testable modules; keep SwiftUI views thin.
- Puzzle validation must be deterministic and independent of animation or device state.
- Content belongs in validated data files, not hardcoded switch statements in views.
- Write unit tests for every puzzle validator, save migration, and content invariant.
- Use Swift Testing for new unit tests; use XCTest/XCUITest where the toolchain requires it.
- Respect Dynamic Type, VoiceOver, Reduce Motion, and color-independent cues from the first implementation, not as a late pass.
- Pause audio, timers, and expensive effects when the scene is inactive.
- Do not add a network entitlement, analytics SDK, or remote configuration path.

## GSD workflow

- Phase 1 entry point: `$gsd-discuss-phase 1`, then `$gsd-plan-phase 1`.
- Execute only the current planned phase; do not skip ahead in the roadmap.
- After each plan, update `.planning/STATE.md` and record durable decisions in `PROJECT.md`.
- Keep planning documents committed to Git.

## Commit style

Use concise conventional commits, for example:

```text
feat(puzzle): add tide-gate rotation validator
fix(save): migrate v1 snapshots without losing hints
docs(planning): record chapter 3 narrative decision
```
