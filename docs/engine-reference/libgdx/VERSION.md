# libGDX — Version Reference

| Field | Value |
|-------|-------|
| **Framework Version** | libGDX 1.13.1 |
| **Release Date** | 2024 (1.13.1 patch release) |
| **Project Pinned** | 2026-04-18 |
| **Last Docs Verified** | 2026-04-18 |
| **LLM Knowledge Cutoff** | May 2025 |
| **Risk Level** | MEDIUM — some 1.12/1.13 changes are near or past the cutoff |

## Knowledge Gap Warning

The LLM's training data likely covers libGDX up to approximately 1.11 with
partial awareness of 1.12. Several notable changes landed in 1.12 and 1.13 that
the model may not know about:

- **LWJGL2 desktop backend removed** (1.12+) — only LWJGL3 is supported for desktop
- **Android minSdk bumped** — 1.12 raised the minimum Android API; 1.13 raised it again
- **GWT is now legacy** — TeaVM is the recommended web backend for new projects
- **Java 17** is the current recommended target (was Java 8 in older docs)

Always cross-reference this directory before suggesting libGDX API calls.

## Post-Cutoff Version Timeline

| Version | Approx. Release | Risk Level | Key Theme |
|---------|-----------------|------------|-----------|
| 1.11 | 2022 | LOW | Within training data |
| 1.12.0 | 2023 | MEDIUM | LWJGL2 removed, Android minSdk bump, Java 17 default, gdx-setup modernization |
| 1.12.1 | 2024 | MEDIUM | Bug fixes, platform target updates |
| 1.13.0 | 2024 | MEDIUM | Further Android target updates, TeaVM maturation, Gradle upgrades |
| 1.13.1 | 2024 | MEDIUM | Patch release |

## Major Changes from 1.11 → 1.13

### Breaking Changes
- **LWJGL2 backend**: `com.badlogic.gdx.backends.lwjgl.*` classes removed. Use `com.badlogic.gdx.backends.lwjgl3.*` everywhere (`Lwjgl3Application`, `Lwjgl3ApplicationConfiguration`, `Lwjgl3Launcher`).
- **Android**: minimum SDK raised (now targets more recent Android baseline — verify against project's Gradle config)
- **gdx-setup tool**: replaced with a modernized setup (gdx-liftoff community tool is the most active maintained alternative)
- **Reflection limits on TeaVM**: confirmed reduction in what reflective code works on the web backend

### New / Improved APIs
- `MathUtils` additions — check current methods before assuming your known API set is complete
- `Array` and `ObjectMap` received minor iterator / convenience method additions
- Better null annotations across core classes

### Deprecated / Removed
- LWJGL2 backend (REMOVED)
- Legacy `com.badlogic.gdx.backends.lwjgl.LwjglApplication` and configuration classes (REMOVED)
- Any remaining Java 8-only patterns — Java 11+ idioms (and Java 17 at rest) are the target

## Backend Status

| Backend | Status | Notes |
|---------|--------|-------|
| LWJGL3 (desktop) | **Primary** | Windows / macOS / Linux — the only supported desktop backend |
| Android | **Maintained** | Check current minSdk in gdx-liftoff templates |
| iOS (MobiVM) | **Maintained** | Community fork of abandoned RoboVM; requires macOS + Xcode |
| TeaVM (web) | **Recommended for web** | Supersedes GWT; better Kotlin compatibility; smaller output |
| GWT (web) | **Legacy** | Still ships but not recommended for new projects |
| Headless | **Maintained** | Used for unit tests — `HeadlessApplication` |

## Extensions / Libraries Commonly Used

Active / maintained libGDX extensions as of reference date. These are NOT part of libGDX core — add only when needed:

- **Ashley** — lightweight Entity Component System (official)
- **Box2D / Box2DLights** — 2D physics (official)
- **gdx-controllers** — gamepad support (official)
- **gdx-freetype** — dynamic TTF/OTF font rendering (official)
- **gdx-ai** — AI utilities: state machines, behavior trees, pathfinding (official)
- **VisUI** — Scene2D.ui extensions with better widgets and skin (community)
- **Skin Composer** — GUI tool for authoring Scene2D.ui skins (community)
- **gdx-liftoff** — modern project setup tool (community, replacing gdx-setup)
- **gdx-gltf** — glTF 2.0 3D model support and PBR rendering (community)

Verify latest versions at https://libgdx.com/wiki/ and the extension maintainer's repo before adding.

## Verified Sources

- Official site: https://libgdx.com/
- GitHub: https://github.com/libgdx/libgdx
- Wiki: https://libgdx.com/wiki/
- API docs: https://javadoc.io/doc/com.badlogicgames.gdx/gdx/
- Release notes: https://github.com/libgdx/libgdx/blob/master/CHANGES
- gdx-liftoff: https://github.com/libgdx/gdx-liftoff
- Community forums: https://libgdx.com/dev/ (forum + Discord linked from site)
