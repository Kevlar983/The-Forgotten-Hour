# Technical Architecture — The Salt Archive

**Status:** Design only. No app target or source code exists yet.  
**Target:** iPhone and iPad, iOS/iPadOS 17+, Swift 6, SwiftUI.  
**Runtime dependencies:** Apple frameworks only unless a dependency is explicitly approved and recorded in `PROJECT.md`.

---

## 1. Architectural Goals

1. **Fully offline by construction.** No network entitlement, no remote configuration, no analytics, no account state. The app must be playable in airplane mode from first launch to ending.
2. **Deterministic game logic.** Puzzle validation and progression depend only on content, player input, and saved state — never on frame timing, animation completion, or device capabilities.
3. **Data-driven content.** Chapters, scenes, puzzles, hints, journal entries, and localization live in validated data, not hardcoded view branches.
4. **Thin SwiftUI views.** Views render state and send intents. They do not contain puzzle rules, progression rules, or save logic.
5. **Perfect resume.** A killed app returns to the same scene, puzzle state, inventory, journal, hints, and audio context.
6. **Accessible by default.** Every interactive element has a semantic model that VoiceOver and alternative input can use.
7. **Battery-conscious.** No polling loops, no always-on particle systems, no audio or timers while inactive.
8. **Migratable saves.** A player who finishes the game in 2027 must be able to open a 2026 save after updates.

## 2. Non-Goals

- A general-purpose game engine or editor.
- User-generated content, modding, or remote content delivery.
- Cross-platform abstraction; iOS/iPadOS is the only target.
- Cloud sync, accounts, leaderboards, or social features in v1.
- Real-time 3D, AR, physics simulation, or online multiplayer.
- A database-backed content management system; content is versioned in Git.

## 3. Stack and Platform Baseline

| Concern | Choice | Rationale |
|---|---|---|
| Language | Swift 6 | Strict concurrency, modern ownership, long-term maintainability |
| UI | SwiftUI | Native accessibility, adaptive layouts, low overhead for a 2D narrative game |
| Minimum OS | iOS/iPadOS 17 | Observation framework, modern navigation, current accessibility APIs |
| State observation | `@Observable` and `@MainActor` session model | Clear isolation of UI state; avoids Combine boilerplate |
| Persistence | Versioned `Codable` snapshots written atomically | Small deterministic state; easy migrations and tests |
| Content | Bundled JSON + JSON Schema + build-time validator | Authoring/UI separation; catches broken references before ship |
| Localization | String Catalogs (`.xcstrings`) + localized content keys | First-class English/Italian, pluralization, App Store tooling |
| Audio | AVAudioEngine / AVAudioPlayer with bundled assets | Offline, adaptive layers, precise lifecycle control |
| Images | Asset catalogs with downsampled variants | Memory and battery control; iPad/iPhone variants |
| Testing | Swift Testing for unit tests; XCTest/XCUITest for UI | Native toolchain, fast logic tests, end-to-end coverage |
| CI | GitHub Actions macOS runner | Build, unit tests, content validation, archive checks |

No third-party runtime package is planned. If one becomes necessary, it must be justified in a decision record, checked for privacy/offline behavior, and pinned.

## 4. Module Structure

The project is split into an app shell and local Swift packages so that game logic can be tested without launching SwiftUI.

```text
TheSaltArchive/
├── App/
│   ├── TheSaltArchiveApp.swift
│   ├── AppEnvironment.swift
│   ├── RootView.swift
│   └── Routes/
├── Packages/
│   ├── GameCore/          # Session, progression, inventory, journal, case board
│   ├── PuzzleKit/         # Puzzle definitions, validators, hint engine
│   ├── ContentKit/        # Codable content models, localization keys, validation
│   ├── PersistenceKit/    # Save snapshots, atomic storage, migrations
│   ├── DesignSystem/      # Colors, typography, components, motion tokens
│   └── AudioKit/          # Music/ambience/SFX, captions, lifecycle
├── Content/
│   ├── chapters/
│   │   ├── 01-low-water.json
│   │   └── ...
│   ├── schema/
│   │   └── game-content.schema.json
│   └── Localizable.xcstrings
├── Resources/
│   ├── Art/
│   ├── Audio/
│   └── Fonts/
├── Tests/
│   ├── GameCoreTests/
│   ├── PuzzleKitTests/
│   ├── ContentKitTests/
│   └── PersistenceKitTests/
└── UITests/
```

### 4.1 Dependency direction

```text
App
 ├── DesignSystem
 ├── AudioKit
 ├── GameCore
 │    ├── PuzzleKit
 │    ├── ContentKit
 │    └── PersistenceKit
 └── ContentKit (content loading at composition root)

PuzzleKit ──> ContentKit
ContentKit ──> Foundation only
PersistenceKit ──> Foundation only
DesignSystem ──> SwiftUI only
AudioKit ──> AVFoundation + ContentKit (for localized captions)
```

`GameCore` owns the session and orchestrates the others. `PuzzleKit` and `PersistenceKit` never import SwiftUI. `DesignSystem` and `AudioKit` never own game rules.

## 5. Content Model

Content is authored as JSON, validated against a schema at build time, and decoded into immutable value types. All user-facing text is a localization key, not a literal.

### 5.1 Core types

```swift
struct GameContent: Codable, Sendable {
    let schemaVersion: Int
    let chapters: [Chapter]
    let items: [ItemDefinition]
    let endings: [EndingDefinition]
}

struct Chapter: Codable, Sendable, Identifiable {
    let id: ChapterID
    let titleKey: LocalizationKey
    let locations: [Location]
    let puzzles: [PuzzleDefinition]
    let journalEntries: [JournalEntryDefinition]
}

struct Location: Codable, Sendable, Identifiable {
    let id: LocationID
    let nameKey: LocalizationKey
    let backgroundAsset: String
    let hotspots: [Hotspot]
    let ambientAudio: AudioCue?
}

struct Hotspot: Codable, Sendable, Identifiable {
    let id: HotspotID
    let accessibilityKey: LocalizationKey
    let shape: NormalizedShape
    let action: HotspotAction
    let requiredFlags: [Flag]
}

enum HotspotAction: Codable, Sendable {
    case inspect(LocalizationKey)
    case startPuzzle(PuzzleID)
    case collectItem(ItemID)
    case conversation(ConversationID)
    case travel(LocationID)
    case endingChoice
}
```

### 5.2 Puzzle definition

```swift
struct PuzzleDefinition: Codable, Sendable, Identifiable {
    let id: PuzzleID
    let family: PuzzleFamily
    let difficulty: DifficultyTier
    let promptKey: LocalizationKey
    let ruleKey: LocalizationKey          // one-sentence rule for hints
    let hints: [HintDefinition]           // exactly three tiers
    let evidenceKey: LocalizationKey
    let journalEntryID: JournalEntryID
    let validator: ValidatorDescriptor
    let accessibility: PuzzleAccessibility
    let estimatedMinutes: Int
}
```

`ValidatorDescriptor` selects a pure validator and supplies its data. A validator signature is:

```swift
protocol PuzzleValidating: Sendable {
    associatedtype Input: Sendable
    associatedtype Configuration: Sendable
    func evaluate(_ input: Input, configuration: Configuration) -> PuzzleEvaluation
}
```

`PuzzleEvaluation` is a value type with `.solved`, `.incomplete(reason:)`, or `.invalid(reason:)`. It contains no UI text; the UI maps reason codes to localized feedback.

### 5.3 Content invariants

The build-time validator rejects content with:

- Duplicate IDs across chapters, puzzles, hotspots, items, or journal entries.
- A hotspot that references a missing puzzle, item, conversation, or location.
- A puzzle with fewer or more than three hint tiers.
- A main puzzle that unlocks no journal entry or evidence.
- A chapter with fewer than four main puzzles or one optional puzzle.
- A localization key missing from English or Italian.
- A localized string with mismatched format placeholders.
- An ending condition that references an unknown flag or item count.
- A save migration target that does not exist.

## 6. Session and State Model

`GameSession` is the single source of truth for the active game. It is `@MainActor @Observable` because it drives SwiftUI, but all rule evaluation is delegated to pure functions.

```swift
@MainActor @Observable
final class GameSession {
    private(set) var state: GameState
    private(set) var content: GameContent
    private let saveStore: SaveStoring
    private let validator: PuzzleValidatorRegistry
    private let hintEngine: HintEngine
    private let clock: GameClock

    func send(_ action: GameAction) async { ... }
}
```

`GameState` is a `Codable`, `Sendable` value:

```swift
struct GameState: Codable, Sendable {
    var schemaVersion: Int
    var chapterID: ChapterID
    var locationID: LocationID
    var flags: Set<Flag>
    var inventory: [ItemID: Int]
    var solvedPuzzles: Set<PuzzleID>
    var assistedPuzzles: Set<PuzzleID>
    var puzzleStates: [PuzzleID: PuzzleRuntimeState]
    var journal: JournalState
    var caseBoard: CaseBoardState
    var hintsUsed: [PuzzleID: HintTier]
    var endingState: EndingState?
    var playTimeSeconds: Int
    var lastSavedAt: Date
}
```

### 6.1 Action flow

```text
User tap / gesture
  → View sends GameAction
  → GameSession validates action against current state
  → Pure reducer mutates GameState
  → Side effects (save, audio, haptics) are requested through protocols
  → SwiftUI re-renders from new state
```

Side effects are explicit and injectable:

- `SaveStoring` — autosave after meaningful actions.
- `AudioControlling` — play/stop cues, ducking, lifecycle.
- `HapticControlling` — optional, disabled by Reduce Motion or accessibility settings.
- `Analytics` — intentionally absent.

### 6.2 Determinism

- Puzzle validators are pure and synchronous; they receive all data they need.
- Animations observe state but never gate state transitions.
- Timed puzzles, if any, use an injectable clock and pause when the app is inactive. The design currently has no fail-on-time puzzle.
- Randomization is prohibited in puzzle solutions. If a visual shuffle is used, the seed is saved so a reload shows the same layout.

## 7. Navigation and View Composition

Navigation uses a typed `NavigationStack` path owned by the app shell, not by individual views.

```swift
enum Route: Hashable {
    case chapterIntro(ChapterID)
    case location(LocationID)
    case conversation(ConversationID)
    case puzzle(PuzzleID)
    case journal(JournalTab)
    case caseBoard
    case ending(EndingID)
}
```

**Composition:**

- `RootView` selects between title screen and active game.
- `GameShellView` owns the navigation path and the persistent HUD (journal, inventory, hints).
- `LocationView` renders the layered scene and accessible hotspot list.
- `PuzzleHostView` selects the puzzle view from its family descriptor.
- `JournalView` is a tabbed reader with search and spoiler-safe recaps.
- `EndingView` presents the epilogue and replay options.

**Presentation rules:**

- Puzzles present full-screen on iPhone and as a centered sheet/panel on iPad, never covering the journal permanently.
- The journal is a sheet on iPhone and a persistent sidebar on regular-width iPad.
- Scene transitions use a shared `MotionToken`; Reduce Motion swaps to a crossfade or instant cut.
- Deep links and state restoration are not required for v1, but the route model is serializable to support them later.

## 8. Puzzle Rendering Strategy

Puzzles are grouped by interaction family, not by individual puzzle. Each family has one view that is configured by data.

| Family | View | Implementation notes |
|---|---|---|
| Observation | `ComparisonPuzzleView` | Zoomable document, focusable differences, list fallback |
| Cipher | `CipherPuzzleView` | Text entry, character picker, alphabet strip |
| Logic grid | `LogicGridPuzzleView` | Stepper/selection grid, spoken row/column summaries |
| Spatial | `MechanismPuzzleView` | Rotation buttons + drag; coordinate fallback |
| Mathematical | `CalculationPuzzleView` | Numeric keypad, equation slots, scratchpad |
| Audio | `ResonancePuzzleView` | Waveform, tone buttons, captioned pattern grid |
| Linguistic | `TextDeductionPuzzleView` | Annotatable text, glossary, translation toggle |
| Deduction | `EvidenceBoardPuzzleView` | Premise toggles, contradiction highlighting |
| Meta | `LedgerPuzzleView` | Structured worksheet with saved intermediate conclusions |

Shared components: `PuzzleChrome`, `HintSheet`, `EvidenceReveal`, `PuzzleFeedback`, `AccessibleControlGroup`. Each puzzle view receives a `PuzzleViewModel` that exposes state and intents; it never evaluates the solution itself.

### 8.1 Accessibility contract for puzzle views

Every puzzle view must provide:

- A semantic summary that updates as state changes.
- An alternative control path that does not require drag, pinch, or simultaneous gestures.
- A non-audio representation of any informative sound.
- A "Read the rule" action that reopens the rule text.
- Focus order that follows the puzzle's logical structure.
- No color-only state; selected/disabled/error states use text or shape as well.

---

## 9. Persistence and Save Architecture

### 9.1 Storage layout

```text
Application Support/TheSaltArchive/
├── saves/
│   ├── autosave.json
│   ├── slot-1.json
│   ├── slot-2.json
│   ├── slot-3.json
│   └── backups/
│       └── autosave-<timestamp>.json
└── settings.json
```

All files use `.completeUntilFirstUserAuthentication` file protection. Nothing is written to `Documents` unless the player explicitly exports a save (not in v1).

### 9.2 Write policy

- Autosave after: chapter start, location change, puzzle solved, item acquired/used, journal entry unlocked, hint tier used, ending choice, and app background.
- Writes are atomic: encode to a temporary file, fsync where practical, then replace the destination.
- The previous autosave is retained as a single rolling backup to survive a corrupted write.
- Writes are debounced to at most one per 1.5 seconds during rapid interactions; a final write occurs on scene phase change.
- Save failures are surfaced non-destructively: the game continues in memory, and the player is told that progress may not persist.

### 9.3 Versioning and migration

`GameState.schemaVersion` is independent of the app version. Every migration is a pure function:

```swift
protocol SaveMigration: Sendable {
    var fromVersion: Int { get }
    var toVersion: Int { get }
    func migrate(_ data: Data) throws -> Data
}
```

Migration rules:

- Migrations run in order; a missing step is a hard, logged failure, never a silent reset.
- Unknown future versions are rejected with a clear message rather than partially decoded.
- Solved puzzles, hints used, journal entries, and ending state are preserved across migrations.
- Every migration has a fixture: a checked-in old save and an expected migrated snapshot.
- Content IDs are stable forever. If an ID must change, a content alias maps the old ID to the new one for save compatibility.

### 9.4 Why not SwiftData/Core Data

The save state is a small, bounded snapshot (well under 5 MB), not a queryable dataset. Atomic Codable files are simpler to test, migrate, back up, and inspect. The journal is rendered from content plus state, not stored as rows. This also keeps the persistence layer independent of SwiftUI and model-container lifecycle.

## 10. Content Pipeline

### 10.1 Authoring flow

```text
Narrative/puzzle design
  → chapter JSON + localization keys
  → schema validation
  → localization completeness check
  → content reference graph check
  → build + unit tests
  → in-game debug content browser (DEBUG builds only)
```

### 10.2 Validation tool

A Swift command-line target, `ContentValidator`, runs in CI and as an Xcode pre-build phase. It:

- Parses every chapter file.
- Validates against `game-content.schema.json`.
- Builds a graph of chapters, locations, hotspots, puzzles, items, conversations, journal entries, flags, and endings.
- Fails on orphaned content, unreachable puzzles, missing references, hint tier gaps, duplicate IDs, and ending conditions that cannot be met.
- Verifies every localization key exists in both `en` and `it`, with matching format placeholders.
- Emits a human-readable report and a non-zero exit code on failure.

### 10.3 Debug affordances

DEBUG builds include a content browser that can:

- Jump to any chapter, location, or puzzle.
- Show puzzle state and expected solution.
- Simulate unlocks and endings.
- Toggle a localization pseudo-language and long-string stress mode.
- Reset a single puzzle without wiping the save.

These tools are excluded from Release builds by compilation conditions, not runtime flags.

## 11. Localization Architecture

- UI strings live in `Localizable.xcstrings` and are accessed through generated symbols where possible.
- Narrative and puzzle content use stable localization keys in JSON; the key is never the English sentence.
- Each chapter has parallel English and Italian authoring. Italian is not a post-hoc machine translation.
- Documents that appear as images have a text-layer equivalent in both languages; no important clue is baked into artwork.
- Plurals, dates, times, and numbers use `FormatStyle` / `AttributedString` with the current locale.
- The content validator checks placeholder parity and missing keys.
- Pseudo-localization (long words, accents, mirrored layout dry run) is used to catch truncation before the Italian pass.
- VoiceOver labels are localized separately from visible text where the spoken form needs more context.

## 12. Audio Architecture

- `AudioDirector` owns music, ambience, and effects; it observes scene phase and navigation.
- Audio assets are bundled AAC/CAF files, preloaded per chapter, released on chapter exit.
- Ambient beds loop seamlessly; music layers crossfade over 1–2 seconds.
- The silent switch is respected for effects. Music follows the player's explicit setting.
- All informative sounds have a caption and a visual equivalent; captions are stored as localized strings.
- Audio stops on `.background` and resumes on `.active` without replaying a backlog.
- No audio session is activated during the title screen until the player starts or enables sound.

## 13. Performance and Battery

**Budgets:**

- Cold launch to interactive title: under 2 seconds on the oldest supported device.
- Scene transition: under 350 ms perceived; no blank frame.
- Puzzle interaction: 60 fps on supported devices; 30 fps acceptable only for non-interactive ambience.
- Peak memory: under 350 MB on a 3 GB device; art is downsampled per device class.
- Install size: target under 800 MB; no duplicated full-resolution art across languages.

**Techniques:**

- Downsample and cache scene layers; release chapter assets when leaving a chapter.
- Use `Canvas`/`TimelineView` only for small, bounded effects; no permanent 60 fps particle loops.
- Pause timers, animations, and audio when the scene is inactive.
- Avoid `GeometryReader` in hot paths; prefer layout containers and fixed aspect-ratio scene canvases.
- Profile with Instruments (Time Profiler, Allocations, Energy Log) on physical devices, not only the simulator.
- Provide a "Reduce visual effects" setting in addition to system Reduce Motion.

## 14. Privacy and Security

- No network entitlement. The app must not link a networking framework.
- No analytics, advertising, crash-reporting SDK, or remote logging.
- No accounts, email addresses, contacts, location, camera, microphone, or photo-library access.
- No personal data collection. The privacy manifest declares no collected data and no tracking.
- Save files contain only game progress and settings.
- Debug logging is compiled out of Release; no save contents are logged.
- Content is bundled and code-signed; there is no remote code or downloadable executable content.
- App Store age rating target: 12+ for thematic elements, peril, and grief; no graphic violence, profanity, or gambling.

## 15. Testing Strategy

### 15.1 Unit tests

- Every puzzle validator: correct solution, near-miss, incomplete state, malformed input, and accessibility-alternative input.
- Hint engine: tier ordering, unlock rules, idempotence, and localized key coverage.
- Progression reducer: chapter gates, item use, flags, replay, and ending conditions.
- Save migrations: all historical fixtures, corruption handling, unknown version handling.
- Content validator: intentionally broken fixtures for every invariant.
- Localization: key parity, placeholder parity, and plural coverage.

### 15.2 UI tests

- New game → first scene → first puzzle → evidence → autosave → terminate → resume.
- Hint tier progression and assisted solve.
- Journal open, search, and case-board link.
- Inventory combine/use and replay of a solved puzzle.
- Each ending, including secret-ending unlock conditions.
- VoiceOver-relevant accessibility identifiers and focus order (smoke level).

### 15.3 Manual validation

- iPhone SE-class, standard iPhone, iPhone Pro Max, iPad mini, and standard iPad.
- Airplane mode from a clean install.
- Low Power Mode, Reduce Motion, Increase Contrast, Bold Text, and largest Dynamic Type.
- Italian layout pass with long strings.
- Battery drain test: 30 minutes of exploration and 30 minutes of puzzle play.
- Save corruption simulation and recovery from backup.

### 15.4 CI

GitHub Actions macOS runner:

1. Build all packages and the app for iOS Simulator.
2. Run Swift Testing and XCTest suites.
3. Run `ContentValidator` against all chapters and both localizations.
4. Run `xcodebuild -archive` on a release configuration for unsigned archive validation.
5. Upload test results and content-validation reports as artifacts.

No CI step may require a network service at runtime; package resolution is the only network use and is pinned.

## 16. Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Puzzle content outgrows the data schema | Rework late in production | Prototype all nine puzzle families in Phase 1/2 before authoring 24 puzzles |
| Italian text expansion breaks layouts | Cut-off clues, unreadable UI | Pseudo-localization from the first vertical slice; no text baked into art; flexible layouts |
| Save corruption loses progress | Player trust and reviews | Atomic writes, rolling backup, migration fixtures, corruption test |
| Accessibility retrofit is expensive | Delay or incomplete compliance | Semantic puzzle model and alternative controls from the first puzzle |
| Art asset size and memory | Slow downloads, crashes on older devices | Downsampled variants, chapter-scoped loading, size budget in CI |
| Difficulty curve is unfair | Poor reviews, abandoned playthroughs | Playtest each tier; tune hints; instrument only local, opt-in session notes (no telemetry) |
| Audio puzzles exclude deaf players | Accessibility failure | Visual waveform and pattern-grid equivalents authored with the puzzle |
| Scope creep into online features | Breaks offline promise | Explicit Out of Scope list; architecture has no network layer |
| Final Ledger puzzle is obtuse | Unsatisfying ending | Prototype early; require the rule to be stated by prior evidence; hint tier 2 must explain the synthesis |

## 17. Architecture Decisions to Revisit

1. Whether the journal should use a lightweight local index for full-text search, or simple in-memory filtering over 150 entries.
2. Whether the case board is a separate package or part of `GameCore` after the prototype.
3. Whether iPad should use `NavigationSplitView` for the journal or a custom two-pane shell.
4. Whether audio should use `AVAudioEngine` for adaptive layers or simpler `AVAudioPlayer` crossfades; decide after the audio prototype.
5. Whether a debug-only save inspector should ship in TestFlight builds for beta diagnostics.

---
*Last updated: 2026-11-24 after initial architecture definition*
