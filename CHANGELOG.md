# Changelog

All notable changes to the LoL Engine package will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0-rc-5] - 2026-06-09
### Fixed
- **`link.xml` moved from `Runtime/` to the package root.** Unity only honors `link.xml` files at a package's root folder (or under the project's `Assets/`); at `Runtime/link.xml` it was silently ignored, so IL2CPP builds at Managed Stripping Level Medium/High stripped the reflection-only constructors of every plain-C# engine service. Symptom: a wall of `Cannot register null service of type I…` at boot with no exception (confirmed in a consuming project at High stripping). Projects on rc.4 or earlier must preserve the four `LoLEngine-*` assemblies in their own `Assets/link.xml`.
- **`ConfigurableServiceInitializer` now fails loudly when constructors are stripped.** `CreateServiceWithType` previously returned null without logging when `GetConstructors()` came back empty, leaving only a generic "Cannot register null service" downstream. It now logs the type, the likely linker cause, and the assembly to preserve in `link.xml`.

## [1.0.0-rc.4] - 2026-06-08
### Asset Store readiness
- **IL2CPP stripping protection**: added `Runtime/link.xml` preserving all four `LoLEngine-*` runtime assemblies (engine services are created via reflection and are invisible to the managed linker). New buyer-facing doc `Documentation~/IL2CPP.md`; `[Preserve]` added to the sample `GameServiceRegistrar`; IL2CPP/High-stripping verification gate added to `RELEASE_CHECKLIST.md` (section 7b).
- **Platform support matrix declared** in package README and documentation index: Desktop + iOS/Android supported; **WebGL not supported in 1.0** (async save/load, async obfuscation, and the file log sink use `Task.Run`, which WebGL lacks).
- **Hardened setup validation**: `ImprovedGameInitializer` now pre-flight checks that each enabled service's config asset resolves through `ResourcePathConfig` (warns with the exact Resources path attempted), validates inspector-driven custom service entries, and throws actionable errors when dependency toggles are inconsistent. The `Validate Setup` context menu runs the full report.
- **PlayMode smoke tests** (`Tests/PlayMode/SmokeTests.cs`): full engine boot with default configs + core service resolution, object pool get/return/recycle, and a save→load round-trip through the real `LocalFileSaveStore`.
- **Script-only samples labelled**: GameState, NotificationSystem, and ResourceManagement samples are now explicitly marked as script-only (no scene) in `Samples~/README.md` and the `package.json` sample descriptions, with usage instructions for each.
- **Singleton duplicate handling now logs**: `Singleton<T>` and `PersistentSingleton<T>` emit a `LoLLogger.Warning` when destroying a duplicate instance (previously silent commented-out code).
- **Docs moved out of the runtime tree**: `AudioHelpers.md` and `AAA-Audio-System-Guide.md` relocated from `Runtime/Scripts/Helpers/Audio/` to `Documentation~/` (indexed in the docs README); duplicate logger README removed.
- **Version metadata consolidated**: concrete version strings now live only in `package.json` (canonical), `LoLEngineVersion.cs`, the README badges, and `CHANGELOG.md`. All setup guides, the samples README, and the release checklist use `vX.Y.Z` placeholders or point at `package.json`, so releases no longer require a doc-wide version sweep. A verification grep was added to `RELEASE_CHECKLIST.md` section 1.

## [1.0.0-rc.3] - 2026-05-29
- Fixed documentation to add references to UPM installation and remove stale references to deleted systems and APIs.

## [1.0.0-rc.2] - 2026-05-29
### Fixed
- **Declared the missing `com.unity.modules.audio` dependency in `package.json`.** Without it, a clean UPM consumer hit 81 `CS1069` errors (AudioSource/AudioClip/AudioMixer types forwarded to `UnityEngine.AudioModule`). The dev project masked this because its manifest includes every built-in module. Verified via a clean-room batchmode import.

### Documentation
- Removed stale references to systems deleted before 1.0 (Character System, UI System, `IInputService`, `ISceneService`) from `CHEATSHEET-How-To-Use.md`, `Troubleshooting.md`, `Config.md`, `GettingStarted.md`, `Notifications.md`, and `ServiceLocator.md`.
- Rewrote the `ServiceConfiguration` checklists in the cheatsheet to match the real `enable*` toggles (added Rng, Music Playlist, Audio Orchestrator, Asset Updater).

## [1.0.0-rc.1] - 2026-05-29
### Release candidate
- First release candidate for the Unity Asset Store. API is considered stable; only bug fixes are expected before `1.0.0`.
- **Lowered minimum Unity to `6000.0`** (was `6000.4`) to support the Unity 6.0 LTS install base. Re-verify a clean compile against your minimum supported editor before final.
- Bumped version metadata across `package.json`, `LoLEngineVersion.cs`, READMEs, and documentation baselines.
- Renamed `PersistantSingleton<T>` → `PersistentSingleton<T>` (corrected spelling of a public type before the API freezes at 1.0).
- Unified all editor menu items under a single `Tools/LoL Engine/…` submenu (removed top-level menu entries and a mismatched `Tools/LoLEngine` path).
- Removed stray files from the shipping tree (`.DS_Store`, leftover `.meta` files inside `Documentation~/`).

## [0.22.6-beta] - 2026-05-06
### Asset Store readiness
- Added `Third Party Notices.md` and `Documentation~/PackageShipping.md` (shipping boundary and maintainer checklist)
- Renamed test assemblies to `LoLEngine-Tests-Edit` and `LoLEngine-Tests-PlayMode` to avoid buyer-project name collisions
- Aligned repo-root and samples README version metadata with `package.json` (0.22.6-beta, Unity 6000.4+)
- **Documentation (P2):** `Input.md` (Unity Input System — no `IInputService`), `RELEASE_CHECKLIST.md`, quickstart/samples clarify bundled vs regenerated scenes, fixed stale Now-Playing sample numbering and removed obsolete API-reference script references
- Cleanup of code to prepare the package for release to the Unity Asset Store

## [0.22.5-beta] - 2026-04-27
### Highlights
- **Structured localized text** — every localization callsite can now use `LocString` for dynamic variables and pluralization. Old `GetText(key)` calls still work but are marked `[Obsolete]` (no compile errors, just warnings).

### Changed
- Localization callsites switched to `LocString` + `Format()` API for dynamic variables and richer formatting.
- Updated localization docs with `LocString` and `DynamicVar` usage examples (see `Documentation~/Localization.md` and `LOCALIZATION_MIGRATION.md`).

### Migration
- No breaking changes. `GetText(key)` remains functional; migrate at your own pace per `LOCALIZATION_MIGRATION.md`.

## [0.22.4-beta] - 2026-04-26
### Highlights
- **Quieter default logs in production** — engine logs default to `Warning` (was `Debug`), so shipping builds aren't flooded by engine-internal messages. Game-side logs still default to `Debug`.
- **`GameLogger` for game code** — new game-facing logger so your game code can stay verbose without touching engine log levels.

### Added
- `LoLEngineConfig.EngineLogLevel` and `GameLogLevel` — independent thresholds for engine vs. game logs.
- `GameLogger` — game-facing facade using the same sink pipeline as `LoLLogger`.
- `LogSource` field on `LogEvent` — distinguishes engine-originated from game-originated entries in log history.

### Changed
- `ImprovedGameInitializer` reads both log levels from config at startup.
- File log sink now uses the same threshold rules as console and history sinks.

### Fixed
- `ImprovedDependencyChecker` honours the configured engine log level instead of forcing `Debug`.
- Project-level `Assets/Resources/...` configs now correctly override bundled package fallbacks at the same path.
- `Warning` threshold no longer accidentally allows `Emphasis` messages through (enum-ordering bug).

## [0.22.3-beta] - 2026-04-25
### Fixed
- LoLLogger correctly loads its default configuration on first use.

## [0.22.2-beta] - 2026-04-25
### Fixed
- LoLLogger now respects the Logging Level set in its configuration asset (previously it logged everything regardless of the config).

## [0.22.1-beta] - 2026-04-24
### Changed
- Bumped supported Unity version to **6.4.4f1**.

## [0.22.0-beta] - 2026-04-23
### Engine Core [0.22.0]
#### Added
- **`TypedPoolBag<TKey, TItem>`** — Genre-neutral typed pool with per-bucket shuffled draw, `IsAllowed` predicate filter, automatic cascade on exhaustion, and `PoolExhaustedException` on full exhaustion. Pass `ref Rng` to `Shuffle()`; serialize remaining items via `GetRemainingItems(key)`. ADR-013 added. New files: `Runtime/Scripts/Utility/TypedPoolBag.cs`, `Runtime/Scripts/Utility/Odds/PoolExhaustedException.cs`.
- **`AbstractOdds<TOutcome>`** — Base class for probabilistic systems with persistent state. Holds an injected `Rng` substream and a `CurrentValue` counter. `RestoreState(int)` + `ReplaceRng(Rng)` enable save/load round-trips without losing pity state. ADR-014 added. New file: `Runtime/Scripts/Utility/Odds/AbstractOdds.cs`.
- **`AntiPityOdds<T>`** — Engine-shipped implementation of `AbstractOdds<T>`. Weighted roll where the pity target's effective weight increases by `pityIncrement` per consecutive miss and resets on hit. `EffectiveWeight(i)` and `TotalEffectiveWeight()` exposed for tooltip/UI display of current odds. New file: `Runtime/Scripts/Utility/Odds/AntiPityOdds.cs`.
- Unit tests: `Tests/Edit/Utility/TypedPoolBagTests.cs` (draw, cascade, filter, exhaustion, shuffle coverage) and `Tests/Edit/Utility/AntiPityOddsTests.cs` (counter mechanics, save-load round-trip, statistical distribution sanity check).
#### Fixed (bug fixes from Phase 3 code review)
- **HIGH** — Per-domain migration/deserialization failures in `SaveSystem` now caught per-domain; the load continues with remaining domains and `LastLoadReport` reflects partial success instead of propagating an exception.
- **MEDIUM** — `InMemorySaveStore.List()` now correctly filters by extension suffix extracted from the glob pattern (`*.sav` → `.sav`); previously returned all files regardless of extension.
- **MEDIUM** — `ISaveStore` extended with `GetLastModified(string)` and `GetFileSize(string)`; `ListSavesAsync()` and `HasAnySavesAsync()` no longer call `Directory.Exists` or `new FileInfo()` directly — fully routed through the store abstraction.
- **LOW** — `SaveLoadReport.DomainResult.WasMigrated` is now a stored `bool` set only when a migration step actually ran; previously derived from version arithmetic, causing false positives when a chain was missing.
#### Changed
- **`ILocalizationService.GetText()` and `GetTextFormatted()` marked `[Obsolete]`** — Both methods now carry `[Obsolete("Prefer LocString + Format(). See Documentation~/LOCALIZATION_MIGRATION.md. Will not be hard-removed per ADR-011.")]`. The deprecation was scheduled in ADR-011 (v0.23.0-beta). Neither method is removed or functionally altered. Callers get a compiler warning; all callsites in existing projects continue to compile and run unchanged.
#### Documentation
- **ADR-015: Fluent Option Builder Pattern** — Documents the `With*` method-chain pattern used by `PlayOptions`, `PlaylistPlayOptions`, and `ResourceLoadOptions`. Explains when to prefer it over constructor overloads and when a builder class is more appropriate. Added to `ARCHITECTURE_NOTES.md`.
- **ADR-016: ExtraFields Kitchen Drawer** — Documents the `Dictionary<string, object> ExtraFields` pattern on mutable `AbstractModel` subclasses for one-off game-specific flags that don't warrant a real field. Includes save/load contract and rules for when to graduate a field out of the drawer. Added to `ARCHITECTURE_NOTES.md`.
- **ADR-017: Null-Object Pattern for Optional Scope** — Documents the `NullFoo.Instance` pattern that eliminates call-site null checks when a scope is validly absent at runtime (e.g. no active run in the main menu). Distinguishes "validly absent" from "programmer error". Added to `ARCHITECTURE_NOTES.md`.
- **ADR-018: Test-Only Escape-Hatch Naming Convention** — Formalises the `NeverEverCallThisOutsideOfTests_*` prefix convention used throughout the engine. Documents existing examples, preferred visibility (`internal` + `InternalsVisibleTo`), and compiler-level enforcement via `#if` guards. Added to `ARCHITECTURE_NOTES.md`.
- **`ARCHITECTURE_NOTES.md` summary updated** — ADR index in the summary section updated to include ADR-008 through ADR-018 (previously the summary listed only ADR-001 through ADR-007). Validation date bumped to 2026-04-23.

## [0.21.0-beta] - 2026-04-23
### Engine Core [0.21.0]
#### Added
- **`ISaveStore` — pluggable save I/O backend** — Abstracts all file reads, writes, deletes, directory listing, and quarantine operations. `LocalFileSaveStore` (default) wraps `File.*` with atomic write-via-temp; `InMemorySaveStore` is a dictionary-backed backend for integration tests (no disk, no ServiceLocator). `SaveSystem` constructor accepts optional `ISaveStore` and `saveBaseDirectory` override to make save paths test-controllable. ADR-012 added to `ARCHITECTURE_NOTES.md`. New files: `Interfaces/ISaveStore.cs`, `Service/LocalFileSaveStore.cs`, `Service/InMemorySaveStore.cs`.
- **Per-domain schema versioning (`IDomainMigration`)** — `PersistableData.SchemaVersion` (virtual, default 1) lets each domain declare its own schema version. `GameSaveData` now records `_domainSchemaVersions` per domain at save time (included in integrity checksum). On load, if stored version < current version the registered `IDomainMigration` chain runs on the raw JSON string before deserialization. Migrations chain: register one `IDomainMigration` per version step per domain (e.g. v1→v2, v2→v3). Stored version 0 (pre-domain-version save) loads as-is — no spurious migration chains when the feature is first introduced. New interface: `Interfaces/IDomainMigration.cs`. `ISaveSystem.RegisterDomainMigration(IDomainMigration)` added.
- **Corrupt-file quarantine** — On load failure, instead of silently returning `false`, `SaveSystem` moves the bad file to `{saveDir}/Quarantine/{timestamp}_{reason}_{filename}.bak`. Reason codes: `JSN` (JSON parse failure), `CHK` (checksum mismatch), `SCH` (incompatible bundle schema version). File is recoverable from quarantine; no data is destroyed. `LocalFileSaveStore.Quarantine()` implements the move; `InMemorySaveStore.Quarantine()` moves the entry to a test-inspectable list.
- **`SaveLoadReport`** — Describes the outcome of every `LoadGame` call. Exposed via `ISaveSystem.LastLoadReport`. Quarantined saves return `Quarantined(reason)` with `Success=false`. Per-domain results in `DomainResults`: `DomainId`, `Loaded`, `StoredVersion`, `CurrentVersion`, `WasMigrated`. New file: `Data/SaveLoadReport.cs`.
- **`SaveSystem` test seam** — `NeverEverCallThisOutsideOfTests_InjectSerializer(IDataSerializer)` bypasses ServiceLocator serializer resolution; pairs with `InMemorySaveStore` for fully controlled integration tests.
- Integration tests: `Tests/Edit/Core/DataPersistence/SaveSystemIntegrationTests.cs` — 6 tests covering JSN quarantine, CHK quarantine, missing-file handling, per-domain migration, report contents, pre-domain-version compatibility.

## [0.20.0-beta] - 2026-04-22
### Engine Core [0.20.0]
#### Added
- **`AbstractModel` canonical/mutable boundary** — Base class that enforces the template-vs-instance split. Canonical instances (registered in `ModelDb`) have `IsMutable = false`; calling any mutating method while canonical throws `InvalidOperationException` via `AssertMutable()`. `MutableClone()` creates a live runtime instance with `IsMutable = true`. `ClonePreservingMutability()` returns `this` for canonicals (no allocation). Engine-default `MutableClone()` uses `MemberwiseClone`; subclasses override for deep-clone. Pairs with `ModelDb` for the full content/instance lifecycle. ADR-010 added to `ARCHITECTURE_NOTES.md`. New file: `Runtime/Scripts/Runtime/Models/AbstractModel.cs`.
- **`LocString` + `DynamicVar` — structured localized text values** — `LocString` is a value object `(LocTable table, string key, DynamicVar[] vars)` that resolves via `ILocalizationService.Format(locString)`. Tokens `{varName}` / `{varName:specifier}` are substituted at resolution time using `DynamicVar.CalculatedValue`. `DynamicVar` has three layers: `BaseValue` (authoring constant), `CalculatedValue` (runtime override), `PreviewValue` (upgrade preview), plus `WasJustUpgraded` flag for UI highlights and `Upgrade(delta)` to accumulate bonuses. Unresolved tokens render as `{varName}`. Custom specifiers handled by `ILocStringFormatter` chain registered via `locService.AddLocStringFormatter(...)`. `LocTable` predefined constants: `UI`, `Common`, `Items`, `Enemies`, `Cards`; custom via `LocTable.From(name)`. Backwards compatible — existing `GetText(key)` unchanged. ADR-011 added to `ARCHITECTURE_NOTES.md`. New files: `Runtime/Scripts/Core/Localization/LocString/`.
- **`LocValidator` editor tool** — `Tools > LoLEngine > Validate Localization Tables` reports every key missing a translation in any language column. Callable as `LocValidator.ValidateAll()` from CI build scripts. New file: `Editor/Localization/LocValidator.cs`.
- **`TestMode` gate** — `TestMode.IsOn` static flag signals automated-test runs. `AssertOn()` / `AssertOff()` throw typed exceptions. Set once at boot via `TestMode.Enable()`. New file: `Runtime/Scripts/Runtime/Testing/TestMode.cs`.
- **RNG override injection (`#if UNITY_EDITOR || LOL_TESTS_ENABLED`)** — `IRngService` gains `InjectNextInt/Float/Bool(streamIndex, forcedValue)`, `ClearOverrides(streamIndex)`, `ClearAllOverrides()`. Each override fires once then auto-clears. Zero production overhead (preprocessor-gated).
- `ILocalizationService` extended with `Format(LocString)` and `AddLocStringFormatter(ILocStringFormatter)`.
- `Documentation~/LOCALIZATION_MIGRATION.md` — before/after migration examples for all scenarios.

#### Deprecated (not yet removed)
- `GetText(string key)` for strings with variable substitution — prefer `LocString + Format()`. Deprecation `[Obsolete]` planned for v0.23.0-beta per ADR-011 timeline.

## [0.19.0-beta] - 2026-04-22
### Engine Core [0.19.0]
#### Added
- **Deterministic RNG service (`IRngService`)** — SplitMix64-backed named stream system. Projects define streams via `IRngStreamSet` (enum-shaped); the service vends one independent `Rng` per stream, all seeded from a single root seed with zero inter-stream correlation. `Rng` is a value-type struct with `.Int()`, `.Float()`, `.Bool()`, `.Weighted<T>()`, `.Shuffle<T>()`, `.Pick<T>()`, `.AdvanceBy()`, `Snapshot()`/`Restore()`. `RngServiceSnapshot` is fully serializable for mid-session save/load. Enabled via `ServiceConfiguration.enableRngService` (default `true`). ADR-008 added to `ARCHITECTURE_NOTES.md`. New files: `Runtime/Scripts/Core/Rng/Rng.cs`, `IRngService.cs`, `IRngStreamSet.cs`, `RngService.cs`, `RngServiceSnapshot.cs`.
- **`ModelDb` — string-keyed content registry** — Static registry mapping `ModelId → canonical object`. `ModelId` is a two-part `(Category, Entry)` struct (`"Enemy/goblin"`) that is stable across serialization. Registration is explicit (`ModelDb.Register<T>(id, instance)`) — no reflection at runtime, so IL2CPP/AOT builds are safe. `GetByIdOrNull<T>` returns null on miss; `Require<T>` throws. `GetAll<T>` enumerates by type. Domain reloads clear the registry via `[RuntimeInitializeOnLoadMethod]`. `RegisterInModelDbAttribute` is a documentation marker only (no auto-wiring). ADR-009 added to `ARCHITECTURE_NOTES.md`. New files: `Runtime/Scripts/Runtime/Models/ModelDb.cs`, `ModelId.cs`, `RegisterInModelDbAttribute.cs`.

## [0.18.2-beta] - 2026-04-18
### Engine Core [0.18.2]
#### Added
- **Gapless crossfade via look-ahead preload + early-start** — `MusicPlaylistService` now spins up a hidden `MusicPlaylistPump` MonoBehaviour during `LateInitialize()` that calls a per-frame `Tick()`. While the current track plays, `Tick()` warms the next clip in the resource cache via `IResourceService.LoadAsync<AudioClip>` once `remaining <= defaultPreloadLookaheadSeconds` (default 3 s). When `remaining <= crossfadeDuration` and the preload is ready, `AdvanceBy` fires early so both clips genuinely overlap. If the preload has not finished, `OnCurrentTrackComplete` fires the advance as before (backward-compatible fallback, no regression).
- **Equal-power crossfade curve** — New `FadeCurve` enum (`Linear`, `EqualPower`) in `LoLEngine.Core.Audio.Data`. `AudioFadeController` gains `FadeIn(handle, duration, targetVolume, FadeCurve)` and `FadeOut(handle, duration, stopAfterFade, FadeCurve)` overloads; existing parameterless overloads delegate to `Linear`. `IAudioService` and `AudioService` expose matching overloads. `MusicPlaylistService` uses `EqualPower` for playlist transitions; `AudioConfig.defaultPlaylistCrossfadeCurve` (default `EqualPower`) lets projects switch back to `Linear` per-config.
- **Per-track gain** — `PlaylistTrackEntry` (new serializable class in `LoLEngine.Core.Audio.Data`) carries `addressableId`, `gainDb` (0 = neutral, recommended −12 to +6 dB), `displayTitle`, `artist`, and `coverArt (Sprite)`. `MusicPlaylist` gains a `List<PlaylistTrackEntry> tracks` field and migrates the legacy `List<string> addressableIds` field via `OnValidate`. When starting a track, `gainDb` is converted to a linear multiplier (`Mathf.Pow(10, gainDb/20)`) and applied to `PlayOptions.volume` ahead of the Music track volume.
- **Now-Playing metadata + event** — `IMusicPlaylistService` gains `PlaylistTrackEntry CurrentTrackEntry`, `event Action<PlaylistTrackEntry> OnTrackEntryChanged`, and a new `Play(IReadOnlyList<PlaylistTrackEntry>)` overload for runtime playlists with metadata. The existing `OnTrackChanged(string)` event is unaffected. Sample `Samples~/Scripts/11_NowPlayingSample.cs` binds `TMP_Text` and `UnityEngine.UI.Image` fields to the event.
- **`PlaylistPlayOptions.preloadLookaheadSeconds`** — per-call override for the look-ahead window; `0` uses `AudioConfig.defaultPreloadLookaheadSeconds`.
- **`AudioConfig.defaultPreloadLookaheadSeconds`** (default `3f`) and **`AudioConfig.defaultPlaylistCrossfadeCurve`** (default `EqualPower`) — new inspector fields on the Music Playlist header.
- Five new PlayMode tests in `MusicPlaylistServiceTests`: `EarlyStartCrossfade_NextStartsBeforeCurrentEnds`, `PreloadFallback_UsesOnCompletePath_WhenCrossfadeIsZero`, `PerTrackGain_AppliedToHandle`, `Shuffle_CoversEveryTrackOncePerCycle`, `OnTrackEntryChanged_FiresWithMetadata`.

## [0.18.1-beta] - 2026-04-18
### Engine Core [0.18.1]
- Asynch support for Playlist audio

## [0.18.0-beta] - 2026-04-17
### Engine Core [0.18.0]
#### Added
- **`IMusicPlaylistService`** — new first-class service that orchestrates back-to-back music playback on top of `IAudioService`. Hand it a `MusicPlaylist` ScriptableObject (or a runtime `IReadOnlyList<string>` of addressable IDs) and it handles auto-advance via `IAudioHandle.OnComplete`, shuffle (Fisher-Yates with a guard against repeating the just-played track on reshuffle), crossfade (overlapping `FadeIn`/`FadeOut` on the Music track — requires `maxConcurrentSounds >= 2`), and themed playlist swaps via `SwitchTo`. Mute semantics are exposed two ways: `MuteMode.PauseOnMute` (default) preserves playback position via `PauseTrack`/`ResumeTrack`, while `MuteMode.SilenceOnly` keeps tracks advancing inaudibly via `SetTrackMute`; the settings UI binds to a single `IsMuted` toggle regardless of mode. `GlobalCrossfadeEnabled` is a settings-bindable kill-switch that short-circuits crossfade to instant cut. New types in `LoLEngine.Core.Audio`: `IMusicPlaylistService`, `MusicPlaylistService` (plain C# class, not MonoBehaviour), `MusicPlaylist` (`[CreateAssetMenu]`), `PlaylistPlayOptions`, `PlaylistPlaybackMode { Sequential, Shuffle, RepeatOne }`, `MuteMode { PauseOnMute, SilenceOnly }`, and `PlaylistEvents.{PlaylistStarted, PlaylistTrackChanged, PlaylistStopped, PlaylistSwitched, PlaylistCompleted}`. `AudioConfig` gained `defaultCrossfadeDuration`, `defaultPlaybackMode`, `globalCrossfadeEnabledDefault`, and `defaultMuteMode`. `ServiceConfiguration` gained `enableMusicPlaylistService` (defaults to `true`; requires Audio Service). The service resolves `IAudioService` in `LateInitialize()` per ADR-007 and warns if the Music track's `maxConcurrentSounds < 2` (crossfade falls back to instant cut in that case). Sample in `Samples~/Scripts/10_MusicPlaylistSample.cs` and PlayMode tests in `Tests/PlayMode/Core/Audio/MusicPlaylistServiceTests.cs`.

## [0.17.8-beta] - 2026-04-14
### Engine Core [0.17.6] 
#### Added
- ObserveFault(this Task task, string context) as a sibling helper to the randomization extensions

## [0.17.7-beta] - 2026-04-14
### Engine Core [0.17.5]
#### Added
- **Audio randomization helpers** for per-call pitch/volume jitter and pool-based variant selection. New types in `LoLEngine.Core.Audio.Data`: `PlayRandomization` (serializable struct with min/max pitch and volume ranges plus an `avoidImmediateRepeat` flag, and `Default` / `None` presets) and `RandomAudioPool` (stateful picker that remembers the last index so successive picks can avoid immediate repeats). New extension methods in `LoLEngine.Core.Audio.Extensions.AudioExtensions`: `PlayOptions.WithRandomization(in PlayRandomization)` returns a copy of options with re-rolled pitch/volume; `IAudioService.PlayRandomAsync(RandomAudioPool, PlayOptions, PlayRandomization)` and `IAudioService.PlayRandomAsync(string, PlayOptions, PlayRandomization)` pick (optionally) and play with jitter via the existing `PlaySoundAsync` path. `IAudioService` and `AudioService` are unchanged — the feature composes from existing primitives. Intended for UI click variation, footstep variants, and similar "same action repeated rapidly should not sound identical" cases that previously required per-game boilerplate. Sample updated in `Samples~/Scripts/06_AudioSystemSample.cs` with a `Play Randomized UI Click` context menu.

## [0.17.6-beta] - 2026-04-13
### Engine Core [0.17.4]
#### Fixed
- **`UnityPrivateFieldContractResolver` now actually serializes `[SerializeField]` private fields**: the resolver's only override was `CreateProperty`, which Newtonsoft only invokes for members that `GetSerializableMembers` has already decided to include — and by default that list does not contain private fields with just `[SerializeField]`. The override was silent dead code. As a result, `GameSaveData.checksum`, `gameName`, `gameVersion`, and `dataVersion` were never written to save files, so on every load `checksum` deserialized to `null` and `SaveSystem.LoadGameInternal` logged `"Save file {name} failed integrity check. Data may be corrupted."` on every load — confirmed by decoding `Tangrams-Masterpieces/Saves/save1.osav` and observing that only `_timestamp` and `_serializedData` (the two fields marked `[JsonProperty]`) were present in the JSON. The resolver now overrides `GetSerializableMembers` as well, walking the type hierarchy with `BindingFlags.DeclaredOnly` so inherited private `[SerializeField]` fields are included too. Added `JsonSerializer_PrivateSerializeFieldFields_ShouldRoundTrip` regression test that asserts private field names appear in the JSON and round-trip cleanly — the previous test suite only covered `[SerializeField] public` fields, which is why the bug went unnoticed. Existing save files written before this fix will emit the integrity warning once on first load (load still proceeds) and will be rewritten with the full field set on the next save.

## [0.17.5-beta] - 2026-04-13
### Engine Core [0.17.3]
#### Fixed
- **`GameEvent` static facade now wired at startup**: `ConfigurableServiceInitializer.InitializeCoreServicesAsync()` now calls `GameEvent.Initialize(eventManager)` immediately after creating `EventService`, before any feature service (SaveSystem, AutoSaveService, etc.) comes up. Previously, callers routing through `GameEvent.Trigger<T>` / `Subscribe<T>` / `Unsubscribe<T>` — including `SaveSystem` via `SaveEvents.OnSaveBegin/OnSaveComplete` — threw `InvalidOperationException: EventManager not initialized. Call Initialize first.` on every save, even with `EnableEventManagerService` enabled. The only workaround was a manual `GameEvent.Initialize(...)` from the game initializer. `GameEvent.Initialize(...)` was also changed to overwrite unconditionally instead of warn-and-skip, so edit-mode test runs that re-boot the engine without a domain reload don't hold a stale reference to a disposed `EventService`.

## [0.17.4-beta] - 2026-04-10

- Upgraded Unity to Version 6.4.2f1

## [0.17.3-beta] - 2026-04-10
### Engine Core [0.17.2]
#### P0 - URGENT Fixes and Improvements
- Hardening SaveSystem against registry mutatuin during save/load
- Guaranteed TimeService ticking at runtime
- Aligned docs with actual toggles/runtime behavior

#### P1 Fixes
- QuickSave dependency validation
- Autosave startup fix

#### P2 Fixes
- Notification service diagnostics + hygiene
- Resource cache entry cap configurability

## [0.17.2-beta] - 2026-04-02 
- Upgraded Unity to Version 6.4.1f1

## [0.17.1-beta] - 2026-03-28
### Engine Core [0.17.1]

#### Fixed — Tier 1 Correctness (H-025 through H-028)
- **H-025 — Task.Delay eliminated from all runtime code**: `AudioOrchestratorService.PreloadResourceAsync` timeout path now uses `MainThreadDelay` (Task.Yield loop) instead of `Task.Delay`. `AdvancedAudioPlayer` (3 occurrences: load timeout, fade wait, retry delay) and `AssetUpdaterService` (progress polling) updated identically. Zero thread-pool timer usage remains in runtime code — all async delays stay on the Unity main thread.
- **H-026 — Hardcoded config path decoupled**: `LoLEngineConstants` and `ImprovedDependencyChecker` both hardcoded `Resources.Load("Configs/DefaultLoLEngineConfig")`. Both now try to resolve the path via `ResourcePathConfig` first (respecting project-specific layout), falling back to the prior hardcoded path for backward compatibility. `ImprovedDependencyChecker` also applies the same treatment to `DefaultSaveConfig`.
- **H-027 — Notification auto-dismiss via TimeService**: `NotificationService.ScheduleAutoDismiss()` now uses `ITimeService.Scheduler.Schedule()` when TimeService is available (resolved in `LateInitialize()`), eliminating the hidden `NotificationHelper` MonoBehaviour dependency. The coroutine fallback remains for contexts where TimeService is not registered. Manual `DismissNotification()` cancels any pending auto-dismiss task via tracked `_autoDismissTaskIds`.
- **H-028 — GameState re-entrant transition guard**: `GameStateManagerService.ChangeState()` previously had no guard against re-entrant calls from `Enter()`, `Exit()`, or event handlers, risking stack overflows and undefined state. Added `_isTransitioning` bool flag and `_pendingTransitions` queue. Re-entrant calls are queued; the outermost transition drains the queue on completion. `try/finally` guarantees the flag is always cleared.

#### Fixed — Tier 2 Hardening (H-029 through H-030)
- **H-029 — Logger bounded queue and crash-safe flush**: `FileSink` was backed by an unbounded `ConcurrentQueue` that could OOM under log storms. Added `LoggerConfig.FileMaxQueueSize` (default 10,000) — when the queue is full, `Emit()` increments `DroppedCount` and returns instead of enqueueing. Added crash-safe flush hooks: `AppDomain.CurrentDomain.ProcessExit` and `AppDomain.CurrentDomain.UnhandledException` both flush sinks on abnormal exit. `Shutdown()` unhooks all handlers to prevent double-flush. `FileSink.DroppedCount` is a public diagnostic property.
- **H-030 — Service registrar conflict detection**: `RunExternalRegistrars()` now wraps `IServiceLocator` in a `RegistrationTracker` proxy for each registrar. The tracker records which registrar registered each service type; if a second registrar registers the same type, `LoLLogger.Error` is emitted identifying both the incumbent and the challenger. The overwrite still proceeds to preserve flexibility, but silent service replacement is now an observable error.

#### Changed
- **`ServiceConfiguration`**: `NotificationService.LateInitialize()` now resolves `ITimeService` for scheduler-based dismiss (no API change; behavior improves when TimeService is registered).
- **`GameStateManagerService`**: `ChangeState(GameStateType)` and `ChangeState(IGameState)` delegate to new private `ChangeStateInternal()`. Public API unchanged.
---

## [0.17.0-beta] - 2026-03-27
- Upgraded Unity to version 6.4
- Newtonsoft added as a package manager dependency

## [0.16.4-beta] - 2026-03-15
### Engine Core [0.16.3]
#### Added
- **Service Extensibility**: `IServiceRegistrar` auto-discovery — implement the interface in your game assembly and engine picks it up automatically during init
- **LateInitialize lifecycle hook**: `ILoLEngineService.LateInitialize()` called after ALL services are registered, safe for cross-dependency resolution
- **ServiceRegistrationInfo validation**: `Validate()` checks both interface and implementation types resolve correctly at runtime
- **Sample**: `10_GameServiceRegistrarSample.cs` showing WalletService registration pattern

#### Fixed
- **Custom service registration bug**: Services registered via `ServiceRegistrationInfo` were stored under `typeof(object)` making them unretrievable via `Get<IMyService>()`. Now registers under the specified interface type.

#### Changed
- **`IServiceRegistrar`**: Parameter typed as `ServiceConfiguration` instead of `object`, added `Priority` property for ordering
- **`ServiceRegistrationInfo`**: Split `assemblyQualifiedTypeName` into `interfaceTypeName` + `implementationTypeName` (old property marked `[Obsolete]`)
- **`CreateCustomService`**: Now uses `CreateServiceWithType()` for proper MonoBehaviour lifecycle support

## [0.16.3-beta] - 2026-03-14
- Upgraded Unity to Version 6.3.11f1

## [0.16.2-beta] - 2026-02-23
### Engine Core [0.16.2]
- ResourcePathConfig path mismatch — the wizard now derives engineResourceRoot and configsPath from the actual output directory. For the default Assets/Resources/Configs/, this sets
  engineResourceRoot = "" and configsPath = "Configs/", so Resources.Load("Configs/DefaultAudioConfig") correctly resolves to Assets/Resources/Configs/DefaultAudioConfig.asset

## [0.16.1-beta] - 2026-02-23
### Engine Core [0.16.1]
Added Setup Wizard to the Editor to Make Setup of the LoLEngine Easier.

## [0.15.15-beta] - 2026-02-23
### Engine Core [0.15.42]
Fixed AudioSource Memory Leak

## [0.15.14-beta] - 2026-02-23
### Engine Core [0.15.41]
Updated from Debug.* to LoLLogger

## [0.15.13-beta] - 2026-02-22
### Engine Core [0.15.40]
Both APIs work now:
- Old way: settings.musicVolume = 0.8f
- New way (data-driven): settings.SetTrackVolume("Music", 0.8f) or settings.SetTrackVolume("MyCustomTrack", 0.5f)

## [0.15.12-beta] - 2026-02-22
### Engine Core [0.15.39]
- Fixed Configuration paths
- Track volume sync uses hardcoded string matching
- AudioSettings bypass the save system 
- Fixed Save System with respect to Obfuscation, encryption

## [0.15.11-beta] - 2026-02-22
### Engine Core [0.15.38]

**AudioService.cs — CleanupAllHandles():**
- Changed List<IAudioHandle> to HashSet<IAudioHandle> so handles that exist in multiple collections (e.g. a persistent sound in both _activeHandles and _persistentHandles) are only cleaned up    
  once. This eliminates the double-return to ObjectPool that caused the warning spam.
**ImprovedGameInitializer.cs — removed all dead commented-out code:**
- Line 25: removed //[SerializeField] private bool createServiceManager = true;
- Line 36: removed // private bool _isShuttingDown; (the old bool comment above the int)
- Line 79: removed //if (_instance == this && !_isShuttingDown)
- Line 138: removed //yield return new WaitUntil(() => task.IsCompleted);
- Lines 187-194: removed the 3 commented-out lines (//if, //return, //_isShuttingDown = true;) and the redundant _isShuttingDown = 1; assignment after Interlocked.Exchange already set it
- Line 217: removed //_isShuttingDown = false;

## [0.15.10-beta] - 2026-02-20
- Upgraded Unity to Version 6.3.9f1

## [0.15.9-beta] - hotfix - 2026-02-20
### Engine Core [0.15.37]
#### Audio Service
##### Fixed
- **AudioConfig.defaultVolume ignored on first run**: `LoadSettings()` was creating `new AudioSettings()` (hardcoded `1f` for all tracks) when no PlayerPrefs existed, then `ApplySettings()` overwrote the correct track volumes already set by `CreateDefaultTracks()`. First-run settings are now seeded from `AudioConfig.trackConfigs[].defaultVolume` via the new `CreateSettingsFromConfig()` helper. When `AudioConfig` is unavailable, behavior falls back to `1f` as before.
- **Player volume changes not persisted to PlayerPrefs**: `SetTrackVolume()` and `SetTrackMute()` updated the in-memory `AudioTrack` objects and the AudioMixer but never wrote back to `_currentSettings`. `SaveSettings()` serializes `_currentSettings`, so every player-changed volume was silently discarded — subsequent sessions always loaded startup values. Both methods now call `SyncTrackVolumeToSettings()` / `SyncTrackMuteToSettings()` to keep `_currentSettings` in sync before the auto-save or shutdown persists it.

## [0.15.8-beta] - 2026-02-13
### Engine Core [0.15.35]
#### JsonDataSerializer:
- Removed the using CompressionService import — no more silently creating a fallback
#### SaveSystem:
- Both compressionService variables in SaveSystem were dead code — fetched but never used anywhere in the method bodies
- Corrected DependencyChecker Save Config Parameters
#### LocalizationService:
- Swapped Get<LocalizedFontConfig>() → TryGet(out _fontConfig) — no warning, the existing fallback logic on lines 74-81 handles the rest

## [0.15.7-beta] - 2026-02-12
- Upgraded unity to version 6.3.8f1

## [0.15.6-beta] - 2026-02-12
### Engine Core [0.15.31]
### Fixed — Performance
- **EventService.TriggerEvent per-trigger allocation**: Replaced `list.ToArray()` (allocated a new array on every event trigger) with a reusable static dispatch buffer using `List.CopyTo()`. Zero allocations in steady state; buffer only grows when subscriber count exceeds previous peak.

## [0.15.5-beta] - 2026-02-12
### Engine Core [0.15.30]
### Fixed — Design Violations (H-007, H-009, H-010, H-011, H-013)
- **H-007 — IServiceInitializer contract**: `IServiceInitializer` now declares `Task InitializeServicesAsync()` + `Shutdown()`. Removed two throw-only methods from `ConfigurableServiceInitializer`. Deleted dead `EngineConfiguration` class. Removed dead private `InitializeServices()` from `ImprovedGameInitializer`.
- **H-009 — AutoSaveService excluded scenes**: Hardcoded scene list moved to `SaveConfig.excludedSceneNames` (inspector-configurable `string[]`). Engine defaults: MainMenu/Boot/Loading/Splash/Settings/Credits. Game-specific names removed from engine defaults.
- **H-010 — GameStateType game-specific values**: Enum trimmed to None=0, Bootstrap=1, Loading=2. Game projects extend via `const GameStateType X = (GameStateType)100+`. Added `Samples~/GameState/SampleGameStateType.cs` demonstrating the pattern.
- **H-011 — LoLEngineConfig encapsulation**: All public mutable fields → `[SerializeField] private` with PascalCase read-only properties. `defaultCharacterGravityScale` (game-specific) deleted. Asset serialization backward-compat preserved.
- **H-013 — SaveConfig hardcoded resource load**: `SaveConfig.ToRuntimeSettings()` accepts `LoLEngineConfig` as injected parameter. `ConfigurableServiceInitializer` retrieves registered config from `ServiceLocator` and passes it in.

## [0.15.4-beta] - 2026-02-12
### Removed Battle Engine [0.2.0]
- **Battle Engine (BattleEngine.asmdef)**: Entire VoidRiders-specific combat assembly removed (~42 files). Included `Dice`, `Abilities`, `Pressure`, `Signatures`, `Resonance`, `CombatLog`, `TacticalSystems`, `PowerSources`, `CombatStyle`, `Resolution`, `FocusSystem`, and `CombatSimulator`. This was a game-specific TTRPG combat system that had no place in a generic engine — it was never integrated as an engine service.
- **`CombatAudioConfig.cs`**: Already-obsoleted ScriptableObject removed with the Battle Engine.
- **`Editor/Combat/DifficultyPresetCreator.cs`**: Editor tool for battle engine difficulty presets removed.
- **VoidRiders combat documentation**: `ResonanceCombatSystem.cs`, `VoidRiders_Combat_Packet_v2.pdf`, `VoidRiders_Combat_Packet_v2_Resolve.pdf`, `VoidRiders_BattleSystem_Part1_LightShadow.md`, `VoidRiders_BalanceGrid_Resolve_L1-10.csv`, `VoidRiders_QuickCard_Resolve_v2.pdf`, `VoidRider_Implementation_Part4_Combat.md` removed.
- **Deprecated combat docs**: `CombatEnhancements.md`, `Combat-VFX-Integration.md`, `CombatSystemAnalysis.md`, `CombatExample.cs` removed.

## [0.15.3-beta] - 2026-02-10
### Engine Core [0.15.21]
### Dead Feature Fixes (H-001 through H-008)
- **TimeChannel / Scheduler (H-001 CRITICAL)**: `TimeChannel._accumulatedTime` was never advanced because `TimeChannel.Update()` was never called at runtime. `TimeService.UpdateAllTimers()` now updates all registered channels each frame before ticking timers — `channel.Time` (used by `Scheduler` for delay/periodic tasks) now correctly reflects elapsed engine time.
- **TimeChannel.SetTimeScale smooth transition (H-002)**: Smooth time-scale transitions (`duration > 0`) created a `new Timer()` directly, which was never tracked by `TimeService._timers` and therefore never updated. Timer is now created via `_timeService.CreateTimer()` (registered and ticked automatically) and uses the "Unscaled" channel for real-time speed, matching the behavior of `TimeService.SetTimeScale`.
- **QuickSaveService.RefreshQuickSaveList (H-003)**: Stub replaced with a real async implementation using `ISaveSystem.ListSavesAsync()`. Scans for slots prefixed `quicksave_`, orders newest-first, respects `MaxQuickSaves`. Fire-and-forget from `Initialize()` so startup is non-blocking. Oldest slot deletion now calls `ISaveSystem.DeleteAsync()` instead of leaving a dangling TODO.
- **ResourcePool.PooledCount (H-005)**: Always returned `0`. `ResourcePool` now tracks all pool IDs it has created and aggregates `InactiveCount` from `IObjectPoolService.GetPoolStats()` across them. `ResourceStats.PooledInstanceCount` reflects actual inactive pooled objects.
- **AssetBundleLoader.Release(string) (H-006)**: Returned `true` without doing anything. Now tracks per-bundle asset reference counts (incremented on successful load, decremented on release). When the count for a bundle reaches zero, `UnloadBundle(bundleName, false)` is called — the bundle is unloaded while in-use instantiated objects remain alive.
- **ResourceService memory budget enforcement (H-004)**: `StartMemoryMonitoring()` was an empty stub; `maxMemoryBudgetMB` config had no effect. Empty method removed. Budget is now enforced inline in `CacheAsset()` via `EnforceMemoryBudget()`, which evicts the oldest non-pinned cache entries after each new asset load until usage is under the configured budget. No periodic monitor needed — enforcement happens at the point of pressure.
- **IScheduler.ScheduleCron (H-008)**: Removed silent stub that returned `Guid.Empty` from both `IScheduler` interface and `Scheduler` implementation. Callers will now get a compile-time error rather than a silently useless handle.

## [0.15.2-beta]
### Engine Core [0.15.20]
### Localization System Overhaul

#### Fixed
- **LocalizationConfig**: Replaced unserializable `Dictionary<SystemLanguage, SystemLanguage>` fallback field with serializable `List<LanguageFallback>` structs — fallback chains now actually work in the Inspector and at runtime
- **LocalizedText / LocalizedImage**: Added `ServiceAwaiter` retry pattern — components no longer silently fail if enabled before service initialization
- **LocalizationService**: String cache now cleared on language switch — prevents stale LRU entries from wasting capacity
- **CSVStringProvider**: Header line now parsed with `SplitCSV()` instead of naive `Split(',')` — consistent with data row parsing

#### Added
- **Font Localization (Phase 1)**
  - `LocalizedFontConfig` ScriptableObject — per-language font mapping with TMP, legacy font, fallback chains, and size scale
  - `LocalizedFontApplier` component — standalone font switching for dynamic text (chat, logs)
  - `LocalizedFontConfigEditor` — custom inspector with validation (missing defaults, duplicate languages, zero scale)
  - `LocalizedText` now automatically applies per-language font and size scale on language switch
  - `LocalizationConfig` gains optional `fontConfig` reference field
  - `ILocalizationService` — new methods: `GetFontForCurrentLanguage()`, `GetLegacyFontForCurrentLanguage()`, `GetFontSizeScale()`, `FontConfig` property
- **Case Transforms (Phase 2)**
  - `TextTransform` enum: `None`, `UpperCase`, `LowerCase`, `UpperFirst`, `TitleCase`
  - `LocalizedText` applies transform after text lookup using `CultureInfo` (handles Turkish İ/i)
- **Plural Rules (Phase 2)**
  - `PluralRules.Resolve(SystemLanguage, int)` — CLDR plural categories for 30+ languages
  - 6 categories: `Zero`, `One`, `Two`, `Few`, `Many`, `Other`
  - `GetPluralText(key, count)` on `ILocalizationService` — looks up `key[category]`, falls back to bare key
  - `"KEY".LocalizePlural(count)` extension method
- **Per-Language Memory Management (Phase 3)**
  - `CSVStringProvider` now loads per-language files first (`StringTable_English.csv`), falls back to monolithic CSV
  - `UnloadLanguage()` on `IStringProvider` — removes dictionary and loaded flag to free memory
  - `LocalizationService` automatically unloads previous non-default language on switch
  - `Resources.UnloadAsset()` called after CSV parsing to free native TextAsset memory
  - `LanguageSplitter` editor tool (menu: LoLEngine > Localization > Split CSV by Language)
  - `LanguageSplitBuildPreprocessor` — auto-splits CSV before every build via `IPreprocessBuildWithReport`
- **Localized Audio (Phase 4)**
  - `LocalizedAudio` component — loads `GetLocalizedAsset<AudioClip>()` on language change, assigns to `AudioSource`
  - `SetLocalizedAudio(this AudioSource, string key)` extension method
- **RTL Support (Phase 5)**
  - `IsCurrentLanguageRTL` property on `ILocalizationService` (Arabic, Hebrew)
  - `LocalizedText` auto-sets `isRightToLeftText` on TMP and flips alignment (Left↔Right) for RTL languages
  - `LocalizedLayoutGroup` component — flips `HorizontalLayoutGroup.reverseArrangement` and child alignment for RTL
- **Tests**: 30+ new EditMode tests covering fallbacks, cache invalidation, plural rules, font config, memory unloading, RTL alignment flipping

## [0.15.1-beta]
### Engine Core [0.15.11]
#### Fixed
- Updated Category for PlaySfx options to be a non Constant so they can be overriden.

## [0.15.0-beta]
### Engine Core [0.15.10]
#### Changed
- **LoLLogger**: Rewritten as static facade over sink pipeline (ADR-006)
  - `UnityConsoleSink`: Rich-text formatting with level-based colors, frame count, timestamps
  - `FileSink`: Asynchronous batched file writes on background thread (was synchronous per-call)
  - `HistorySink`: O(1) ring buffer replacing O(n) List with RemoveAt(0)
  - `LoggerConfig`: Centralized configuration replacing PlayerPrefs-backed scattered fields
  - Converted from `MonoBehaviour` to `static class` (zero MonoBehaviour usage existed)
  - `LogLevel` enum extracted to its own file (`LogLevel.cs`)
  - `LogEvent` struct extended with `FrameCount`, `GameTime`, `CallerClassName` for sink formatting
- **LoLLogger**: Replaced `new StackTrace()` per log call with zero-cost `[CallerMemberName]`, `[CallerFilePath]`, `[CallerLineNumber]` compiler attributes on all 12 public logging methods
- **LoLLogger**: Added `[Conditional("UNITY_EDITOR"), Conditional("DEVELOPMENT_BUILD")]` to `Log`, `DebugLog`, and `LogEmphasis` methods (6 overloads) — entire call site (including argument evaluation) is stripped in release builds. `Warning` and `Error` remain unconditional.

#### Fixed
- **LoLLogger**: Channel filtering now functional — `LogChannel` overloads check bitmask before dispatch (was non-functional: channels were string-prepended, never filtered)
- **LoLLogger**: `LOGChannels` property no longer shares `_logLevelValueIsSet` flag with `LOGLevel` (shared flag caused whichever was accessed second to return uninitialized defaults)
- **LoLLogger**: `IsMainThread()` now always correct — initialized via `[RuntimeInitializeOnLoadMethod(SubsystemRegistration)]` instead of first-call capture (which could be wrong if first call was off main thread)
- **LoLLogger**: Removed broken `CallerCache` (key `threadId * 100000 + stackFrameCount` systematically mislabeled callers at same stack depth)
- **LoLLogger**: Eliminated `new StackTrace()` per log call — replaced with `[CallerMemberName]` / `[CallerFilePath]` / `[CallerLineNumber]` (zero runtime cost, compiler-injected)
- Ensured **Ambient** is distinct from **Music** in the Audio Manager 
- **VectorExtensions**: Fixed Debug Spamming
**ServiceLocator Fast Cache Race Condition**
- ClearAllServices(): Moved _fastCache.Clear() inside both write locks, after dictionaries are cleared. No window for stale cache re-population.
- ClearServices(): Added missing _fastCache.Clear() inside the write lock — was previously leaving stale cached entries for cleared services.
**Event Pooling Bugs**
- Non-generic ReleaseEvent(GameEvent): Added MAX_POOL_SIZE check (was missing, pool grew unboundedly)
- Trigger<T>(string, Dictionary): Now copies the dictionary instead of assigning by reference — ReleaseEvent calling .Clear() no longer destroys the caller's data
- Added null-safety on Parameters?.Clear() in both ReleaseEvent overloads
  Fix #2: SaveSystem sync deadlock (SaveSystem.cs)
  Was: Task.Run(async () => await SaveGameAsync()).GetAwaiter().GetResult() — blocks main thread, callbacks fire on thread pool, deadlock risk if Unity's SynchronizationContext leaks.
  Now: Truly synchronous implementations using File.WriteAllText/File.ReadAllText. No Task.Run, no await, no deadlock. Events and progress callbacks fire on the calling thread as expected.
**ObjectPool split-lock (ObjectPool.cs)**
Lock 1 atomically checks AND removes from _active. Second concurrent Return(obj) fails the Contains check and bails. Still two lock acquisitions (needed to keep Unity API calls outside the lock), but the race condition is eliminated.
**NotificationService thread safety (NotificationService.cs)**
- Subscriber lists locked in Subscribe/Unsubscribe/Send (lock-per-list pattern)
- Send takes a locked snapshot before iterating
- _notificationHistory protected by _historyLock
**SaveSystem sync deadlock (SaveSystem.cs)**
Truly synchronous implementations using File.WriteAllText/File.ReadAllText. No Task.Run, no await, no deadlock. Events and progress callbacks fire on the calling thread as expected.
**ServiceLocator**
- Fixed ServiceLocator inconsistent lock timeouts

#### Added
- **LoLLogger**: `GetLogHistory()` — public API replacing reflection access to private `_logHistory`
- **LoLLogger**: `ClearLogHistory()` — public API replacing reflection invocation of private method
- **Documentation**: `Documentation~/Logger.md` — user guide for sink-based logging
- **Architecture**: ADR-006 in `ARCHITECTURE_NOTES.md` — Sink-Based Logger Architecture

#### Cleanup and Optimization
- **AudioServices**: Cleanup of the LoLLogger and the inclusion of DEBUG Specific BLOCKS to prevent the code getting to Runtime Builds
- **Compression**: Cleanup of LoLLogger
- **Config**: Cleanup of LoLLogger
- **DependencyChecker**: Optimization + LoLLogger Cleanup
- **Events**: Cleanup of LoLLogger
- **Game Initializer**: Optimization + LoLLogger Cleanup

#### Removed
- **Input Service**: Deleted entirely — game-specific (InputConstants with hardcoded Player actions, UI components, handler classes). Removed from ServiceConfiguration, ConfigurableServiceInitializer, ResourcePathConfig, BaseGameState, ImprovedDependencyChecker, tests, samples, and .asset files.

#### DEPRECATED
- **FootstepAudio**
- **SurfaceAudio**
- **CombatAudio**
- **Extensions** : TimeExtensions, MathExtensions, VectorExtensions, GeometryExtensions, StringExtensions.

## [0.14.1-beta] - 2026-02-05
- Upgraded Unity to version 6.3.7f1

## [0.14.0-beta] - 2026-02-05
### Engine Core [0.14.0]
### Removed — Mass Deprecation of Game-Specific Systems
Removed 19 game-specific/wrapper systems from Core, the entire GameObjects assembly,
related events, persistence data, samples, tests, and documentation to keep the engine
focused on reusable infrastructure.

**Tier 1 — Game-Specific Systems (19 Core directories removed):**
- **Interaction** — Trigger system, interactables, interaction manager
- **Dialogue** — Dialogue service, data, conditions, actions
- **Quests** — Quest service, data, tracker, rewards
- **Combat** (Core) — Status effects, damage types, combat modifiers (Battle Engine in Runtime kept)
- **Achievements** — Achievement service, tracker, definitions
- **Currency** — Currency service
- **Relationships** — Reputation service, faction data
- **Tutorials** — Tutorial service, steps, sequences
- **Items** — Inventory, equipment, consumables, shops, loot
- **AI** — Pathfinding service, behavior trees, state machines
- **Map** — Map service, fog of war, minimap, compass
- **Environment** — Day/night cycle, weather service
- **Stats** — Character stats (game-specific layer)
- **CharacterCustomization** — Body parts, colors, presets
- **Perception** — AI vision, hearing, target tracking
- **Character** — Character service, persistence, factory

**Tier 2 — Thin Wrapper Services (4 registered services removed):**
- **VFX Service** — VFX controller, pooled particles, VFX library
- **Camera Service** — Camera behaviors, effects, zones
- **Quest Service** — Removed from ServiceManager Update loop
- **Spawn Service** — Spawn points, waves, formations

**GameObjects Assembly — Entirely removed:**
- Characters, NPCs, Combat components, AI state machines, Abilities
- Achievement/Quest/Tutorial/Relationship components
- Character customization, Animation, Audio bridges
- `LoLEngine-GameObjects.asmdef`

**Events removed** (12 event files from Core/Events/GameEvents/):
- AchievementEvents, CharacterEvents, CombatEvents, CurrencyEvents,
  ExplorationEvents, InventoryEvents, PlayerEvents, QuestEvents,
  RelationshipEvents, SpawnEvents, TutorialEvents, WorldEvents

**Persistence data removed:**
- CharacterPersistentData, InventoryPersistentData, QuestPersistentData,
  RelationshipPersistentData, Achievements/, Tutorials/

**ServiceConfiguration changes:**
- Removed: `enableCharacterPersistenceService`, `enableCharacterFactoryService`,
  `enableVFXService`, `enableCameraService`, `enableQuestService`, `enableSpawnService`
- Kept: All core/feature service toggles, `enableAssetUpdaterService`

**ConfigurableServiceInitializer changes:**
- Removed VFX, Camera, Quest, Spawn factory registrations and Create methods
- Removed `RegisterExternalAssemblyServices()` (GameObjects assembly bridge)

**Legacy ServiceInitializer changes:**
- Removed `InitializeDialogueService()`, `InitializeEnvironmentServices()`, `InitializeMapService()`

**ServiceManager changes:**
- Removed QuestService Update loop from `Update()`

**Samples removed:** 7 directories + 7 scripts for deprecated systems
**Tests removed:** `Tests/Edit/GameObjects/` directory
**Documentation:** 30+ files moved to `Documentation~/Deprecated/`

**Obsolete legacy code removed:**
- `ServiceInitializer.cs` — replaced by `ConfigurableServiceInitializer`
- `GameInitializer.cs` — replaced by `ImprovedGameInitializer`
- `DependencyChecker.cs` — replaced by `ImprovedDependencyChecker` / `RuntimeDependencyValidator`

**Sample game states moved to Samples~/GameState/:**
- `GameplayState.cs`, `MainMenuState.cs`, `PausedState.cs` — tutorial examples, not engine infrastructure

**LogChannel cleanup:**
- Removed unused enum values: `UI`, `AI`, `Animation`, `Scene`, `Characters`, `Gameplay`, `Effects`

**What's KEPT (not touched):**
- Core: Events (base), ServiceManagement, Config, ImprovedGameInitializer, ObjectPool,
  ResourceManagement, DataPersistence (base classes), Serialization, Compression,
  Encryption, Obfuscation, ImprovedDependencyChecker, Helpers, Input, Localization,
  TimeManagement, Notifications, GameState (base + manager), Audio
- Runtime: Logger, Singletons, Extensions, ServiceAwaiter, FrameRateLimiter,
  Combat/BattleEngine (separate assembly)
- AssetUpdaterService (ResourceManagement feature)
- All Helpers and Utility assemblies

## [0.13.1-beta] - 2026-02-05
### Engine Core [0.13.1]
### Removed
- **UIService**: Deprecated and removed entire UI management system
  - Deleted `Runtime/Scripts/Core/UI/` folder (interfaces, service, config, events, base classes, animation, navigation)
  - Deleted `Samples~/UISystem/` folder
  - Removed `enableUIService` from `ServiceConfiguration`
  - Removed `uiConfigName` from `ResourcePathConfig`
  - Removed UI service registration from `ConfigurableServiceInitializer`
- **DialogueUI**: Removed UI component that depended on UIScreen base class
  - DialogueService remains available for manual registration
- **UIScreenAudioPlayer**: Removed convenience component that coupled Audio to UI events
  - Use AudioService directly in MonoBehaviours instead

## [0.13.0-beta] - 2026-02-04
### Engine Core [0.13.0]
### Removed
- **SceneService**: Deprecated and removed entire SceneManagement system
  - Not useful for real-world Boot and MainMenu scenarios
  - Use Unity's native `UnityEngine.SceneManagement.SceneManager` directly
  - Deleted `Runtime/Scripts/Core/SceneManagement/` folder (all interfaces, services, configs, events, extensions, transitions, providers, UI)
  - Deleted `Samples~/SceneManagement/` folder
  - Deleted `Tests/Edit/Core/SceneManagement/` folder
  - Removed `enableSceneService` from `ServiceConfiguration`
  - Removed `sceneConfigName` from `ResourcePathConfig`
  - Removed scene service registration from `ConfigurableServiceInitializer`

## [0.12.7-beta] - 2026-02-02
### Engine Core [0.12.5]
### Removed
- **Boot Screen System** - Completely removed from the framework. Users should implement their own loading/splash screens.
    - Deleted `Runtime/Scripts/Core/BootSystem/` folder (all interfaces, services, controllers, UI components)
    - Deleted `BootScreenService`, `BootScreenController`, `HybridBootController`, `BootScreenUI`, `LoadingSpinner`
    - Deleted `BootConfiguration` ScriptableObject and `DefaultBootConfiguration.asset`
    - Deleted `StartupDiagnostics` class and related tests
    - Deleted `Editor/Build/BootDebugBuildValidator.cs` and `BootDebugValidationMenu.cs`
    - Removed `enableBootScreen` from `ServiceConfiguration`
    - Removed boot screen registration from `ConfigurableServiceInitializer`
    - Simplified diagnostic code blocks in service initialization
    - Documentation moved to `Documentation~/Deprecated/`
- Updated README.md and all documentation to remove Boot Screen references
- Updated sample code to remove Boot Screen references

## [0.12.6-beta] - 2026-02-02
### Added
- **SETUP_GUIDE.md** - New unified setup guide for developers new to LoL Engine

## [0.12.5-beta] - 2026-02-01
- Upgraded unity to version 6.3.6f1

## [0.12.4-beta] - 2026-01-23
### Engine Code [0.12.4]
- Changes to modify the PlayerInputController to take into account the newly modified InputConstants


## [0.12.3-beta] - 2026-01-23
- Corrections made to the  Implementation plans for the Input Constants and Added Plans to not use the Boot Screen from the LoLEngine
### Engine Code [0.12.3]
- Changes to modify the InputConstants to accomodate the separation of LoLEngine Specific Constants and GameSpecific Input Constants

## [0.12.2-beta] - 2026-01-20
- Performed Monthly Code review - 2026/01 - High Defects and Findings
### Engine Code [0.12.2] 
- InputService Event Unsubscription in InputService.cs
- AudioService Periodic Cleanup in AudioService.cs
- ResourceService Callback Leak in ResourceService.cs
- LocalizationService Shutdown in LocalizationService.cs

## [0.12.1-beta] - 2026-01-19
- Performed Monthly Code review - 2026/01
### Engine Code [0.12.1]
- Updated ResourceService to properly implement iLoLEngineService

## [0.11.15-beta] - 2026-01-19 
- Upgraded Unity Version to Version 6.3.4f1

## [0.11.14-beta] - 2025-12-30
### Engine Core [0.11.6]
- Spinner configuration is 100% self-contained on the LoadingSpinner component
- No more dual configuration - just set it in the Inspector on the spinner
- Auto-starts when the GameObject becomes active

## [0.11.13-beta] - 2025-12-30
### Engine Core [0.11.5]
- Removed Start() method from LoadingSpinner - no more auto-start

## [0.11.12-beta] - 2025-12-29
### Engine Core [0.11.4]
- Updated to Clarify that the Rotation Speed in the loading Spinner Component is a failback only

## [0.11.11-beta] - 2025-12-24
### Engine Core [0.11.3]
- Hotfix to fix all configs now use ResourcePathConfig consistently:

  | Config                   | Before Hotfix        | After Hotfix         | Status           |
  |--------------------------|----------------------|----------------------|------------------|
  | LoLEngineConfig          | ✅ ResourcePathConfig | ✅ ResourcePathConfig | No change needed |
  | AudioConfig              | ✅ ResourcePathConfig | ✅ ResourcePathConfig | No change needed |
  | LocalizationConfig       | ✅ ResourcePathConfig | ✅ ResourcePathConfig | No change needed |
  | ResourceManagementConfig | ✅ ResourcePathConfig | ✅ ResourcePathConfig | No change needed |
  | UIConfig                 | ✅ ResourcePathConfig | ✅ ResourcePathConfig | No change needed |
  | SceneConfig              | ❌ Hardcoded          | ✅ ResourcePathConfig | FIXED ✨          |
  | SaveConfig               | ❌ Hardcoded          | ✅ ResourcePathConfig | FIXED ✨          |


## [0.11.10-beta] - 2025-12-23
### Engine Core [0.11.2]
- DifficultyPresetCreator.cs was in Runtime/Scripts/Runtime/Combat/Editor/, moved to Editor/Combat/DifficultyPresetCreator.cs
- Removed the empty Editor folder from Runtime

## [0.11.9-beta] - 2025-12-21
### Engine Core [0.11.1]
- Fixed the Initialization related issue with the Scene Audio Manager.
- Updated instructions for Audio service to ensure the addressable for the Audio mixer is correctly set.

## [0.11.8-beta] - 2025-12-18

- Upgraded unity to version 6.3.2f1

## [0.11.7-beta] - 2025-12-03

- Fixed warnings with unused fields

## [0.11.5-beta] - 2025-12-02

- Updated manifest files

## [0.11.4-beta] - 2025-12-01

- Fixed the release automation shell script to adhere to the release branching strategy

## [0.11.3-beta] - 2025-12-01

- Fixed the release automation shell script to take care in case a release branch already exists.

## [0.11.2-beta] - 2025-12-01

- Cleanup of the folder structures to ensure proper organization, removal of additional unused folders.

## [0.11.1-beta] - 2025-12-01

- Upgraded unity to version 6.2.14f1

## [0.11.0-beta] - 2025-11-30

- Instructions provided for Versioning Strategy
- Instructions for incorporating the LoLEngine into custom games following standards used in AAA studios.
- Added scripts for creating releases, backport commits etc
- Baselined Engine Core and Battle Engine

### Engine Core [0.11.0]
### Battle Engine [0.2.0]

---

## [0.10.21-alpha] - 2025-11-30
### Engine Core [0.10.15]

#### Tutorial System
- Multiple tutorial step types (tooltips, highlights, prompts, interactive, etc.)
- Sequential and non-linear tutorial sequences
- Automatic progress persistence
- Flexible trigger conditions
- Event driven architecture
- Rich UI components for display
- Reward system integration
- Skip and replay functionality

#### Bug Fixes
- Fixed the Implementation of the SerializableDictionary class to make sure it can be used as a Core Dependency

---

## [0.10.20-alpha] - 2025-11-29
### Engine Core [0.10.14]

#### Achievement & Progression System
- Achievement Definition & StatDefinition ScriptableObjects
- Achievement Service - Centralized service with IAchievementService interface
- Achievement ProgressData & StatisticsData - PersistableData classes
- Achievement Tracker - Automatic event-based tracking
- Stat Tracker - Manual stat tracking component
- Progression Controller - XP-based leveling system
- Achievement NotificationUI - Popup toast notifications
- Achievement JournalUI & AchievementListItem - Full achievement list
- Achievement types
- Achievement categories 
- Event system integration  
- DataPersistence integration with automatic save/load
- Reward system

## [0.10.19-alpha] - 2025-11-26
### Engine Core [0.10.13]

#### Character Customization
- **Body Part Customization**: Swap heads, hair, facial features, and more
- **Color Customization**: Skin tones, hair colors, eye colors, clothing colors
- **Blend Shapes**: Facial morphing for detailed facial features
- **Equipment Visuals**: Visual representation of equipped items
- **Preset System**: Save and load complete character appearances
- **Random Generation**: Procedural character appearance generation
- **Persistence**: Full save/load integration


## [0.10.18-alpha] - 2025-11-25
### Engine Core [0.10.12]

#### Character Relationships System
- **Core.Relationships** - Data structures, enums, ScriptableObjects
- **Core.Relationships.Service** - IReputationService interface and implementation
- **Core.Events.GameEvents** - Relationship change events
- **GameObjects.Relationships** - MonoBehaviour components 

#### Integration Points With Existing Systems

1. **Dialogue System** 
    - DialogueCondition checks relationship/faction
    - DialogueData filters by relationship
    - Dialogue choices can modify relationships

2. **Quest System** 
    - Quest completion modifies relationships
    - Quest rewards can include reputation
    - Quest prerequisites can require relationships

3. **Inventory System** 
    - Gift-giving modifies relationships
    - Shop prices affected by relationship standing

4. **AI & Pathfinding** 
    - CompanionController uses PathAgent 

5. **Combat System** 
    - Helping in combat improves relationship
    - Companions fight alongside player 

6. **Event System** 
    - All relationship changes fire events 

#### Character Persistence Enhancements

**Key Features:**
- **Character State Persistence**: Health, stats, position, equipment, abilities
- **Inventory Persistence**: Items, equipment, currency, durability
- **Quest State Persistence**: Active/completed quests, objectives, timers
- **Relationship Persistence**: NPC relationships, faction reputation, companions

- **Character Persistence Integration** - Character-specific persistence (NEW - DEF-013)
    - CharacterPersistentData
    - InventoryPersistentData
    - QuestPersistentData
    - RelationshipPersistentData
    - Integration examples
  
## [0.10.17-alpha] - 2025-11-24
### Engine Code [0.10.11]

#### Advanced Character Upgrades

    Advanced Movement Controllers
    - WalJumpController.cs - Wall jump/slide mechanics
    - ClimbingController.cs - Ladder/rope climbing
    - SwimmingController.cs - Swimming, diving, oxygen management

    Skill Tree System
    - SkillNodeData.cs - ScriptableObject for skill definitions
    - SkillTreeData.cs - Complete tree configuration
    - SkillTreeController.cs - Runtime skill management

    Character Progression
    - CharacterLevelingController.cs - XP/leveling with AnimationCurve-based progression


## [0.10.16-alpha] - 2025-11-21
### Engine Core [0.10.10]

### Dialogue, Environmental, and Map Systems

**Dialogue System (Core.Dialogue)**

    - Dialogue Data (ScriptableObject) - Complete dialogue tree definition
    - Dialogue Node - Individual conversation node
    - Dialogue Choice - Player choice in conversation
    - Dialogue Condition - Condition evaluation system
    - Dialogue Action - Actions during dialogue
    - Dialogue Service (IDialogueService) - Dialogue state management
    - Dialogue UI (UIScreen) - Dialogue display 
    - Dialogue NPC (MonoBehaviour, IInteractable) - NPC conversations
    - DialogueEvents - Event system integration
        * DialogueStartedEvent - Fired when conversation begins
        * DialogueEndedEvent - Fired when conversation ends
        * DialogueNodeDisplayedEvent - Fired when node is shown
        * DialogueChoiceMadeEvent - Fired when player makes choice
        * DialogueActionExecutedEvent - Fired when action executes
        * DialogueVariableChangedEvent - Fired when variable changes

**Environmental Systems (Core.Environment)**

    - TimeOfDay enum - 7 time periods (Dawn, Morning, Noon, Afternoon, Dusk, Night, Midnight)
    - WeatherType enum - 11 weather types (Clear, Cloudy, LightRain, HeavyRain, Thunderstorm, LightSnow, HeavySnow, Blizzard, Fog, Sandstorm, Windy)
    - DayNightCycleConfig (ScriptableObject) - Day/night configuration
    - DayNightCycleService (IDayNightCycleService) - Time progression
    - WeatherConfig (ScriptableObject) - Weather configuration
    - WeatherPreset - Individual weather configuration
    - WeatherService (IWeatherService) - Weather management
    - EnvironmentManager (MonoBehaviour) - Update driver
    - EnvironmentalHazard (Base Class) - Hazard foundation
    - EnvironmentEvents - Event system integration
        * TimeOfDayChangedEvent - Fired when time period changes
        * DayNightCycleUpdateEvent - Fired each frame with time data
        * WeatherChangedEvent - Fired when weather changes
        * WeatherTransitionStartedEvent - Fired when transition begins
        * WeatherTransitionCompletedEvent - Fired when transition ends
        * WeatherUpdateEvent - Fired each frame with weather data

**Map & Navigation UI (Core.Map)**

    - MapMarkerType enum - 14 marker types (Player, QuestObjective, QuestGiver, Waypoint, POI, Enemy, Ally, Vendor, CraftingStation, Treasure, Dungeon, Boss, FastTravel, Custom)
    - MapConfig (ScriptableObject) - Map configuration
    - MapMarker - Map marker data
    - MapService (IMapService) - Map state management
    - Fog of War System - Dynamic exploration
    - MinimapUI (MonoBehaviour) - Minimap display
    - CompassUI (MonoBehaviour) - Directional compass
    - MapEvents - Event system integration
        * MapMarkerAddedEvent - Fired when marker is added
        * MapMarkerRemovedEvent - Fired when marker is removed
        * MapMarkerUpdatedEvent - Fired when marker is updated
        * MapMarkerTrackedEvent - Fired when marker tracking changes
        * FogOfWarRevealedEvent - Fired when fog is revealed
        * AreaDiscoveredEvent - Fired when new area is discovered

## [0.10.15-alpha] - 2025-11-21
### Engine Core [0.10.9]

**Status Effect System (Core.Combat & GameObjects.Characters.Combat)**

    - StatusEffectType enum - 11 built-in effect types (Poison, Burn, Bleed, Regeneration, Frost, Stun, Weaken, Brittle, Empowered, Haste, Custom)
    - EffectBehavior enum - 5 behaviors (DamageOverTime, HealOverTime, StatModifier, CrowdControl, Hybrid)
    - StackingBehavior enum - 5 stacking modes (NoStack, RefreshDuration, StackIntensity, ExtendDuration, Independent)

    - StatusEffectData (ScriptableObject) - Comprehensive status effect definition
        * Identity (effectId, effectName, description, effectType, behavior, stackingBehavior)
        * Duration & Timing (duration, tickRate, tickImmediately, maxStacks)
        * Damage/Healing (damagePerTick, healPerTick, damageType, canCrit)
        * Stat Modifiers (modifiesStats, modifiedStat, modifierType, modifierValue)
        * Crowd Control (causesStun, causesRoot, causesSilence)
        * Visual & Audio (applyVfxId, loopingVfxId, expireVfxId, icon, effectColor)
        * Advanced (canBeDispelled, priority, removeOnDeath)

    - ActiveStatusEffect - Runtime status effect instance
        * Duration tracking with tick timing
        * Stack management with automatic stacking behavior
        * Stat modifier integration
        * Tick value calculation with stack multipliers

    - StatusEffectController (MonoBehaviour) - Status effect manager
        * Effect application with automatic stacking
        * Effect removal (individual, by type, dispel all)
        * Automatic duration and tick updates
        * DOT/HOT execution with combat integration
        * Stat modifier application/removal
        * Crowd control state queries (IsStunned, IsRooted, IsSilenced)
        * Event firing (OnEffectApplied, OnEffectRemoved, OnEffectTicked)
        * Death event integration (auto-remove effects on death)

    - StatusEffectPresets - Factory class for common effects
        * Poison (5 dmg/sec, 5s, 3 stacks)
        * Burn (8 dmg/tick, 4s, refresh duration)
        * Bleed (3 dmg/sec, 6s, 5 stacks)
        * Regeneration (5 heal/sec, 10s)
        * Frost (-50% speed, 3s)
        * Weaken (-30% damage, 5s)
        * Empowered (+50% damage, 8s)
        * Haste (+50% speed, 5s)
        * Brittle (-20 armor, 4s, 3 stacks)
        * Stun (2s, full disable)
        * CreateHybridEffect() for custom combinations

**Combat Modifier System (GameObjects.Characters.Combat)**

    - CombatModifierController (MonoBehaviour) - Buff/debuff manager
        * Temporary stat modifications with automatic expiration
        * Convenience methods (AddDamageBoost, AddSpeedBoost, AddArmorBoost, AddCriticalChanceBoost)
        * Custom modifier support (AddCustomModifier)
        * Source tracking for bulk removal
        * Modifier queries (GetModifiersFromSource, GetModifiersForStat)
        * Events (OnModifierAdded, OnModifierRemoved)
        * Max modifiers limit (default: 50)

**Knockback System (GameObjects.Characters.Combat)**

    - KnockbackController (MonoBehaviour) - Physics-based knockback
        * 2D and 3D physics support (auto-detects Rigidbody type)
        * Knockback resistance (0-1 scale)
        * Minimum force threshold
        * Upward force component (launch effects)
        * Duration-based movement disable
        * Direction-based and position-based knockback
        * Convenience methods (ApplyKnockbackFromPosition, ApplyKnockbackFromSource)
        * Recovery system (automatic and manual)
        * Events (OnKnockbackApplied, OnKnockbackRecovered)
        * Configurable force modes (Impulse, Force, etc.)

**Stun System (GameObjects.Characters.Combat)**

    - StunController (MonoBehaviour) - Crowd control manager
        * Stun resistance (0-1 scale)
        * Minimum/maximum duration limits
        * Duration stacking support
        * Visual indicator with VFX spawn
        * Duration reduction method
        * Force recovery (cleanse)
        * Events (OnStunApplied, OnStunRecovered)
        * Progress tracking (RecoveryProgress property)

**Critical Hit Enhancements (Existing CombatCharacterComponent)**

    - Enhanced TakeDamageEnhanced() method
    - Critical hit calculation with configurable chance/multiplier
    - DamageResult struct with detailed feedback
    - Automatic critical hit event firing
    - Damage type resistance calculations

**Events Integration (Core.Events.GameEvents - Already existed)**

    - CharacterCriticalHitEvent - Critical hit notifications
    - StatusEffectAppliedEvent - Effect applied notifications
    - StatusEffectRemovedEvent - Effect removed notifications
    - StatusEffectTickedEvent - DOT/HOT tick notifications

**Example Scenes**

    - CombatEnhancementsExamples.cs - Comprehensive interactive examples
        * 5 scene setups (Status Effects, Modifiers, Knockback, Stun, Complete Demo)
        * 7 runtime test methods (Poison, Burn, Frost, Buffs, Knockback, Stun, Full Combat)
        * Event listeners for all combat events
        * Auto-setup with visual markers

### Character Audio Integration System

#### Added

**Core Audio Components (GameObjects.Characters.Audio)**
    
    - CharacterFootstepHandler- Surface-aware footstep audio
        * 15 surface types with automatic detection via raycast
        * Tag, physics material, and terrain texture detection
        * Random variation (multiple clips, pitch/volume randomization)
        * 3D spatial audio support
        * Configurable cooldowns and manual surface override
        * Virtual methods for custom surface detection

    - CharacterCombatAudioHandler- Event-driven combat audio
        * Attack sounds (melee, ranged, magic, special)
        * Hit reaction sounds with severity detection (light, medium, heavy, critical)
        * Blocked/dodged sounds
        * Death and kill celebration sounds
        * Audio cooldowns to prevent spam
        * Hit severity based on damage percentage of max HP

      - CharacterAnimationAudioBridge - Animation event audio mapping
          * Animation event → audio ID mapping system
          * Multiple audio IDs per event with random selection
          * Per-mapping pitch/volume randomization
          * 3D spatial audio support
          * Runtime mapping management (add/remove/clear)

      - EnvironmentalAudioTrigger - Zone-based ambient audio
          * Looping ambient sounds with fade in/out
          * Random one-shot sounds at configurable intervals
          * Music zone transitions
          * Multiple zone configurations
          * Trigger-based activation

**Configuration Assets (Core.Audio)**
    
    - FootstepAudioConfig - ScriptableObject for footstep configuration
        * Per-surface audio settings (volume, pitch, 3D audio, track)
        * Surface detection settings (distance, layer mask, terrain)

    - FootstepSurfaceConfig - Per-surface configuration
        * Multiple audio IDs for variation
        * Volume/pitch randomization ranges
        * 3D audio and track settings

    - CombatAudioConfig - ScriptableObject for combat audio
        * Attack sounds by type (melee, ranged, magic, special)
        * Hit reactions by severity with configurable thresholds
        * Healing, death, and kill sounds
        * Volume scaling and audio settings

    - SurfaceType enum - 15 surface types (Grass, Stone, Wood, Metal, Water, Mud, Sand, Snow, Gravel, Carpet, Tile, Concrete, Dirt, Ice, Default)

**Audio System Updates**
- Added AmbientTrack property to AudioExtensions (matching Music, SFX, UI, Voice)
- Updated CharacterDeathHandler to use AudioService instead of legacy Unity audio

### Combat Enhancements System
#### Added

**Critical Hit System (GameObjects.Characters.Combat)**
- DamageResult struct - Detailed damage calculation results
- Enhanced CombatCharacterComponent 

- DamageType enum (Core.Combat) - 4 damage types
    * Physical (reduced by Armor)
    * Magic (reduced by MagicResistance)
    * True (ignores resistances)
    * Environmental (reduced by Armor * 0.5)

- CharacterCriticalHitEvent - Event for critical hit notifications

**Damage-Scaled Knockback (GameObjects.Characters.Knockback)**
- Enhanced KnockbackController with damage scaling

**Status Effect System (Core.Combat.StatusEffects)**
    
    - StatusEffectData - ScriptableObject with 100+ configuration options
        * Identity (effectId, displayName, icon, description)
        * Duration and tick settings
        * 4 stack behaviors (None, RefreshDuration, IncreaseIntensity, AddInstance)
        * Stat modifier system (20+ stat types, 3 modifier types)
        * DoT/HoT configuration with damage/healing scaling
        * Crowd control settings (movement, actions, turning restrictions)
        * VFX and audio integration
        * Immunity and dispel configuration
        * Expiration behaviors (Remove, FadeOut, Burst, Transform)

    - ActiveStatusEffect - Runtime effect tracking

    - StatusEffectManager- Main status effect component
      * Effect application with automatic stacking
      * Effect removal with cleanup
      * DoT/HoT tick system (processes every frame)
      * Crowd control state management (IsStunned, IsRooted, IsSilenced, CanMove, CanAct)
      * Freeze implementation (isKinematic, velocity = 0)
      * Ragdoll implementation (physics-based stun)
      * VFX integration points
      * Audio integration (apply, tick, remove sounds)
      * Immunity system
      * Dispel system (by priority)

**Status Effect Enums (Core.Combat.StatusEffects)**
- StatusEffectCategory (5): Buff, Debuff, CrowdControl, DamageOverTime, HealOverTime
- StatusEffectType (30+): Poison, Burn, Bleed, Regeneration, Shield, Stun, Root, Freeze, etc.
- StackBehavior (4): None, RefreshDuration, IncreaseIntensity, AddInstance
- ExpirationBehavior (4): Remove, FadeOut, Burst, Transform
- EffectDisplayStyle (6): Icon, IconWithTimer, IconWithStacks, IconWithBoth, ProgressBar, None
- StatModifierType (20+): AttackDamage, Armor, MoveSpeed, CriticalChance, etc.
- ModifierType (3): Flat, PercentageAdditive, PercentageMultiplicative

**Status Effect Events (Core.Events.GameEvents)**
- StatusEffectAppliedEvent - Fired when effect is applied
- StatusEffectRemovedEvent - Fired when effect is removed/expires
- StatusEffectTickedEvent - Fired when DoT/HoT ticks

**Status Effect Presets**
    
    - StatusEffectPresetGenerator - Editor utility
        * Tools > LoLEngine > Combat > Generate Status Effect Presets
        * 12 preset status effects:
            - Poison (DoT, 5 dmg/sec, 10s, stacking)
            - Burn (DoT, 8 dmg/sec, 5s)
            - Bleed (DoT, 3 dmg/sec, 15s, high stacks)
            - Regeneration (HoT, 5 heal/sec, 10s)
            - Shield (+50% armor, 5s)
            - AttackBoost (+30% damage, 8s)
            - SpeedBoost (+50% speed, 6s)
            - Weakness (-30% damage, 6s)
            - Vulnerability (-25% armor, 5s)
            - Slow (-50% speed, 4s)
            - Stun (prevents all, 2s, ragdoll)
            - Freeze (prevents movement/turning, 3s)

**Stat Modifier System (Core.Combat.StatusEffects)**
- StatModifierConfig - Stat modification configuration
    * 20+ stat types (combat, movement, health/mana)
    * 3 modifier types (Flat, PercentageAdditive, PercentageMultiplicative)

#### Fixed
- Moved DamageType enum from GameObjects to Core assembly to prevent circular dependency
- Fixed audio track assignments to use AudioService.GetTrack() instead of string
- Fixed event subscription to use IEventManager.AddListener instead of non-existent GameEventBus
- Fixed audio method calls to use StopSound instead of Stop
- Fixed combat audio to use Vigor instead of MaxHP for Resonance-Battle system
- Fixed event handler method names to use OnGameEvent instead of OnEvent
---
## [0.10.14-alpha] - 2025-11-20
### Engine Core [0.10.8]

### Spawn Management System

**Phase 1 & 2: Core Spawning and Wave System**

    Core Components (Core.Spawning):
        - SpawnEnums - All spawn-related enumerations (SpawnPattern, SpawnConditionType, WaveState, SpawnResult, WaveCompletionCondition, SpawnTriggerMode)
        - SpawnConfiguration - ScriptableObject for spawn settings (prefab, counts, timing, respawn, limits, conditions, position)
        - SpawnData - Runtime spawn instance tracking (entity, timing, respawn count, statistics)
        - WaveConfiguration - ScriptableObject for wave definitions (spawn entries, timing, completion, difficulty scaling)
        - WaveData - Runtime wave instance tracking (entities, statistics, completion tracking)

    Event System (Core.Events.GameEvents):
        - SpawnEvents - Comprehensive event system
            * EntitySpawnedEvent - Entity spawned notification
            * EntityDespawnedEvent - Entity despawned notification
            * WaveStartedEvent - Wave started notification
            * WaveCompletedEvent - Wave completed notification
            * AllWavesCompletedEvent - All waves completed notification
            * WaveFailedEvent - Wave failed notification
            * FormationSpawnedEvent - Formation spawned notification

    GameObject Components (GameObjects.Spawning):
        - SpawnPoint - Single-entity spawn point with patterns, respawn, cooldowns, conditions, pooling
        - SpawnZone - Area-based spawning with shapes, random distribution, auto-maintain
        - WaveSpawner - Wave-based spawning with progression, difficulty scaling, completion conditions

**Phase 3: Advanced Spawning Features**

    Spawn Conditions (Core.Spawning.Conditions):
        - SpawnCondition - Abstract base class for extensible condition system
        - DistanceCondition - Min/max distance from target
        - LevelCondition - Player level requirements with virtual GetPlayerLevel()
        - TimeCondition - Time-of-day spawning with virtual GetCurrentTime()
        - FlagCondition - Game state flag checking with virtual GetFlagValue()

    Formation System (Core.Spawning):
        - FormationPattern - ScriptableObject for formation patterns
            * 6 Formation Shapes: Line, Circle, Grid, V-Formation, Random, Custom
            * Configurable spacing, rotation, scaling
            * Position calculation engine for squad spawning

    Spawn Service (Core.Spawning.Service):
        - ISpawnService - Service interface for centralized spawn management
        - SpawnService - Service implementation
            * Spawner registration/tracking
            * Entity tracking with spawner references
            * Global spawn limits and budget enforcement
            * Pause/resume spawning control
            * Statistics tracking (total spawns, despawns, active entities)

    GameObject Components (GameObjects.Spawning):
        - FormationSpawner - Formation-based squad spawning with leader support
        - SpawnTrigger - Event-based spawning (OnEnter, OnExit, Manual modes)

#### Fixed

    Event System Integration:
        - Fixed all spawn components to use correct IEventListener<CharacterDeathEvent> pattern
        - Updated SpawnPoint, SpawnZone, WaveSpawner to implement IEventListener interface
        - Changed event subscription to use EventStartListening/EventStopListening extension methods
        - Removed non-existent GameEvent.AddListener/RemoveListener static method calls

    Architectural Violations:
        - Fixed LevelCondition to not depend on GameObjects.Characters.Base.Character
        - Fixed TimeCondition to not depend on non-existent Time.Service namespace
        - Removed duplicate FormationShape enum definition from SpawnEnums
        - All conditions now use virtual methods for extensibility without assembly dependencies

    Event Signatures:
        - Fixed FormationSpawner.SpawnFormation() to call FormationSpawned with correct parameters
        - Updated to pass GameObject[] array, spawner, formationName string, and count

#### Architecture Notes

    Proper Assembly Separation:
        - Core assembly contains all data structures, conditions, service interfaces 
        - GameObjects assembly contains all MonoBehaviour components 
        - No Core → GameObjects dependencies maintained throughout
        - Spawn conditions use virtual methods for game-specific integration
        - SpawnService tracks without controlling to avoid assembly dependencies

---

## [0.10.13-alpha] - 2025-11-19
### Engine Core [0.10.7]

#### Bug Fixes

Fixing Service Locator Pattern Violations
- ConfigurableServiceInitializer
- Refactored CameraManagerService to CameraService
- 

## [0.10.12-alpha] - 2025-11-18
### Engine Core [0.10.6]

#### Enhanced Interaction UI System

New
- InteractionUIConfig.cs - ScriptableObject for centralized configuration 
- InteractionPromptUI.cs - Animated interaction prompts
- InteractionProgressUI.cs - Progress bars for hold-to-interact actions
- InteractionUIManager.cs - Centralized UI orchestration

Existing
- InteractionManager.cs: Added InteractionUIManager integration


## [0.10.12-alpha] - 2025-11-18
### Engine Core [0.10.5]

#### Quest System

Core Assembly:
- Quest Data System - ScriptableObject-based quest and objective definitions
- Quest Runtime Logic - Quest state tracking, objective progress, time limits
- Quest Service - Centralized quest management with IQuestService interface
- Quest Events - 8 game events for quest lifecycle (accepted, completed, objectives, etc.)
- Quest Rewards - Multi-currency, items, and experience rewards

GameObjects Assembly:
- QuestTrackerComponent - Player quest tracking with automatic objective updates
- QuestGiverComponent - NPC quest-giving functionality
- QuestTrackerUI - Simple UI for displaying tracked quests
- Updated QuestNPC - Integrated with new Quest System

Sample Scripts:
1. QuestSystemSampleManager.cs - Main demo controller with 8 interactive examples via context menu:
- Example 1: Basic Quest Flow
- Example 2: Simulate Objective Progress
- Example 3: Complete Active Quest
- Example 4: Timed Quest with countdown
- Example 5: Quest Chain with prerequisites
- Example 6: Quest Journal view 
- Example 7: Quest Events logging 
- Example 8: Abandon Quest

## [0.10.12-alpha] - 2025-11-17
### Engine Core [0.10.4]

#### Inventory and Item System

- Item management with 6 item categories
- Slot-based inventory with weight limits
- Equipment with automatic stat modifiers
- Consumables with effects and buffs
- Loot drops from enemies
- Interactive loot containers
- Multiple currency types
- Fully functional shop system
- Complete event system integration
- Full serialization for save/load


    1. Core Item System
       - ItemData - ScriptableObject for item definitions with rarity, categories, weight, value
       - Item - Runtime instances with stacking, GUID tracking, custom data, serialization
       - ItemDatabase - Centralized registry with fast lookup, search, validation
       - ItemEnums - Categories, Rarity levels, EquipmentSlots
       - IUsable - Interface for consumable items

    2. Inventory System
       - Inventory - Slot-based with capacity/weight limits, auto-stacking, sorting
       - InventorySlot - Individual slots with locking support
       - InventoryComponent - MonoBehaviour wrapper with events and persistence
       - InventoryEvents - Complete event system (ItemAdded, ItemRemoved, ItemUsed, etc.)

    3. Equipment System
       - EquipmentData - Equipment with stat modifiers and level requirements
       - CharacterEquipment - Equip/unequip with automatic stat application
       - Two-handed weapon support
       - Equipment visuals with attachment points
       - Full serialization for save/load

    4. Consumables System
       - ConsumableData - Base class for all consumables
       - PotionData - Health/Mana/Stamina restoration (instant or over time)
       - FoodData - Health restoration + temporary stat buffs
       - VFX and audio integration

    5. ShopkeeperNPC Integration
       - ProcessPurchase() - Now adds items to player's InventoryComponent
       - ProcessSale() - Now removes items from player's inventory
       - Full validation (space, weight, database lookups)

--- 
## [0.10.11-alpha] - 2025-11-17
### Engine Code [0.10.3]

#### Combat VFX Integration System
- **Event-Driven VFX** - Automatically triggers effects based on CharacterEvents
- **CombatVFXController** - One-component setup for automatic VFX integration
- **Status Effect VFX** - Looping effects with duration management
- **Customizable Effects** - Configure VFX IDs and offsets per event type
- **Performance Optimized** - Uses VFX pooling for efficient instantiation
- **Easy Integration** - Works with existing CombatCharacterComponent

---

## [0.10.10-alpha] - 2025-11-17
### Engine Core [0.10.2]

Implemented comprehensive AI & Pathfinding system with NavMesh integration,
perception, behavior trees, and state machines.

#### Pathfinding (Core/AI/)
- IPathfindingService - Interface for pathfinding operations
- PathfindingService - NavMesh wrapper with path calculation, sampling
- PathAgent - Character component for NavMesh navigation

#### Perception (Core/Perception/)
- AIPerception - Vision cone, hearing, target tracking with memory
- LineOfSightChecker - LOS utility for 2D and 3D
- PerceivedTarget - Target tracking with confidence decay

#### Behavior Trees (Core/AI/BehaviorTree/)
- BehaviorTree - Main tree component with blackboard
- Blackboard - Shared data storage for AI
- CompositeNodes - Sequence, Selector, Parallel
- DecoratorNodes - Inverter, Repeater, Conditional, Timeout, Cooldown
- LeafNodes - Action, Condition, Wait, Log, Random

#### State Machine (GameObjects/Characters/AI/)
- AIStateMachine - Generic state machine controller
- AIStateBase - Base class for custom states
- Built-in states: Idle, Patrol, Chase, Attack
- SimpleAIController - Easy one-component AI setup

### Examples
- AIExamples.cs - Comprehensive examples component with 4 scene setups:
- Basic AI Scene - SimpleAIController with 3 enemies
- Behavior Tree Scene - Custom decision tree AI (Flee/Attack/Chase/Patrol)
- State Machine Scene - Custom states and transitions
- Perception Test Scene - Vision cone and hearing visualization

---

## [0.10.9-alpha] - 2025-11-17

### Engine Core [0.10.1]

#### Animation System

1. AnimationController
    - Low-level wrapper around Unity's Animator
    - Simplified API for parameter setting (Bool, Float, Int, Trigger)
    - Automatic parameter hash caching for performance
    
2. AnimationEventHandler.cs (~250 lines)
    - Handles animation events from Unity's Animation window
    - Pre-defined event types:
        Footstep events (left/right foot)
        Attack events (with attack type)
        Weapon swing, projectile spawn, VFX spawn, sounds
        Custom events with parameters

3. CharacterAnimator.cs (~500 lines)
    - High-level character animation controller
    - Standard parameters (Speed, VelocityX/Y, IsGrounded, IsCrouching, etc.)
    - Convenience methods (TriggerJump, TriggerAttack, TriggerDamage, etc.)

4. Character Integration

#### BUG RESOLVED: 
- CRITICAL: Removed deprecated Health component and completed migration to two-tier stats system.
- Removed incorrect ServiceLocator.IsServiceRegistered<T>() calls

### Health.cs Removed
- Moved to Assets/LoLEngine/Deprecated/v0.10.8/Health.cs.deprecated
- Created deprecation README explaining removal

### New Helper Components
1. InvincibilityController.cs
    - Replaces Health invincibility system
    - Configurable i-frames duration
    - Visual feedback (sprite flashing)
    - Events: OnDamageTaken, OnDamageBlocked, OnInvincibilityStarted/Ended
    - Works with both CharacterStatsComponent and CombatCharacterComponent

2. CharacterDeathHandler.cs
    - Replaces Health death handling
    - Features: DestroyOnDeath, DisableCollidersOnDeath, ChangeLayerOnDeath
    - VFX and audio integration
    - Death animation support
    - Events: OnBeforeDeathHandled, OnDeathHandled

#### Additional Changes

1. DamageType Namespace Collision
    - Deleted duplicate DamageType.cs from Stats namespace
    - Updated CharacterStats.cs to use existing enum from Interfaces namespace
    - Fixed: DamageType.Magical → DamageType.Magic 
    - Added DamageType.Environmental case handling

2. ServiceLocator Namespace Errors - DamageFeedbackController.cs
    - Removed incorrect Core.ServiceManagement.ServiceLocator reference
    - Correct namespace: Core.ServiceManagement.Service.ServiceLocator
    - Removed incorrect Core.Camera.ICameraService reference
    - Correct namespace: Core.Camera.Interfaces.ICameraService
    - Simplified camera shake to log-only with TODO for future integration

### Event Management Changes

Added new Character Events
- CharacterDamagedEvent
- CharacterHealedEvent
- CharacterDeathEvent
- CharacterAttackEvent
- CharacterKillEvent
- ItemPurchasedEvent
- ItemSoldEvent
- ShopOpenedEvent
- ShopClosedEvent
- CharacterEvents

---

## [0.10.8-alpha] - 2025-11-16

### Engine Core [0.10.0]

#### Bug Fixes

    NonPlayableCharacter - Fixed incorrect CharacterType return value

**Major Architectural Change:** Migrated to two-tier stats system for tactical turn-based games.

    Combat Characters (PlayerCharacter, EnemyNPC):
        - Now use `LoLEngine.Combat.Resolution.CharacterStats` (Resonance-Battle system)
        - Full tactical combat: CurrentHP, Vigor, Defense, Evasion, Focus, Openings, Combat Styles
        - New `CombatCharacterComponent` for Unity integration
        - No longer use `Health` component

    Non-Combat NPCs (QuestNPC, ShopkeeperNPC, InteractableNPC):
        - Now use extended `LoLEngine.GameObjects.Characters.Stats.CharacterStats`
        - NEW: CurrentHealth/CurrentMana/CurrentStamina tracking
        - NEW: TakeDamage(), Heal() methods
        - NEW: OnDamaged, OnHealed, OnDeath events
        - Lightweight - no tactical combat overhead
        - No longer use `Health` component

**Deprecated/Removed:**
    
- `Health` component marked `[Obsolete]`

#### **Character Stats Extension**
- `CharacterStats.CurrentHealth` - Current HP tracking (was missing)
- `CharacterStats.CurrentMana` - Current mana tracking (was missing)
- `CharacterStats.CurrentStamina` - Current stamina tracking (was missing)
- `CharacterStats.IsAlive` - Alive/dead status
- `CharacterStats.TakeDamage(float, GameObject)` - Damage handling with source tracking
- `CharacterStats.Heal(float)` - Healing with overflow prevention
- `CharacterStats.RestoreToFull()` - Full HP/Mana/Stamina restoration
- `CharacterStats.SpendMana(float)` / `RestoreMana(float)` - Mana management
- `CharacterStats.SpendStamina(float)` / `RestoreStamina(float)` - Stamina management
- `CharacterStats.OnHealthChanged` - Event for HP changes (old, new values)
- `CharacterStats.OnManaChanged` - Event for mana changes
- `CharacterStats.OnStaminaChanged` - Event for stamina changes
- `CharacterStats.OnDamaged` - Event for damage taken (amount, source)
- `CharacterStats.OnHealed` - Event for healing received
- `CharacterStats.OnDeath` - Event for character death

#### **Combat Character Component**
- `CombatCharacterComponent` - MonoBehaviour wrapper for Combat.CharacterStats
- Provides Unity component interface for Resonance-Battle combat system
- Integrates with AttackResolver for damage calculation
- Supports VFX integration via `CombatVFXIntegration`

#### **Interface Updates**
- `ICharacter.GetCombatStats()` - Access combat stats for combat characters
- `ICharacter.GetHealth()` marked `[Obsolete]` with migration guidance

#### **Documentation**
- `CharacterSystem-HealthStats-ImpactAnalysis.md` - Comprehensive migration impact analysis
- `CharacterSystem-MigrationGuide.md` - Step-by-step migration guide (TODO)
- Updated `CharacterSystem-ActionPlan.md` - Marked BUG-002 as resolved
- Updated `ReusableComponents-Status.md` - Character system status updated

### Changed

#### **PlayerCharacter**
- **BREAKING:** Now uses `CombatCharacterComponent` instead of `Health`
- Automatically initializes with Combat.CharacterStats on Start()
- Default: Level 1, Balanced style, Martial power source
- Access via: `player.GetComponent<CombatCharacterComponent>().CombatStats`

#### **EnemyNPC**
- **BREAKING:** Now uses `CombatCharacterComponent` instead of `Health`
- Automatically initializes with Combat.CharacterStats on Start()
- Configurable enemy level and tier (Minion, Elite, Boss, etc.)
- ICombatant interface now delegates to Combat.CharacterStats
- Access via: `enemy.GetComponent<CombatCharacterComponent>().CombatStats`

#### **NonPlayableCharacter** (and derived: QuestNPC, ShopkeeperNPC, InteractableNPC)
- **BREAKING:** Now uses extended `CharacterStatsComponent` instead of `Health`
- `CharacterStatsComponent` now has CurrentHP tracking
- Damage/heal methods available via stats component
- Access via: `npc.GetStats().CurrentHealth` or `npc.GetStats().TakeDamage(10f)`

#### **Character (Base Class)**
- Removed required Health component dependency
- `GetHealth()` marked `[Obsolete]`
- `IsAlive` now checks appropriate stats system (Combat or GameObject)
- Initialization updated to work with new stats systems

#### **CharacterStatsComponent**
- **BREAKING:** Now delegates CurrentHealth/Mana/Stamina properties
- **BREAKING:** Now delegates TakeDamage/Heal methods
- **BREAKING:** Now exposes OnDamaged/OnHealed/OnDeath events
- Backward compatible: existing stat modifiers unchanged

#### **Character Factory**
- Updated `CreatePlayer()` to use Combat.CharacterStats
- Updated `CreateEnemy()` to use Combat.CharacterStats
- Updated `CreateQuestNPC()` to use extended CharacterStatsComponent
- Updated `CreateShopkeeper()` to use extended CharacterStatsComponent
- **BREAKING:** No longer adds Health component to any characters

### Deprecated

- `Health` component - Use `CharacterStatsComponent` for non-combat or `CombatCharacterComponent` for combat
- `ICharacter.GetHealth()` - Use `GetStats()` or `GetCombatStats()` instead

---

## [0.10.7-alpha] - 2025-11-15

### Engine Core [0.9.14]

#### Bug Fixes

    ObjectPool
        - Modify ObjectPool.Clear() to also destroy its own root GameObject.

#### Camera System

    Camera Behaviors (5 types):
        - FollowCamera - Smooth following with look-ahead, deadzones, and boundaries
        - OrbitCamera - Third-person orbital camera with collision detection and zoom
        - TopDownCamera - Strategy/isometric view with edge panning and rotation
        - SideScrollCamera - 2D platformer camera with look-ahead and deadzones
        - FirstPersonCamera - FPS camera with mouse look and head bob
    Camera Effects:
        - CameraShake - 5 presets (Light, Medium, Heavy, Explosion, Earthquake) + custom
        - CameraZoom - Smooth FOV/orthographic transitions
    Management:
        - CameraManagerService - Centralized camera switching and transitions
        - CameraZone - Automatic camera switching on trigger
        - CameraConfiner - Keep cameras within boundaries

#### VFX Management System
    
    Components:
        - VFXController - Base class with lifecycle management
        - PooledParticleSystem - Automatic pooling integration
        - VFXSpawner - Spawning manager with preloading
        - VFXService - Centralized VFX management
        - VFXLibrary - ScriptableObject asset organization
        - HitEffect - Combat impacts with camera shake
        - ExplosionEffect - Explosions with light flash
        - TrailEffect - Movement trails
        - HealthVFXIntegration - Auto-spawn on damage/heal/death
   
    Documentation: VFXSystem.md

#### Interaction and Trigger System
    
    Trigger System (7 components)
        - BaseTrigger - Base class with layer/tag filtering and debug visualization
        - GenericTrigger - Flexible trigger with UnityEvents (enter/exit/stay)
        - ConditionalTrigger - 5 condition types (custom, component check, tag, velocity, count)
        - DelayedTrigger - Timed activation with progress tracking
        - OneShotTrigger - Single-use trigger for cutscenes/events
        - ReusableTrigger - Multi-use with cooldown and max activations
        - TriggerGroup - Coordinate multiple triggers (All/Any/Sequential/Exactly modes)
    
    Interaction System (4 components)
        - BaseInteractable - IInteractable implementation with highlighting
        - InteractionZone - Proximity detection with priority system
        - InteractionManager - Centralized service for input handling
        - InteractionPrompt - UI display with world/screen space support
    
    Common Interactables (7 components)
        - PressurePlate - Weight-activated with visual feedback
        - Button - 3 modes: OneShot, Toggle, Hold
        - Lever - Multi-state control (2+ positions)
        - Door - 4 types: Sliding, Swinging, Animation, Custom
        - Chest - Loot container with lock/key system
        - PickupItem - Collectibles (automatic/manual pickup)
        - ExamineObject - Multi-page inspection system

#### Samples added

    - Camera Samples added
    - Interactables Samples added
    - VFX Samples Added

---

## [0.10.6-alpha] - 2025-11-15

### Engine Core [0.9.13]

#### Added new test Suites for the following

        1. ObjectPoolServiceTests (60+ tests)
            - Initialization and shutdown
            - Get/Return operations
            - Pool statistics
            - Callbacks and preloading
            - Memory management
            - Edge cases and stress tests

        2. TimeServiceTests (50+ tests)
            - Time scaling (global and channel-specific)
            - Pause/resume functionality
            - Timer creation and management (countdown, stopwatch, periodic)
            - Time channels with independent scaling
            - Event triggering

        3. EventServiceTests (45+ tests)
            - Listener management (add/remove)
            - Event triggering and distribution
            - Subscription validation
            - Thread safety

        4. GameStateManagerServiceTests (35+ tests)
            - State registration and retrieval
            - State transitions
            - Update/FixedUpdate lifecycle

        5. NotificationServiceTests (40+ tests)
            - Notification sending and receiving
            - Category-based and ID-based subscriptions
            - Active notifications management
            - Notification history
            - Category enable/disable

#### Coverage Improvements:
    ObjectPool: 0% → ~90% coverage
    TimeManagement: 0% → ~88% coverage
    Events: ~30% → ~92% coverage
    GameState: 0% → ~87% coverage
    Notifications: 0% → ~89% coverage

## [0.10.5-alpha] - 2025-11-15
### Engine Core [0.9.12]
    - Upgraded Unity Version to 6.2.11f1
    - Updated documentation for 
        - GameInitializer.md
        - GameState.md
        - Input.md (Completely Rewritten)
        - Localization.md (Transformed from Spec to User Guide)
        - Notifications.md
    - New Documentation added for ImprovedGameInitializer.md 
#### Critical Fixes:
    - Object Pool - Added Thread Safety
        - Added _lock object for synchronization 
        - Protected all Count properties with locks 
        - Synchronized Get(), Return(), Clear(), PreloadItems(), UpdateMaxSize()
        - Optimized lock duration by moving Unity API calls outside locks 
        - Callbacks execute outside locks to prevent deadlocks
    - Scene Management - Circular Dependency Detection. Added dependency chain tracking in SceneService.cs
    - Service Manager - Added configurable LOCK_TIMEOUT_MS constant, Improved code clarity and maintainability
    - Time Management
        - Optimized UpdateAllTimers() - 33% faster, zero GC allocations
        - Added time scale validation (negative values, physics warnings)
    - UI System
        - Added UICoroutineRunner for async operations
    - Helpers
        - Fixed ShuffleBag edge cases (size=1 bags, empty validation)
        - Implemented proper Fisher-Yates shuffle algorithm
#### Documentation Improvements:
    - Added comprehensive limitations section to Serialization.md
    - Updated documentation for ObjectPool, SceneManagement and Serialization.
#### Unit Tests
    - Added ObjectPoolThreadSafetyTests.cs to validate thread-safe operations
    - Added SceneManagementCircularDependencyTests.cs to verify circular dependency detection

---

## [0.10.4-alpha] - 2025-11-12
### Battle Engine [0.1.1]
#### Phase 1 Changes for the Battle Engine
- Weighted Dice System 
- Core Combat Mechanics
  - Tri-dice rolling (Skill/Light/Shadow)
  - Hit resolution (Skill + Level ≥ Evasion)
  - Damage calculation (Weapon + Bonus - Armor)
  - Critical hits (natural max on Skill die)
  - Severe Hits (damage ≥ Threshold, checked BEFORE armor)
  - Graze rule (EV-1 = 1 damage + Opening)
  - Resonance system (Light vs Shadow → resources)
-  Dynamic Probabilities
- Combat Simulator
  - Runs 1000+ simulated combats 
  - Tests hit rates, combat length, win rates and Reports critical/severe hit frequencies 
  - Compares actual vs expected results
- NPC System
  - Simplified d20 attacks (not tri-dice)

#### Phase 2 adds tactical depth to the Resonance Battle System with:
- **Focus Tier System** - Dynamic die upgrades based on Focus levels
- **Openings System** - Tactical bonuses from smart positioning and defensive play
- **Environmental Advantages** - Terrain and positioning bonuses
- **Enhanced Abilities** - Combat abilities with Focus/Resolve costs

#### Phase 3 Implementation to add Power Sources, Combat Styles, Pressure System
- Power Sources
- Combat Styles
- GM Pressure System 
- Signature Abilities
- Updated CharacterStats

#### Combat Log System 
- **Comprehensive Event Tracking**: Every dice roll, attack, damage, resonance, ability logged
- **Importance Levels**: Filter by Trivial/Normal/Important/Major/Critical
- **UI-Friendly Formatting**: Color-coded with icons for Unity UI/TextMeshPro
- **Flexible Filtering**: By round, actor, event type, tags, importance
- **Multiple Display Modes**: Compact, detailed, round-by-round summaries
- **Export Support**: Save to file for debugging or analysis
- **Performance Optimized**: Auto-pruning, minimal allocations


## [0.10.3-alpha] - 2025-11-08
### Engine Core [0.9.11]
- Changes to Improve the ImprovedGameInitializer to fix the issuex`s with the unhandled Asynch pattern, race Condition and unregister
- Changes to ServiceLocator to add IDisposable and removed race conditions
- ImprovedGameInitializer - Async/Coroutine Pattern
  - Problem: WaitUntil creates a lambda that's called every frame until the task completes. Performance Gain: ~15-20% reduction in frame time during initialization, no lambda allocations.
- ServiceLocator - Reduce Lock Contention
  - Every service lookup acquires locks, even for read operations. Under heavy load with many concurrent reads, this causes thread contention.
  - Optimized Solution - Add Fast Path Cache
  - Optimize TryGet Pattern, Checks persistent services first, then regular services - always acquires two locks even if service is in the first dictionary. Performance Gain: Prevents infinite waiting on locks, ~5% improvement in lookup time.
  - Pool StringBuilders for Large Logs 
  - Profile-Guided Optimization; Added performance metrics to identify bottlenecks
- ConfigurableServiceInitializer 
  - Reduce Reflection Overhead (CreateServiceWithType uses reflection extensively for every service creation) 
- Updated LoLLogger Logic to reduce string Allocations
- Updated Audio manager for quality of Life improvements related to Initialization and exception management
- Updated Boot Manager to accomodate for the following:
  - Added allowSceneReload field to BootConfiguration with tooltip
  - Updated TransitionToMainScene to respect AllowSceneReload setting
  - Fixes issue where boot scene cannot reload itself when NextSceneName matches current scene
- Changes Encryptor to Use HKDF or direct SHA256 instead of PBKDF2
- Added new Obfuscation Interface and Service
  - Updated ImprovedDependencyChecker to account for the Obfuscation
  - Updated LoLengineConfig to include the Option to use Obfuscation instead of Encryption

## [0.10.2-alpha] - 2025-11-05
### Engine Core [0.9.10]
- Upgraded Unity Version to 6.2.10f1
- Cleanup of empty folders left behind by previous builds

## [0.10.1-alpha] - 2025-10-28
### Engine Core [0.9.9]
- Upgraded Unity Version to 6.2.9f1

## [0.10.0-alpha] - 2025-10-18
### Battle Engine 
- Setting up the Battle Engine for defining the Battle System for use within the Game.

## [0.9.8-alpha] - 2025-10-18
### Engine Core [0.9.8]
- Preparing for BattleEngine Release
- Added a new File for the LoLEngine Version to manage the Battle Engine and the Core Engine versions.
- Updated ImprovedGameInitializer to add in the Version Details Printed
- Updated LoLLogger to add LogEmphasis

## [0.9.7] - 2025-10-18
- Upgraded unity Version to 6.2.8f1 

## [0.9.6] - 2025-10-03
- Upgraded to Unity Version 6.2.6f1 to take care of critical security bugs in the Unity Engine

## [0.9.5] - 2025-10-02
### Major Save System Overhaul
- **SECURITY**: Removed committed encryption keys from repository and added security warnings
- **ARCHITECTURE**: Consolidated duplicate configuration sources - encryption/compression settings now centralized in LoLEngineConfig
- **PRODUCTION**: Fixed save location hygiene - saves now properly use Application.persistentDataPath for builds
- **FLEXIBILITY**: Replaced hardcoded file extensions with dynamic detection from settings
- **SCENE-AWARE**: AutoSave service now excludes menu/boot/loading scenes with configurable scene filtering
- **API ENHANCEMENT**: Added comprehensive save management methods (ListSavesAsync, DeleteAsync, GetLatestAsync, HasAnySavesAsync)
- **VERSION SUPPORT**: Added save version validation and migration framework foundation
- **INTERFACE CONSISTENCY**: Clarified ISaveSystem vs IAutoSaveService naming patterns
### Added
- SaveFileInfo class for file system metadata (separate from game-specific SaveMetadata)
- Scene-based autosave filtering with configurable excluded scenes list
- SaveSettings.GetSaveFilePath() method ensuring proper persistent data paths
- SaveSettings.GetFileExtension() method for dynamic extension selection
- Atomic save file writes using temporary files for corruption prevention
- Version validation with placeholder migration system
- Enhanced SaveConfig focused on game-specific behavior (slots, intervals, UI settings)
### Changed
- SaveConfig no longer contains encryption/compression settings (moved to LoLEngineConfig)
- AutoSaveService constructor now uses SaveSettings parameter instead of float interval
- SaveSystem file detection now uses settings-based extensions instead of hardcoded patterns
- AutoSaveService now subscribes to scene changes for intelligent start/stop behavior
### Fixed
- Namespace conflicts between SaveMetadata classes resolved
- ServiceInitializer compatibility with new AutoSaveService constructor
- ConfigurableServiceInitializer proper dependency injection for AutoSaveService
- GameSaveData.GetDataVersion() method calls instead of non-existent DataVersion property
### Security
- Replaced production encryption keys with dev-only placeholders
- Added clear warnings about security practices in configuration tooltips
- Documented proper production key injection via environment variables

# [0.9.4] - 2025-09-30
### Added
- Audio: Resource-level preloading in `AudioOrchestratorService` using `IResourceService` with explicit retain/release of `AudioClip` assets.
- Audio: Budget-aware, concurrent preloading driven by `AudioPreloadProfile` (respects `memoryBudgetMB`, `maxConcurrentLoads`, and `loadingTimeoutSeconds`).
- Tests (EditMode): `AudioOrchestratorPreloadTests`
    - Budget respected when requests exceed memory budget
    - Concurrency is capped to configured limit
    - Slow loads time out and are skipped
### Changed
- Audio: Preloading no longer “plays at volume 0 then stops” (avoids `AudioSource` churn and mixer side effects). Assets are retained via resource service instead.
- Audio: Concurrency workers now run via awaited async tasks (no `Task.Run`) to keep Unity API calls on the main thread.
- Audio: Platform overrides for preloading are selected at runtime (`Application.platform`), and in Editor the enabled override (mobile or console) is honored for easier testing/tuning.
### Fixed
- Audio: Avoided background-thread Unity API usage during preloading (e.g., `AudioClip.Create`), preventing main-thread violations in tests and editor.
- Audio: Resolved ambiguous `PreloadRequest` type references by removing the symbol from orchestrator worker signatures.
- Tests: Aligned fake interfaces and service locator usage with current engine APIs to compile cleanly.

## [0.9.3-alpha] - 2025-09-29
### Added
- Robust fallback logic for SceneAudioManager when automatic music fails
  - Intelligent fallback from GameMusicMapConfig to manual music ID
  - Multi-layer fallback: AudioOrchestratorService → Basic AudioService
  - Clear error reporting with emoji indicators for easy debugging
### Changed
- **BREAKING**: AudioOrchestratorService.SetMusicContext now returns `Task<IAudioHandle>` instead of `Task`
  - Enables proper error detection and fallback logic
  - Returns null when music loading fails (instead of silently swallowing errors)
  - Updated IAudioOrchestratorService interface to match
### Fixed
- SceneAudioManager now properly handles invalid audio IDs in GameMusicMapConfig
  - No longer crashes the entire audio system on invalid Addressable keys
  - Provides clear warning messages and attempts fallback to manual music
  - Prevents silent failures that left users without audio
### Improved
- Enhanced error logging in AudioOrchestratorService with specific failure reasons
- Better debugging experience with clear success/warning/error indicators
- More resilient audio system that gracefully handles configuration mistakes

## [0.9.2-alpha] - 2025-09-29
### Added
- SceneAudioManager: Clean scene-based audio management component
  - Automatic music playback based on scene mappings in GameMusicMapConfig
  - Manual music override support with proper fallback handling
  - Graceful degradation from AudioOrchestratorService to basic AudioService
### Fixed
- AudioOrchestratorService: Fixed MusicMapConfig loading path to use game-specific configs
  - Now loads from `Configs/GameMusicMapConfig` instead of engine-internal path
  - Proper separation of engine framework from game-specific audio configurations
- AudioSourcePool: Fixed Unity warning when resetting AudioSource time without clip
  - Added null check before setting `audioSource.time = 0f`
  - Eliminates "Attempting to set 'time' on an audio source..." warningwarning
### Changed
- MusicMapConfig: Architecture improvement for game-specific audio configurations
  - Moved from LoLEngine internal folder to game project Resources folder
  - Enables per-game customization of scene-to-music mappings
- Reduced debug logging verbosity in ConfigurableServiceInitializer
  - Cleaner console output during service initialization
### Removed
- Debug components: SimpleAudioTester and AudioSourceDetector
  - Temporary debugging utilities removed after successful implementation

## [0.9.1-alpha] - 2025-09-25
- AudioOrchestratorService: New audio service implementing ILoLEngineService
  - Priority-based voice management with intelligent voice stealing policies (Oldest, Quietest, LowestPriority)   
  - Smart preloading with memory management for instant critical audio playback
- AudioPriorityConfig: ScriptableObject for voice limits, ducking rules, and platform overrides
  - Configurable category rules (Music, SFX, UI, Voice, Ambient)
- MusicMapConfig: Scene-to-music mapping with crossfade settings
  - Context-aware music switching (combat, exploration, menus)
- AudioPreloadProfile: Critical audio preloading profiles for loading screens
- SceneAudioPlayer: Simple drop-in component for basic audio needs
  - Auto-detects AudioOrchestratorService with graceful fallback to basic AudioService
- AdvancedAudioPlayer: Full-featured component for complex audio scenarios
  - Smart loading strategies (Always Async/Sync, PreloadThenSync, Smart)
- AudioPreloadManager: Singleton manager for audio asset preloading 
- ServiceConfiguration Integration: Added enableAudioOrchestrator toggle
- ConfigurableServiceInitializer : Automatic service registration via reflection
- SceneAudioManager: Clean scene-based audio management component
- Fixed AudioOrchestratorService: Fixed MusicMapConfig loading path to use game-specific configs
- AudioSourcePool: Fixed Unity warning when resetting AudioSource time without clip

## [0.9.0-alpha] - 2025-09-25 
- Assembly Architecture Redesign: Restructured assembly dependencies to eliminate circular references
- Namespace Changes: Moved core infrastructure classes to lower-level assemblies
    - LoLEngine.Helpers.Logger → LoLEngine.Runtime.Logger
    - LoLEngine.Helpers (ServiceAwaiter, IEngineInitializationProvider) → LoLEngine.Runtime
    - LoLEngine.Helpers.Extensions.ColorExtension → LoLEngine.Runtime.Extensions.ColorExtension
    - LoLEngine.Utility.LRUCache → LoLEngine.Runtime.Utility.LRUCache
- New Assembly Hierarchy:
  LoLEngine-Runtime (base)
  ├── LoLEngine-Core → Runtime
  ├── LoLEngine-Helpers → Runtime + Core
  ├── LoLEngine-Utility → Runtime + Core
  ├── LoLEngine-GameObjects → Runtime + Core + Helpers
  └── Tests → Multiple assemblies
    - Infrastructure Consolidation: Moved essential utilities (Logger, ServiceAwaiter, LRUCache, ColorExtension) to Runtime layer
    - Engine Color System: Enhanced SampleEventListener to use LoLEngineConstants.Colors for config-driven color management
- Logger Optimization: Replaced TimeExtension dependency with inline time formatting to eliminate external dependencies
- Cache Management: Cleared Unity build artifacts and assembly caches to prevent stale reference issues
- Namespace Consistency: Updated all using statements across codebase to reflect new assembly structure

## [0.8.3-alpha] - 2025-09-25
- Added SimpleAudiPlayer to helpers/audio for common audio scenarios. These are convenience utilities that wrap the Core AudioService not part of the Core framework itself

## [0.8.2-alpha] - 2025-09-24
- Upgrade Unity Version to Version 6.2.5f1

## [0.8.1-alpha] - 2025-09-16
Boot debug banner, production checklist, and build-time guard
Added
- Boot banner in `BootScreenController` when Debug Mode or Simulate Slow Loading are enabled
    - Prints configuration, build context, diagnostics status, and a production checklist
    - Logged on `LogChannel.Boot` at Warning level for visibility with Warning sensitivity
- Editor build validator blocks production builds with debug flags enabled
    - File: `Assets/LoLEngine/Editor/Build/BootDebugBuildValidator.cs`
    - Scans enabled scenes for Boot/HybridBootController instances; blocks on `DebugMode/SimulateSlowLoading`
    - Logs clear errors for each offending scene/object and a summary with fix steps
- Validator menu: `LoLEngine/Validate Production Readiness`
    - File: `Assets/LoLEngine/Editor/Build/BootDebugValidationMenu.cs`
    - Runs the same checks without starting a build, opens the first offending scene and selects the controller
Changed
- Banner severity raised to Warning and includes explicit production checklist
Fixed
- Editor validator no longer attempts to unload the last open scene; only closes scenes it opened

## [0.8.0-alpha] - 2025-09-14
- Added tests for AudioService
- Added the UIScreenAudioPlayer
- Unity Version upgraded to 6.2.4f1

## [0.7.3-alpha] - 2025-09-14
Ordering
- Initial attribute-based dependency annotations to demonstrate new optional topo-sorting
    - `AudioService` optionally depends on `IEventManager` and `IResourceService`
    - `LocalizationService` optionally depends on `IEventManager` and `IResourceService`
    - `UIService` optionally depends on `IEventManager`, `IObjectPoolService`, `IResourceService`, and `IInputService`
    - `AutoSaveService` depends on `ISaveSystem` (Feature phase; ensures `ISaveSystem` initializes first)
    - `QuickSaveService` depends on `ISaveSystem` (Feature phase)
    - `NotificationService` optionally depends on `IEventManager`
    - `GameStateManagerService` optionally depends on `IEventManager`
    - `SceneService` optionally depends on `IResourceService` and `IEventManager`
    - `JsonDataSerializer` optionally depends on `ICompressionService`
    - `AesEncryptionService` marked independent (uses LoLEngineConfig internally without ordering)
    - `SaveSystem` optionally depends on `ICompressionService` and `IEncryptionService` (in addition to requiring `IDataSerializer`)

## [0.7.2-alpha] - 2025-09-14
Startup Diagnostics, Boot UX polish, and docs refresh
Added
- Per‑service startup timing and health reporting
  - New `StartupDiagnostics` utility tracks init duration and success for each service
  - Summary report logged at boot completion under `LogChannel.Boot`
  - Optional on‑screen diagnostics display (toggle in `BootConfiguration`)
- Engine controller for games
  - New `HybridBootController` (inherits `BootScreenController`) for scene drop‑in
  - `BootScreenController` now exposes `onBeforeBootSequence` and `onAfterBootComplete` UnityEvents
- EditMode tests
  - `StartupDiagnosticsTests` verifies basic record and report
Changed
- Boot flow hardening
  - `BootScreenController` now null‑safe: proceeds without UI/config to avoid NREs
  - Auto‑loads default `BootConfiguration` from `Resources/Configs/DefaultBootConfiguration` if unassigned
  - Shows diagnostics report on boot screen when enabled (with status‑text fallback)
- Conditional compilation for zero‑cost release builds
  - Diagnostics compile only for `UNITY_EDITOR`, `DEVELOPMENT_BUILD`, or when `LOL_STARTUP_DIAGNOSTICS` is defined
Documentation
- BootSystem.md: “New Game Quick Start” with clear, no‑code setup using engine components
- BootMigration.md: Migration guide to consolidate game boot scripts into engine controllers
- README: Note about `LOL_STARTUP_DIAGNOSTICS` flag
Fixed
- Resolved NullReferenceExceptions during boot when UI or config references were missing
- Resolved preprocessor compile error by removing in‑file `#define` and using proper `#if` guards

## [0.7.1-alpha] - 2025-09-13
Testing Infrastructure Improvements
- **Relaxed Legacy Service Initialization**: Tests can now run with minimal configuration
- **Graceful Service Degradation**: Missing optional services log warnings instead of throwing
- **Improved Test Isolation**: Unit tests no longer fail due to unrelated service dependencies
- **Better Error Reporting**: Clear logging shows exactly which services are missing/skipped
Unity Editor Compatibility Fixes 
- **ObjectPoolService**: Added EditMode test support
- Skip `DontDestroyOnLoad()` in EditMode (not supported by Unity)
- Use `DestroyImmediate()` for proper cleanup in EditMode tests 
- Prevents test failures and memory leaks during unit testing
Service Locator Enhancements
- Added `TryGet<T>(out T, key)` for safe, no-throw lookups
- Added `Require<T>(key)` to fail fast with clear errors when mandatory
- `Get<T>()` now uses `TryGet<T>` internally for consistency
- New unit test: `ServiceLocatorBasicTests`
Serialization Robustness
- JSON: Null input no longer throws; returns `{}` (compressed if requested)
- Encrypted: Empty input short-circuits to null (no decryption attempt)
- AES: `DecryptString`/`DecryptStringAsync` handle empty input gracefully
Additional EditMode Compatibility
- **AudioService**: Guard `DontDestroyOnLoad` in play mode only
- **ResourcePool**: EditMode-safe initialization and cleanup
- **NotificationHelper**: Avoid `DontDestroyOnLoad` when not playing
Configuration
- Explicit defaults in `ServiceConfiguration.CreateDefault()` and `CreateMinimal()` to match docs

## [0.7.0-alpha] - 2025-09-12
- Upgraded vFolders
- Built BootSystem and added the BootSystem to the GameInitializer
- Added BootSystem to the Dependency Checker and Initializer
- Added BootSystem to the Service Locator
- Added BootSystem to the Service Configuration
- Upgraded to Unity Version 6.2.3f1
- Documentation Updated for README and CHANGELOG

## [0.6.4-alpha] - 2025-09-05
- Upgraded to Unity Version 6.2.2f1

## [0.6.3-alpha] - 2025-08-27
- Upgraded to Unity Version 6.2.1f1

## [0.6.2-alpha] - 2025-08-18
- Upgraded to Unity Version 6.2.0f1

## [0.6.1-alpha] - 2025-08-18
- Downgraded the Unity Version to Version 6.1.15f1 due to Unity Version Unresolved issue with the Addressables and UI
- Modified and added external Factory Services for Characters to prevent cyclical issues with Assembly references
- Introduced external Factory Service checking to the Dependency checker and initializer

## [0.6.0-alpha] - 2025-08-18
- Upgraded to Unity Version 6.2.0f1

## [0.5.2-alpha] - 2025-08-04
- Upgraded to latest Unity Version 6.1.41f

## [0.5.1-alpha] - 2025-07-08
- Fixed `ImprovedDependencyInjection` to look for the correct `ResourceManagementConfig` instance
- Fixed The issue is that `DontDestroyOnLoad(gameObject)` is being called on a GameObject that's not at the root level.
- Fixed the misleading warning message. The warning now correctly indicates that system language is being used because useSystemLanguage is enabled in the
  config.

## [0.5.0-alpha] - 2025-07-07

- Upgraded to Unity 2025.1.0f1

### Critical Thread Safety & Memory Management Fixes
- Eliminated dangerous lazy evaluation in `AudioService` that could cause deadlocks during service initialization
- Implemented `ReaderWriterLockSlim` for better concurrent access performance and fixed double-checked locking issues
- Added comprehensive handle tracking and release mechanisms to prevent memory leaks and Addressables system instability
- Added proper memory barriers in `ServiceLocator` singleton pattern for thread-safe initialization
- All Addressables `AsyncOperationHandle` objects are now properly tracked and released
- Improved shutdown procedures with automatic handle cleanup in `ResourceService` and `AddressableResourceLoader`

### Bug Fixes
- **AudioService**: Replaced `Lazy<T>` evaluation with direct dependency injection during Initialize()
- **ServiceLocator**: Fixed race conditions in service registration and retrieval
- **ResourceService**: Added thread-safe AssetReference handle tracking
- **AddressableResourceLoader**: Implemented proper handle-to-asset mapping with safe cleanup

### Monitoring & Debugging
- Added methods to monitor active Addressables handles for debugging
- Enhanced ResourceStats to include handle tracking information
- All critical sections now use appropriate locking mechanisms

## [0.4.4-preview] - 2025-07-03
- Fixed warnings when using the LoL Engine with a new Project. No more warnings about missing services.
- Added `ServiceAwaiter` to the Helpers namespace for better service initialization handling.

## [0.4.3-preview] - 2025-06-25
- Fixed Critical issue where Some of the Services were being accessed before they were intitilized, causing a Null Pointer issue sometimes
- Upgraded to Version 9f1 for Unity
- Fixed issue with Asynch and synch mismatches causing the project to hang when running
- Introduced `ServiceAwaiter` in the Helpers

## [0.4.1-preview] - 2025-06-23
- **Addressable Support**: AssetReference vs string-based loading

## [0.4.0-preview] - 2025-06-23
- **Addressable Support**: Added support for Addressables in ResourceManager
- Updated **AudioService** to use Addressables for audio asset loading

## [0.3.2-preview] - 2025-06-22
- **GameObject**: New Character and Non Playable Character Extendable classes added,
  - Bug Fixes
  - Conflict Fixes
- **Unity Version**: Upgraded to 6.1.8f1

## [0.3.1-preview] - 2025-06-11

### Major Features Added
- **Audio System Overhaul**: Complete rewrite of audio management system
- **Comprehensive Character System**: Added a reusable and extensible character framework.
  - Base character classes (`Character`, `PlayerCharacter`, `NonPlayableCharacter`).
  - Component-based design for Health, Stats, and other character aspects.
  - `CharacterFactory` for streamlined creation of different character types (Player, Enemy, NPC).
  - Robust Data Persistence integration (`PersistableCharacterComponent`, `CharacterPersistenceManager`, specialized `CharacterSaveData`).
  - Support for object pooling of characters via `CharacterFactory`.
  - Interface-driven design (`IPersistableCharacter`, `ICombatant`, `IDamageable`, `ICharacterAI`, `IInteractable`, `IMovable`) for flexibility.
  - Hooks for AI and custom behaviors.

### Architecture Improvements
- **Service Locator Pattern**: Implemented comprehensive dependency injection
- **ImprovedGameInitializer**: New reflection-based service initialization system
- **ConfigurableServiceInitializer**: ScriptableObject-based service configuration
- **Enhanced Event System**: Type-safe centralized event handling
- **Resource Management**: Unified interface supporting Resources and Addressables

### Performance Optimizations
- **Object Pooling**: AudioSource pooling for reduced garbage collection
- **Coroutine-based Cleanup**: Eliminated performance-heavy Update() loops
- **Smart Cache Management**: Automatic cache invalidation and memory management
- **Optimized Inspector Repainting**: Intelligent UI update throttling

### Technical Improvements
- **Static Cache Safety**: Domain reload handling and stale reference prevention
- **Memory Leak Prevention**: Comprehensive event unsubscription and cleanup
- **Async Resource Loading**: Non-blocking asset loading with reflection fallbacks
- **Mixer Parameter Validation**: Automatic AudioMixer parameter checking

### Bug Fixes
- Fixed NullReferenceException during AudioService shutdown
- Resolved static cache stale reference issues in AudioExtensions
- Fixed double-disposal problems in audio handle management
- Corrected synchronous blocking issues during initialization
- Fixed memory leaks in event subscription management

### Documentation
- **Comprehensive README**: Installation, usage, and API documentation
- **Code Comments**: Extensive inline documentation for all public APIs
- **Best Practices**: Performance and maintainability guidelines

### Developer Experience
- **Custom Inspectors**: Runtime debugging tools for audio system
- **Assembly Definitions**: Proper module separation and compilation optimization
- **Test Coverage**: Comprehensive unit and integration tests
- **Error Handling**: Detailed logging and graceful fallback mechanisms


## [0.2.0-alpha] - 2025-06-09

### Major Features Added
- **Audio System Overhaul**: Complete rewrite of audio management system
  - Advanced AudioService with object pooling and 3D audio support
  - AudioMixer integration with parameter validation
  - Asynchronous audio loading with fallback mechanisms
  - Comprehensive audio handle management with memory leak prevention
  - Custom AudioServiceInspector for runtime debugging
  - Performance optimizations: replaced Update() loops with coroutines

## [0.1.18-alpha] - Previous Version

### Features
- Basic service locator implementation
- Initial audio system foundation
- Resource management framework
- Core utility classes

---

## Version Naming Convention

- **Major.Minor.Patch-prerelease**
- **Major**: Breaking changes
- **Minor**: New features, backward compatible
- **Patch**: Bug fixes, backward compatible
- **Prerelease**: alpha, beta, rc

## Migration Guide

### From 0.1.x to 0.2.x
1. **GameInitializer → ImprovedGameInitializer**: Replace old initialization system
2. **Service Configuration**: Create ServiceConfiguration assets for service toggles
3. **Audio API**: Update to new audio extension methods and interfaces
4. **Event System**: Migrate to type-safe event handling pattern
