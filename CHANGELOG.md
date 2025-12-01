# Changelog

All notable changes to the LoL Engine package will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
