# LoL Engine - Labour of Love Game Framework

[![Unity Version](https://img.shields.io/badge/Unity-6000.0%2B-blue.svg)]()
[![Version](https://img.shields.io/badge/Version-1.0.0--rc.9-gold.svg)](CHANGELOG.md)
[![License](https://img.shields.io/badge/License-green.svg)](LICENSE.md)

A comprehensive, modular game development framework for Unity 6000.0+. Built on a clean Service Locator architecture, it provides production-ready systems — audio, saves, events, localization, deterministic RNG, object pooling, and more — with performance and ease of use in mind.

## Documentation

**Full documentation site: https://alien-tardigrade.github.io/LoL-Engine-README/**

| Guide | |
|---|---|
| [Quickstart (5 min)](https://alien-tardigrade.github.io/LoL-Engine-README/01_QUICKSTART/) | Install to a working scene |
| [Full Setup Guide](https://alien-tardigrade.github.io/LoL-Engine-README/GettingStarted/) | Every service, step by step |
| [Cheat Sheet](https://alien-tardigrade.github.io/LoL-Engine-README/CHEATSHEET-How-To-Use/) | Quick reference for every system |
| [Architecture Guide](https://alien-tardigrade.github.io/LoL-Engine-README/Architecture/) | Design and rationale |
| [Troubleshooting](https://alien-tardigrade.github.io/LoL-Engine-README/Troubleshooting/) | Common issues |

## Features

- **Service Management** — Service Locator, `ImprovedGameInitializer` startup orchestration, and data-driven `ServiceConfiguration` / `ResourcePathConfig` ScriptableObjects to enable services and link their configs.
- **Core Systems** — Audio (mixing, 3D spatialization, track management, AudioSource pooling), Resource Management (first-class Addressables with Resources fallback), Object Pooling, a type-safe Event System, and Time Management.
- **Data & Persistence** — Save/load with slots, async operations, compression, encryption, per-domain schema migration, corrupt-file quarantine, and pluggable `ISaveStore` backends (local file + in-memory test store).
- **Localization** — Multi-language support with dynamic switching, plus structured `LocString` + `DynamicVar` text with runtime variable substitution and CI validation.
- **Determinism & Content** — Deterministic named RNG streams (`IRngService`), the `ModelDb` content registry, the `AbstractModel` canonical/mutable boundary, and `TestMode` test hooks.
- **Utilities** — `TypedPoolBag`, `AbstractOdds` / `AntiPityOdds` (anti-pity with full save/load round-trip), a Notification system, Encryption & Compression services, and assorted helper classes and extensions.

Per-system guides live on the [documentation site](https://alien-tardigrade.github.io/LoL-Engine-README/).

## Requirements

- **Unity** 6000.0 or later

### Platform Support

| Platform | Status |
|---|---|
| Windows / macOS / Linux (Mono & IL2CPP) | Supported |
| iOS / Android (IL2CPP) | Supported — read the [IL2CPP guide](https://alien-tardigrade.github.io/LoL-Engine-README/IL2CPP/) before shipping (managed code stripping) |
| WebGL | Not supported in 1.0 (async save/load and the file log sink use background threads, which WebGL lacks) |

### Dependencies

| Package | Version | Purpose |
|---|---|---|
| `com.unity.addressables` | 2.5.0+ | Asset loading via `IResourceService` |
| `com.unity.nuget.newtonsoft-json` | 3.2.1+ | JSON serialization for `ISaveSystem` |

Optional: `com.unity.inputsystem` (use directly in game code via `InputActionAsset` / `PlayerInput`) and `com.unity.textmeshpro` (sample-scene UI; falls back to legacy `UnityEngine.UI.Text` if absent).

## Installation

**Unity Package Manager (recommended):** open **Window > Package Manager**, add LoL Engine from the provided UPM source, and confirm it appears as **LoL Engine**.

**From disk (development):** clone the repository, then in Package Manager choose **+ > Add package from disk…** and select the folder containing `package.json`.

## License & Support

- **License** — see [LICENSE.md](LICENSE.md)
- **Changelog** — see [CHANGELOG.md](CHANGELOG.md)
- **Support** — unitysupport@moomoo.games
- **Documentation** — https://alien-tardigrade.github.io/LoL-Engine-README/
