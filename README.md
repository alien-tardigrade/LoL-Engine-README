# LoL Engine - Labour of Love Game Framework

[![Unity Version](https://img.shields.io/badge/Unity-6000.0%2B-blue.svg)]() 
[![Version](https://img.shields.io/badge/Version-1.0.0--rc.5-gold.svg)](CHANGELOG.md)
[![License](https://img.shields.io/badge/License-green.svg)](LICENSE.md)

| Project       |                                                                                Version | Notes                                             |
|---------------|---------------------------------------------------------------------------------------:|---------------------------------------------------|
| LoL Core      | [![Version](https://img.shields.io/badge/Version-1.0.0--rc.5-gold.svg)](CHANGELOG.md) | Core engine systems (Event, Resource, Pool, Time, RNG, ModelDb, LocString, SaveSystem, TypedPoolBag, AbstractOdds) |

A comprehensive Unity game development framework providing essential systems for modern game development. Built with performance, modularity, and ease of use in mind.

## Overview

The LoL Engine provides a suite of interconnected services and tools designed to accelerate game development in Unity. It emphasizes a clean architecture through a service locator pattern, data-driven configuration, and robust implementations of common game systems.

## Features

### **Enhanced Addressables Integration**
*   **✨ AssetReference Support**: Type-safe asset loading with Unity's AssetReference system
*   **Seamless Asset Loading**: Automatic Addressables support with Resources fallback
*   **No Code Changes**: Existing ResourceService calls automatically benefit from Addressables
*   **Performance**: Async-first loading, better memory management, and remote content support
*   **Future-Proof**: Ready for downloadable content and asset streaming

*   **Service Management:**
    *   `Service Locator`: Centralized dependency management.
    *   `ImprovedGameInitializer`: Orchestrates engine startup.
    *   `Configuration System`: Highly flexible setup using `ServiceConfiguration` and `ResourcePathConfig` ScriptableObjects to enable/disable services and link their specific configurations.
*   **Essential Game Systems:**
    *   `Audio System`: Advanced audio playback, mixing, 3D spatialization, track management, and `AudioSource` pooling. ([Docs](Documentation~/Audio.md))
    *   `Resource Management`: Unified asset loading with **first-class Addressables support**, Resources fallback, and object pooling integration. ([Docs](Documentation~/ResourceManagement.md))
    *   `Object Pooling`: High-performance pooling for GameObjects and components. ([Docs](Documentation~/ObjectPool.md))
    *   `Event System`: Type-safe, centralized event handling with automatic listener management. ([Docs](Documentation~/Events.md))
    *   `Data Persistence`: Robust save/load system with slots, async operations, compression, encryption, **per-domain schema migration**, **corrupt-file quarantine**, and **pluggable `ISaveStore` backends** (local file + in-memory test store). ([Docs](Documentation~/DataPersistence.md))
    *   `Localization System`: Comprehensive multi-language support with dynamic switching. ([Docs](Documentation~/Localization.md))
    *   `Input`: Use Unity's Input System package directly in game code ([Docs](Documentation~/Input.md)); no engine `IInputService`.
*   **Determinism & Content:**
    *   `Deterministic RNG` (`IRngService`): Named, independent random streams seeded from a single root seed. Reproducible replays, testable outcomes, IL2CPP-safe. Snapshot/restore for save-game resume.
    *   `ModelDb`: Static string-keyed content registry (`ModelId → canonical object`). IL2CPP/AOT-safe explicit registration. Thread-safe reads post-bootstrap.
    *   `AbstractModel`: Canonical/mutable boundary base class. Prevents silent template mutation. `AssertMutable()` / `AssertCanonical()` / `MutableClone()` enforce the template-vs-instance split.
    *   `LocString` + `DynamicVar`: Structured localized text with runtime variable substitution. Tokens `{varName}` / `{varName:specifier}` resolved at display time. Custom formatters via `ILocStringFormatter`. Upgrade-preview support and CI validation via `LocValidator`.
    *   `TestMode`: Static flag + assertion helpers for automated test runs. RNG override injection (Editor/test builds) lets tests force specific roll outcomes.
*   **Utilities & Advanced:**
    *   `TypedPoolBag<TKey, TItem>`: Genre-neutral typed pool with per-bucket shuffle, `IsAllowed` filter, automatic cascade on exhaustion, and loud `PoolExhaustedException` on full depletion. Deterministic with injected `Rng`.
    *   `AbstractOdds<TOutcome>` / `AntiPityOdds<T>`: Probabilistic systems as first-class objects with persistent `CurrentValue` counters. Anti-pity mechanics (pity target weight grows on miss, resets on hit) with full save/load round-trip support.
    *   `Time Management`: Pause, slow-motion, and custom time scaling.
    *   `Notification System`: In-game messaging and alerts.
    *   Encryption & Compression services.
    *   Various helper classes and extension methods. 

Diagnostics build flag
- Per-service startup diagnostics compile in Editor/Development builds by default and can be enabled in Release with `LOL_STARTUP_DIAGNOSTICS`.

Service dependency ordering
- The engine supports optional, attribute-based dependencies with a topological sort within phases (Core → Feature → Game → Custom).
- Add `[ServiceDependency(typeof(IMyDependency), Optional = true/false)]` on concrete service classes to hint ordering.
- Missing/optional or cross-phase dependencies are ignored; cycles fall back to manual order with a warning.
- See Documentation~/ServiceDependencies.md for details and examples.

Independent services
- Core: `EventService`, `ObjectPoolService`, `TimeService`, `ResourceService`, `CoroutineRunner`
- Feature: `CompressionService`, `AesEncryptionService` (uses `LoLEngineConfig` internally)

## Requirements

- **Unity**: 6000.0 or later

### Platform Support

| Platform | Status |
|---|---|
| Windows / macOS / Linux (Mono & IL2CPP) | ✅ Supported |
| iOS / Android (IL2CPP) | ✅ Supported — read [Documentation~/IL2CPP.md](Documentation~/IL2CPP.md) before shipping (managed code stripping) |
| WebGL | ❌ Not supported in 1.0 (async save/load and the file log sink use background threads, which WebGL lacks) |

The package ships a root-level `link.xml` that protects engine assemblies from IL2CPP
managed code stripping. Game-side `IServiceRegistrar` implementations and string-named
custom services need their own `[Preserve]` — see [Documentation~/IL2CPP.md](Documentation~/IL2CPP.md).

### Required Dependencies (declared in `package.json`)

| Package | Version | Purpose |
|---|---|---|
| `com.unity.addressables` | 2.5.0+ | Asset loading via `IResourceService` |
| `com.unity.nuget.newtonsoft-json` | 3.2.1+ | JSON serialization for `ISaveSystem` |

### Optional Dependencies (graceful degradation)

| Package | If installed | If absent |
|---|---|---|
| `com.unity.inputsystem` | Use in game code with `InputActionAsset` / `PlayerInput` | Not required for engine init or bundled demo scenes |
| `com.unity.textmeshpro` | Sample scene UI uses TMP labels | Falls back to legacy `UnityEngine.UI.Text` |

> ℹ️ The compile-time symbol `ENABLE_INPUT_SYSTEM` (defined automatically by Unity when the Input System package is present) gates input-related code.

## Installation

### From Unity Package Manager (Recommended)
1.  Open **Window > Package Manager**.
2.  Add LoL Engine from the release source provided for this UPM package.
3.  Confirm the package appears as **LoL Engine** in Package Manager.

### From Local Disk (For Development/Testing)
1.  Download or clone the package repository to your local machine.
2.  Open Unity Package Manager (`Window > Package Manager`).
3.  Click the `+` button and select `Add package from disk...`.
4.  Navigate to the root folder of the cloned package (the one containing `package.json`) and select it.

## Quick Start

1.  **Initial Engine Setup:**
    *   **Choose Your Configuration Approach:**
        *   **Option A - Use Default Configuration (Recommended for most games):**
            *   In code: `var config = ServiceConfiguration.CreateDefault();`
            *   **Includes:** All core services + common game features (Audio, Input, Saves, Localization)
            *   **Perfect for:** Production games with standard features
        *   **Option A2 - Use Minimal Configuration (For testing/prototypes):**
            *   In code: `var config = ServiceConfiguration.CreateMinimal();`
            *   **Includes:** Core services only (EventManager, ResourceService, ObjectPool, TimeService)
            *   **Perfect for:** Testing, prototypes, specialized applications
        *   **Option B - Create Custom Configuration:**
            *   Right-click → `Create` → `LoLEngine` → `Configuration` → `Service Configuration`
            *   Name it (e.g., "MyGame_ServiceConfig")
            *   Manually configure which services to enable
    *   Create a `ResourcePathConfig` asset:
        *   Right-click → `Create` → `LoLEngine` → `Configuration` → `Resource Path Config`
        *   Name it (e.g., "MyGame_ResourcePaths")
    *   In your first scene (e.g., a "Boot" or "Initialization" scene), create an empty GameObject (e.g., "EngineInitializer")
    *   Add the `ImprovedGameInitializer` component to this GameObject
    *   Assign your `ServiceConfiguration` and `ResourcePathConfig` assets to the respective fields
    *   Create and assign service-specific config assets (e.g., `AudioConfig`, `SaveConfig`) as needed by your enabled services
    *   **Optional - Enable Addressables** (Recommended):
        *   Go to `Window > Asset Management > Addressables > Groups`
        *   Click "Create Addressables Settings" if prompted
        *   Mark your assets as Addressable and set meaningful addresses (e.g., "Audio/BackgroundMusic01")
        *   In your `ResourceManagementConfig`, set `enableAddressables = true`

2.  **Example: Setting Up Default Configuration:**
    ```csharp
    using LoLEngine.Core.ServiceManagement.Initializer;
    using UnityEngine;

    public class MyGameSetup : MonoBehaviour
    {
        [Header("Engine Configuration")]
        public ImprovedGameInitializer gameInitializer;
        
        void Awake()
        {
            // Option A: Use default configuration
            var defaultConfig = ServiceConfiguration.CreateDefault();
            
            // Option B: Customize from default
            var customConfig = ServiceConfiguration.CreateDefault();
            customConfig.enableLocalizationService = false; // Disable for single-language game
            
            // Assign to game initializer (or create ResourcePathConfig similarly)
            gameInitializer.ServiceConfiguration = defaultConfig;
        }
    }
    ```

3.  **Accessing Services:**
    Once initialized, services can be accessed via the `ServiceLocator`:
    ```csharp
    using LoLEngine.Core.ServiceManagement.Service;
    using LoLEngine.Core.Audio.Interfaces; // Example for Audio

    public class MyGameComponent : MonoBehaviour
    {
        void Start()
        {
            IAudioService audioService = ServiceLocator.Instance.Get<IAudioService>();
            if (audioService != null)
            {
                // Use the service
            }
        }
    }
    ```

4.  **Basic Audio Usage:**
    (AudioService is enabled by default in `ServiceConfiguration.CreateDefault()` - just ensure an `AudioConfig` is created and assigned.)
    ```csharp
    using LoLEngine.Core.Audio.Extensions;
    using LoLEngine.Core.Audio.Data; // For PlayOptions

    // Simple 2D sound playback from a MonoBehaviour
    this.PlaySound("UI_Click");

    // 3D positioned audio
    this.PlaySoundAtPosition("Explosion", transform.position);

    // With custom options
    var options = new PlayOptions
    {
        Volume = 0.8f,
        Loop = true,
        FadeInDuration = 0.5f
    };
    this.PlaySound("BackgroundMusic", options);
    ```
5.  **Resource Loading with Addressables:**
    (ResourceService is enabled by default in `ServiceConfiguration.CreateDefault()`.)
    
    **String-Based Loading (Traditional):**
    ```csharp
    using LoLEngine.Core.ServiceManagement.Service;
    using LoLEngine.Core.ResourceManagement.Interfaces;
    using UnityEngine; // For AudioClip
    using System.Threading.Tasks;

    public class AssetLoader : MonoBehaviour
    {
        async Task LoadMyAssets()
        {
            IResourceService resourceService = ServiceLocator.Instance.Get<IResourceService>();
            
            // Automatic Addressables loading (if asset is marked Addressable)
            // Falls back to Resources if not found in Addressables
            AudioClip audioClip = await resourceService.LoadAsync<AudioClip>("Audio/BackgroundMusic01");
            
            // Synchronous loading (use with caution - Addressables will warn)
            AudioClip syncClip = resourceService.Load<AudioClip>("Audio/UIClick");

            // The service automatically chooses: Addressables -> Resources -> AssetBundles
            if (audioClip != null)
            {
                // Use audioClip - loaded via best available method
            }
        }
    }
    ```
    
    **✨ NEW: AssetReference Loading (Recommended - Type-Safe):**
    ```csharp
    using UnityEngine;
    #if UNITY_ADDRESSABLES
    using UnityEngine.AddressableAssets;
    #endif

    public class TypeSafeAssetLoader : MonoBehaviour
    {
        [Header("Drag assets here in Inspector")]
        #if UNITY_ADDRESSABLES
        [SerializeField] private AssetReference musicAssetRef;
        [SerializeField] private AssetReference playerPrefabRef;
        #endif

        async Task LoadAssetsWithReferences()
        {
            IResourceService resourceService = ServiceLocator.Instance.Get<IResourceService>();
            
            #if UNITY_ADDRESSABLES
            // Type-safe loading with compile-time validation
            if (musicAssetRef != null && musicAssetRef.RuntimeKeyIsValid())
            {
                AudioClip music = await resourceService.LoadAsync<AudioClip>(musicAssetRef);
                // Use music...
            }
            // Cleanup when done
            resourceService.Release(musicAssetRef);
            #endif
        }
    }
    ```

6.  **Event System:**
    (EventManager is enabled by default in `ServiceConfiguration.CreateDefault()`.)
    ```csharp
    // Define an event
    public class PlayerScoreChangedEvent : LoLEngine.Core.Events.GameEvent
    {
        public int NewScore { get; set; }
    }

    // Listener
    using LoLEngine.Core.Events.Interfaces; // For IEventListener
    using LoLEngine.Core.Events.Extensions; // For EventStartListening
    using UnityEngine;

    public class ScoreDisplay : MonoBehaviour, IEventListener<PlayerScoreChangedEvent>
    {
        void OnEnable() => this.EventStartListening<PlayerScoreChangedEvent>();
        void OnDisable() => this.EventStopListening<PlayerScoreChangedEvent>();

        public void OnGameEvent(PlayerScoreChangedEvent eventData)
        {
            Debug.Log($"Score changed to: {eventData.NewScore}");
            // Update UI text
        }
    }

    // Triggering an event
    // GameEvent.Trigger(new PlayerScoreChangedEvent { NewScore = 100 }); // If GameEvent.Initialize was called
    // OR
    // ServiceLocator.Instance.Get<IEventManager>().TriggerEvent(new PlayerScoreChangedEvent { NewScore = 100 });
    ```

## Configuration Deep Dive

The LoL Engine's flexibility comes from its data-driven configuration:

### Service Configuration Presets

The engine provides three built-in service configuration presets:

#### 1. **Default Configuration (Recommended)**
```csharp
var config = ServiceConfiguration.CreateDefault();
```
**Enabled Services:**
- **Core Services:** EventManager, ResourceService, ObjectPool, TimeService
- **Feature Services:** AudioService, LocalizationService, CompressionService, EncryptionService, SerializationService, DataPersistence, NotificationService, AutoSaveService
- **Game Services:** GameStateManager

**Disabled Services:** AssetUpdater, QuickSaveService

*Perfect for most production games requiring common features like audio, saves, and localization.*

#### 2. **Minimal Configuration**
```csharp
var config = ServiceConfiguration.CreateMinimal();
```
**Enabled Services:**
- **Core Services Only:** EventManager, ResourceService, ObjectPool, TimeService

**All Other Services:** Disabled

*Ideal for testing, prototypes, or specialized applications with minimal overhead.*

#### 3. **Custom Configuration**
Create your own configuration by starting with a preset and modifying:
```csharp
var config = ServiceConfiguration.CreateDefault();
config.enableLocalizationService = false; // Single language game
```

### Configuration Components

1.  **`ImprovedGameInitializer`**: The entry point in your scene. Requires a `ServiceConfiguration` and `ResourcePathConfig`.
2.  **`ServiceConfiguration` (ScriptableObject)**:
    *   The central hub for enabling/disabling engine services.
    *   Contains fields to link to specific configuration assets for each service (e.g., `AudioConfig`, `SaveConfig`, `LocalizationConfig`).
3.  **`ResourcePathConfig` (ScriptableObject)**:
    *   Defines default paths for loading resources and configurations if they are not directly assigned or need to be found dynamically.
4.  **Service-Specific Configs (ScriptableObjects)**:
    *   Examples: `AudioConfig`, `SaveConfig`, `LocalizationConfig`.
    *   These assets hold detailed settings for their respective services. Create them via the `Assets/Create/LoLEngine/...` menu and assign them to your `ServiceConfiguration`.

### Service Dependencies

The engine automatically validates service dependencies:
- **DataPersistence** requires **SerializationService**
- **AutoSaveService** requires **DataPersistence**
- **EncryptedJsonDataSerializer** requires **EncryptionService**

### Quick Reference: Default Service Status

| Service Category | Service Name | Default | Minimal | Purpose |
|---|---|---|
| **Core Services** | EventManager | ✅ | ✅ | Event handling system |
|  | ResourceService | ✅ | ✅ | Asset loading and management |
|  | ObjectPool | ✅ | ✅ | GameObject recycling |
|  | TimeService | ✅ | ✅ | Time management and scaling |
| **Feature Services** | AudioService | ✅ | ❌ | Sound and music playback |
|  | LocalizationService | ✅ | ❌ | Multi-language support |
|  | SerializationService | ✅ | ❌ | JSON data serialization |
|  | DataPersistenceService | ✅ | ❌ | Save/load functionality |
|  | CompressionService | ✅ | ❌ | Data compression |
|  | EncryptionService | ✅ | ❌ | Data encryption |
|  | NotificationService | ✅ | ❌ | In-game notifications |
|  | AutoSaveService | ✅ | ❌ | Automatic saving |
| **Game Services** | GameStateManager | ✅ | ❌ | Game state management |
|  | QuickSaveService | ❌ | ❌ | F5/F9 quick save/load |

### Customizing Service Configuration

```csharp
// Start with defaults and customize
var config = ServiceConfiguration.CreateDefault();

// Disable services you don't need
config.enableLocalizationService = false;  // Single language game
config.enableAutoSaveService = false;      // Manual saves only
```

### When to Use Which Configuration

**Use `CreateDefault()`** when:
- Building a production game
- Need common features (audio, saves, input, localization)
- Building a complete game experience

**Use `CreateMinimal()`** when:
- Writing unit tests
- Building prototypes
- Creating specialized tools
- Need minimal memory footprint
- Testing individual systems

**Use Custom Configuration** when:
- You have specific requirements
- Building editor tools or specialized applications
- Need to optimize for specific platforms
- Want fine-grained control over every service

Refer to the detailed documentation for each service (linked in the Features section) for specifics on their configuration assets.


## Documentation

- **[Third Party Notices](<Third Party Notices.md>)** — Unity package dependencies, optional packages, and sample attributions
- **[Input](Documentation~/Input.md)** — Unity Input System in game code (no engine `IInputService`)

### Core Architecture
The framework uses a **Service Locator Pattern** with dependency injection:

- **Services**: Implement `ILoLEngineService` interface
- **Initialization**: Managed through `ServiceConfiguration` ScriptableObjects
- **Access**: Via `ServiceLocator.Instance.Get<TService>()`

### Key Services
- `IAudioService`: Audio playback and management
- `IResourceService`: Asset loading and caching
- `IEventManager`: Event handling and dispatching
- `IObjectPoolService`: Object pooling and recycling
- `ILocalizationService`: Multi-language support
- `IDataPersistance`: Data Persistance

- `IEncryptionService` : Encryption Service
- `IGameStateManagerService` : Game State Service
- `INotificationService` : Notification Service
- `IDataSerializer` : Serialization Service

### Configuration
Services are configured via ScriptableObject assets in `Resources/Configs/`:
- `ResourcePathConfig`: Resource Configurations
- `ServiceConfiguration`: Controls which services are enabled
- `DefaultAudioConfig`: Audio system settings
- `DefaultLocalizationConfig`: Localization settings
- `DefaultResourceManagementConfig`: Resource Management Settings (includes Addressables configuration)
- `DefaultSaveConfig`: Save Load Settings

## Next Steps

➡️ **New here? Read [`Documentation~/01_QUICKSTART.md`](Documentation~/01_QUICKSTART.md)** — a 5-minute walkthrough that takes you from install to a working sound playing.

After the quickstart, the documentation index ([`Documentation~/README.md`](Documentation~/README.md)) covers every system in depth.

## Support

- **Website**: [https://moomoo.games](https://moomoo.games)
- **Email**: alien.tardigrade@gmail.com
- **Documentation**: [Documentation~/README.md](Documentation~/README.md)

---

*Made with ❤️ by MooMoo Games*
