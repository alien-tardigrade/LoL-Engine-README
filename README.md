# LoL Engine - Labour of Love Game Framework

[![Unity Version](https://img.shields.io/badge/Unity-6000.2%2B-blue.svg)]() 
[![Version](https://img.shields.io/badge/Version-0.5.2--alpha-gold.svg)](CHANGELOG.md)
[![License](https://img.shields.io/badge/License-green.svg)](LICENSE.md)

A comprehensive Unity game development framework providing essential systems for modern game development. Built with performance, modularity, and ease of use in mind.

## 📖 Overview

The LoL Engine provides a suite of interconnected services and tools designed to accelerate game development in Unity. It emphasizes a clean architecture through a service locator pattern, data-driven configuration, and robust implementations of common game systems.

## 🚀 Features

### 🎯 **NEW: Enhanced Addressables Integration**
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
    *   `Audio System`: Advanced audio playback, mixing, 3D spatialization, track management, and `AudioSource` pooling. ([Docs](Documentation~/audio-readme.md))
    *   `Resource Management`: Unified asset loading with **first-class Addressables support**, Resources fallback, and object pooling integration. ([Docs](Documentation~/resource-management-readme.md))
    *   `Object Pooling`: High-performance pooling for GameObjects and components. ([Docs](Documentation~/object-pool-readme.md))
    *   `Event System`: Type-safe, centralized event handling with automatic listener management. ([Docs](Documentation~/events-readme.md))
    *   `Data Persistence`: Robust save/load system with slots, async operations, compression, and encryption. ([Docs](Documentation~/data-persistence-readme.md))
    *   `Localization System`: Comprehensive multi-language support with dynamic switching. ([Docs](Documentation~/localization-readme.md))
    *   `Scene Management`: Flexible scene loading, transitions, and management. ([Docs](Documentation~/scene-management-readme.md))
    *   `Input System`: Unified input handling for various devices. ([Docs](Documentation~/input-readme.md))
    *   `UI System`: Structured approach for managing UI screens and dialogs. ([Docs](Documentation~/ui-readme.md))
    *   `Character System`: Reusable character implementation with stats, health, AI hooks, persistence, and factory pattern. ([Docs](Documentation~/character-system-readme.md))
*   **Utilities & Advanced:**
    *   `Time Management`: Pause, slow-motion, and custom time scaling.
    *   `Notification System`: In-game messaging and alerts.
    *   Encryption & Compression services.
    *   Various helper classes and extension methods. 

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
    *   In your project's `Assets` folder, create a `ServiceConfiguration` asset:
        *   Right-click → `Create` → `LoLEngine` → `Configuration` → `Service Configuration`.
        *   Name it (e.g., "MyGame_ServiceConfig").
    *   Similarly, create a `ResourcePathConfig` asset:
        *   Right-click → `Create` → `LoLEngine` → `Configuration` → `Resource Path Config`.
        *   Name it (e.g., "MyGame_ResourcePaths").
    *   In your first scene (e.g., a "Boot" or "Initialization" scene), create an empty GameObject (e.g., "EngineInitializer").
    *   Add the `ImprovedGameInitializer` component to this GameObject.
    *   Assign your created `ServiceConfiguration` and `ResourcePathConfig` assets to the respective fields on the `ImprovedGameInitializer` component in the Inspector.
    *   Customize the `ServiceConfiguration` asset to enable the services your game needs and link to their specific configuration assets (e.g., `AudioConfig`, `SaveConfig`). Many services require their own config assets to be created and assigned here.
    *   **Optional - Enable Addressables** (Recommended):
        *   Go to `Window > Asset Management > Addressables > Groups`
        *   Click "Create Addressables Settings" if prompted
        *   Mark your assets as Addressable and set meaningful addresses (e.g., "Audio/BackgroundMusic01")
        *   In your `ResourceManagementConfig`, set `enableAddressables = true`

2.  **Accessing Services:**
    Once initialized, services can be accessed via the `ServiceLocator`:
    ```csharp
    using LoLEngine.Scripts.Core.ServiceManagement.Service;
    using LoLEngine.Scripts.Core.Audio.Interfaces; // Example for Audio

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

3.  **Basic Audio Usage:**
    (Ensure `AudioService` is enabled in `ServiceConfiguration` and an `AudioConfig` is set up and assigned.)
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
4.  **Resource Loading with Addressables:**
    (Ensure `ResourceService` is enabled in `ServiceConfiguration`.)
    
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

5.  **Event System:**
    (Ensure `EventManagerService` is enabled in `ServiceConfiguration`.)
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

6.  **Character Creation (Basic Example):**
    (Ensure `CharacterFactory` and related services are configured.)
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

1.  **`ImprovedGameInitializer`**: The entry point in your scene. Requires a `ServiceConfiguration` and `ResourcePathConfig`.
2.  **`ServiceConfiguration` (ScriptableObject)**:
    *   The central hub for enabling/disabling engine services.
    *   Contains fields to link to specific configuration assets for each service (e.g., `AudioConfig`, `SaveConfig`, `LocalizationConfig`). You'll need to create these specific config assets and assign them here.
3.  **`ResourcePathConfig` (ScriptableObject)**:
    *   Defines default paths for loading resources and configurations if they are not directly assigned or need to be found dynamically.
4.  **Service-Specific Configs (ScriptableObjects)**:
    *   Examples: `AudioConfig`, `SaveConfig`, `LocalizationConfig`.
    *   These assets hold detailed settings for their respective services. Create them via the `Assets/Create/LoLEngine/...` menu and assign them to your `ServiceConfiguration`.

Refer to the detailed documentation for each service (linked in the Features section) for specifics on their configuration assets.


## 📖 Documentation

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