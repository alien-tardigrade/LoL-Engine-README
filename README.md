# LoL Engine - Labour of Love Game Framework

[![Unity Version](https://img.shields.io/badge/Unity-6000.2%2B-blue.svg)]() 
[![Version](https://img.shields.io/badge/Version-0.9.0--alpha-gold.svg)](CHANGELOG.md)
[![License](https://img.shields.io/badge/License-green.svg)](LICENSE.md)

A comprehensive Unity game development framework providing essential systems for modern game development. Built with performance, modularity, and ease of use in mind.

## 📖 Overview

The LoL Engine provides a suite of interconnected services and tools designed to accelerate game development in Unity. It emphasizes a clean architecture through a service locator pattern, data-driven configuration, and robust implementations of common game systems.

## 🚀 Features

### 🎯 **NEW: Boot Screen System**
*   **Customizable Boot Screens**: Professional loading screens with progress bars, status text, and animated spinners
*   **Service Integration**: Tracks engine initialization progress and provides visual feedback
*   **Flexible Configuration**: ScriptableObject-based configuration for timing, UI elements, and transitions
*   **Scene Management**: Automatic scene transitions after boot completion with fade effects
*   **Error Handling**: Graceful error display and recovery mechanisms
*   **Performance Monitoring**: Debug tools for tracking boot times and optimization

### 🎯 **Enhanced Addressables Integration**
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
    *   `Boot Screen System`: Professional loading screens with progress tracking and scene transitions. ([Docs](Documentation~/BootSystem.md))
    *   `Audio System`: Advanced audio playback, mixing, 3D spatialization, track management, and `AudioSource` pooling. ([Docs](Documentation~/Audio.md))
    *   `Resource Management`: Unified asset loading with **first-class Addressables support**, Resources fallback, and object pooling integration. ([Docs](Documentation~/ResourceManagement.md))
    *   `Object Pooling`: High-performance pooling for GameObjects and components. ([Docs](Documentation~/ObjectPool.md))
    *   `Event System`: Type-safe, centralized event handling with automatic listener management. ([Docs](Documentation~/Events.md))
    *   `Data Persistence`: Robust save/load system with slots, async operations, compression, and encryption. ([Docs](Documentation~/DataPersistence.md))
    *   `Localization System`: Comprehensive multi-language support with dynamic switching. ([Docs](Documentation~/Localization.md))
    *   `Scene Management`: Flexible scene loading, transitions, and management. ([Docs](Documentation~/SceneManagement.md))
    *   `Input System`: Unified input handling for various devices. ([Docs](Documentation~/Input.md))
    *   `UI System`: Structured approach for managing UI screens and dialogs. ([Docs](Documentation~/UI.md))
*   **Utilities & Advanced:**
    *   `Time Management`: Pause, slow-motion, and custom time scaling.
    *   `Notification System`: In-game messaging and alerts.
    *   Encryption & Compression services.
    *   Various helper classes and extension methods. 

Diagnostics build flag
- Per-service startup diagnostics compile in Editor/Development builds by default and can be enabled in Release with `LOL_STARTUP_DIAGNOSTICS`.
- See Boot Screen docs for usage and boot UI display toggle.

Service dependency ordering
- The engine supports optional, attribute-based dependencies with a topological sort within phases (Core → Feature → Game → Custom).
- Add `[ServiceDependency(typeof(IMyDependency), Optional = true/false)]` on concrete service classes to hint ordering.
- Missing/optional or cross-phase dependencies are ignored; cycles fall back to manual order with a warning.
- See Documentation~/ServiceDependencies.md for details and examples.

Independent services
- Core: `EventService`, `ObjectPoolService`, `TimeService`, `ResourceService`, `CoroutineRunner`
- Feature: `CompressionService`, `AesEncryptionService` (uses `LoLEngineConfig` internally)
- Game: `BootScreenService`

## 📋 Requirements

- **Unity**: 6000.0 or later
- **Dependencies**: 
    *   `com.unity.inputsystem` 
    *   `com.unity.nuget.newtonsoft-json`
    *   `com.unity.addressables` (2.5.0+) - Integrated for advanced asset loading 

## 🛠️ Installation

### From Local Disk (For Development/Testing)
1.  Download or clone the package repository to your local machine.
2.  Open Unity Package Manager (`Window > Package Manager`).
3.  Click the `+` button and select `Add package from disk...`.
4.  Navigate to the root folder of the cloned package (the one containing `package.json`) and select it.

## 🚀 Quick Start

1.  **Initial Engine Setup:**
    *   **Choose Your Configuration Approach:**
        *   **Option A - Use Default Configuration (Recommended for most games):**
            *   In code: `var config = ServiceConfiguration.CreateDefault();`
            *   **Includes:** All core services + common game features (Audio, Input, Saves, Localization, BootScreen)
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
    *   **Optional - Enable Boot Screen** (Recommended for professional games):
        *   In your `ServiceConfiguration`, enable `Enable Boot Screen`
        *   Create a `BootConfiguration` asset: Right-click → `Create` → `LoLEngine` → `Boot Configuration`
        *   Set up a boot scene with `BootScreenController` and `BootScreenUI` components
        *   Configure timing, progress weights, and target scene in the BootConfiguration
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
            customConfig.enableUIService = true;           // Enable UI management
            customConfig.enableSceneService = true;        // Enable scene management
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
    using LoLEngine.Scripts.Core.Audio.Extensions;
    using LoLEngine.Scripts.Core.Audio.Data; // For PlayOptions

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
    using LoLEngine.Scripts.Core.ServiceManagement.Service;
    using LoLEngine.Scripts.Core.ResourceManagement.Interfaces;
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
    public class PlayerScoreChangedEvent : LoLEngine.Scripts.Core.Events.GameEvent
    {
        public int NewScore { get; set; }
    }

    // Listener
    using LoLEngine.Scripts.Core.Events.Interfaces; // For IEventListener
    using LoLEngine.Scripts.Core.Events.Extensions; // For EventStartListening
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

7.  **Character Creation (Basic Example):**
    (CharacterFactory is disabled by default - enable it in your ServiceConfiguration if needed.)
    ```csharp
    using LoLEngine.Scripts.Core.ServiceManagement.Service;
    using LoLEngine.Scripts.GameObjects.Characters.Factory;
    using LoLEngine.Scripts.GameObjects.Characters.Interfaces; // For ICharacter
    using UnityEngine;

    public class CharacterSpawner : MonoBehaviour
    {
        public GameObject enemyPrefab; // Assign in Inspector

        void Start()
        {
            CharacterFactory characterFactory = ServiceLocator.Instance.Get<CharacterFactory>();
            if (characterFactory != null && enemyPrefab != null)
            {
                // Assuming CharacterData or similar config is handled by the factory or passed in
                ICharacter newEnemy = characterFactory.CreateEnemy(enemyPrefab, transform.position, Quaternion.identity, null /* CharacterData */, true /* usePooling */);
                if (newEnemy != null)
                {
                    Debug.Log($"Created enemy: {newEnemy.CharacterId}");
                }
            }
        }
    }
    ```

## ⚙️ Configuration Deep Dive

The LoL Engine's flexibility comes from its data-driven configuration:

### Service Configuration Presets

The engine provides three built-in service configuration presets:

#### 1. **Default Configuration (Recommended)**
```csharp
var config = ServiceConfiguration.CreateDefault();
```
**Enabled Services:**
- **Core Services:** EventManager, ResourceService, ObjectPool, TimeService
- **Feature Services:** AudioService, LocalizationService, InputService, CompressionService, EncryptionService, SerializationService, DataPersistence, NotificationService, AutoSaveService
- **Game Services:** GameStateManager
- **Boot System:** BootScreen

**Disabled Services:** UIService, SceneService, CharacterPersistence, CharacterFactory, AssetUpdater, QuickSaveService

*Perfect for most production games requiring common features like audio, saves, localization, and professional boot screens.*

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
config.enableUIService = true;        // Enable UI management
config.enableBootScreen = false;      // Disable boot screen
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
|---|---|---|---|---|
| **Core Services** | EventManager | ✅ | ✅ | Event handling system |
|  | ResourceService | ✅ | ✅ | Asset loading and management |
|  | ObjectPool | ✅ | ✅ | GameObject recycling |
|  | TimeService | ✅ | ✅ | Time management and scaling |
| **Feature Services** | AudioService | ✅ | ❌ | Sound and music playback |
|  | LocalizationService | ✅ | ❌ | Multi-language support |
|  | InputService | ✅ | ❌ | Player input handling |
|  | SerializationService | ✅ | ❌ | JSON data serialization |
|  | DataPersistenceService | ✅ | ❌ | Save/load functionality |
|  | CompressionService | ✅ | ❌ | Data compression |
|  | EncryptionService | ✅ | ❌ | Data encryption |
|  | NotificationService | ✅ | ❌ | In-game notifications |
|  | AutoSaveService | ✅ | ❌ | Automatic saving |
| **Game Services** | GameStateManager | ✅ | ❌ | Game state management |
|  | BootScreen | ✅ | ❌ | Professional loading screens |
|  | UIService | ❌ | ❌ | UI management (project-specific) |
|  | SceneService | ❌ | ❌ | Scene transitions |
|  | CharacterFactory | ❌ | ❌ | Character creation system |
|  | QuickSaveService | ❌ | ❌ | F5/F9 quick save/load |

### Customizing Service Configuration

```csharp
// Start with defaults and customize
var config = ServiceConfiguration.CreateDefault();

// Enable additional services
config.enableUIService = true;
config.enableSceneService = true;
config.enableCharacterFactoryService = true;

// Disable services you don't need
config.enableLocalizationService = false;  // Single language game
config.enableBootScreen = false;           // Skip loading screen
config.enableAutoSaveService = false;      // Manual saves only
```

### When to Use Which Configuration

**Use `CreateDefault()`** when:
- Building a production game
- Need common features (audio, saves, input, localization)
- Want professional boot screens
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


## 📖 Documentation

### Core Architecture
The framework uses a **Service Locator Pattern** with dependency injection:

- **Services**: Implement `ILoLEngineService` interface
- **Initialization**: Managed through `ServiceConfiguration` ScriptableObjects
- **Access**: Via `ServiceLocator.Instance.Get<TService>()`

### Key Services
- `IBootScreenService`: Boot screen progress tracking and visual feedback
- `IAudioService`: Audio playback and management
- `IResourceService`: Asset loading and caching
- `IEventManager`: Event handling and dispatching
- `IObjectPoolService`: Object pooling and recycling
- `ILocalizationService`: Multi-language support
- `IDataPersistance`: Data Persistance
- `IInputService` : Input Service
- `IEncryptionService` : Encryption Service
- `IGameStateManagerService` : Game State Service
- `INotificationService` : Notification Service
- `IDataSerializer` : Serialization Service
- `CharacterFactory`: For creating and managing character instances.
- `CharacterPersistenceManager`: Handles saving and loading of character states.

### Configuration
Services are configured via ScriptableObject assets in `Resources/Configs/`:
- `ResourcePathConfig`: Resource Configurations
- `ServiceConfiguration`: Controls which services are enabled
- `DefaultBootConfiguration`: Boot screen timing, UI settings, and scene transitions
- `DefaultAudioConfig`: Audio system settings
- `DefaultLocalizationConfig`: Localization settings
- `DefaultResourceManagementConfig`: Resource Management Settings (includes Addressables configuration)
- `DefaultSaveConfig`: Save Load Settings
- `DefaultCharacterConfig` (or similar): Defines character archetypes, stats, and persistence settings.

## 📞 Support

- **Website**: [https://moomoo.games](https://moomoo.games) 
- **Email**: Contact via website
---

*Made with ❤️ by MooMoo Games*
