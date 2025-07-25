# Changelog

All notable changes to the LoL Engine package will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.1-alpha] - 2025-07-08
- Fixed `ImprovedDependencyInjection` to look for the correct `ResourceManagementConfig` instance
- Fixed The issue is that `DontDestroyOnLoad(gameObject)` is being called on a GameObject that's not at the root level.
- Fixed the misleading warning message. The warning now correctly indicates that system language is being used because `useSystemLanguage` is enabled in the config.

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

### 🎉 Major Features Added
- **Audio System Overhaul**: Complete rewrite of audio management system
- **Comprehensive Character System**: Added a reusable and extensible character framework.
  - Base character classes (`Character`, `PlayerCharacter`, `NonPlayableCharacter`).
  - Component-based design for Health, Stats, and other character aspects.
  - `CharacterFactory` for streamlined creation of different character types (Player, Enemy, NPC).
  - Robust Data Persistence integration (`PersistableCharacterComponent`, `CharacterPersistenceManager`, specialized `CharacterSaveData`).
  - Support for object pooling of characters via `CharacterFactory`.
  - Interface-driven design (`IPersistableCharacter`, `ICombatant`, `IDamageable`, `ICharacterAI`, `IInteractable`, `IMovable`) for flexibility.
  - Hooks for AI and custom behaviors.

### 🏗️ Architecture Improvements
- **Service Locator Pattern**: Implemented comprehensive dependency injection
- **ImprovedGameInitializer**: New reflection-based service initialization system
- **ConfigurableServiceInitializer**: ScriptableObject-based service configuration
- **Enhanced Event System**: Type-safe centralized event handling
- **Resource Management**: Unified interface supporting Resources and Addressables

### ⚡ Performance Optimizations
- **Object Pooling**: AudioSource pooling for reduced garbage collection
- **Coroutine-based Cleanup**: Eliminated performance-heavy Update() loops
- **Smart Cache Management**: Automatic cache invalidation and memory management
- **Optimized Inspector Repainting**: Intelligent UI update throttling

### 🛠️ Technical Improvements
- **Static Cache Safety**: Domain reload handling and stale reference prevention
- **Memory Leak Prevention**: Comprehensive event unsubscription and cleanup
- **Async Resource Loading**: Non-blocking asset loading with reflection fallbacks
- **Mixer Parameter Validation**: Automatic AudioMixer parameter checking

### 🐛 Bug Fixes
- Fixed NullReferenceException during AudioService shutdown
- Resolved static cache stale reference issues in AudioExtensions
- Fixed double-disposal problems in audio handle management
- Corrected synchronous blocking issues during initialization
- Fixed memory leaks in event subscription management

### 📚 Documentation
- **Comprehensive README**: Installation, usage, and API documentation
- **Code Comments**: Extensive inline documentation for all public APIs
- **Best Practices**: Performance and maintainability guidelines

### 🔧 Developer Experience
- **Custom Inspectors**: Runtime debugging tools for audio system
- **Assembly Definitions**: Proper module separation and compilation optimization
- **Test Coverage**: Comprehensive unit and integration tests
- **Error Handling**: Detailed logging and graceful fallback mechanisms


## [0.2.0-alpha] - 2025-06-09

### 🎉 Major Features Added
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
