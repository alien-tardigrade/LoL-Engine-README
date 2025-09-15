# Changelog

All notable changes to the LoL Engine package will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
