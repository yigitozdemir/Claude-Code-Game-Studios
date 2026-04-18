# libGDX — Breaking Changes

Last verified: 2026-04-18

Version-by-version breaking changes relevant to code written with older API knowledge. If the LLM suggests an API and it appears here as "removed" or "changed", use the replacement instead.

## 1.11 → 1.12

### LWJGL2 Backend — REMOVED

The legacy desktop backend is gone. All references to `com.badlogic.gdx.backends.lwjgl.*` must migrate to `com.badlogic.gdx.backends.lwjgl3.*`.

| Old (REMOVED) | New (USE THIS) |
|---|---|
| `com.badlogic.gdx.backends.lwjgl.LwjglApplication` | `com.badlogic.gdx.backends.lwjgl3.Lwjgl3Application` |
| `com.badlogic.gdx.backends.lwjgl.LwjglApplicationConfiguration` | `com.badlogic.gdx.backends.lwjgl3.Lwjgl3ApplicationConfiguration` |
| `LwjglApplicationConfiguration.width / height` | `Lwjgl3ApplicationConfiguration.setWindowedMode(w, h)` |
| `LwjglApplicationConfiguration.title` | `Lwjgl3ApplicationConfiguration.setTitle(...)` |
| `LwjglApplicationConfiguration.vSyncEnabled` | `Lwjgl3ApplicationConfiguration.useVsync(true)` |

Desktop launcher pattern is now:

```java
public class Lwjgl3Launcher {
    public static void main(String[] args) {
        Lwjgl3ApplicationConfiguration config = new Lwjgl3ApplicationConfiguration();
        config.setTitle("My Game");
        config.setWindowedMode(1280, 720);
        config.useVsync(true);
        new Lwjgl3Application(new MyGame(), config);
    }
}
```

### Android minSdk bumped
The Android backend now requires a newer minimum API level. Update `android/build.gradle`'s `minSdkVersion` accordingly. Verify the current minimum against the libGDX wiki.

### Java version
Gradle projects now assume Java 17 as the default target. Older Java 8-only code still compiles but:
- gdx-liftoff templates use Java 17
- Some libGDX internals may assume Java 11+ language features

## 1.12 → 1.13

### Android target SDK raised
Android Gradle Plugin and target SDK pushed forward — typically required annually for Play Store compliance.

### TeaVM is the recommended web backend
GWT is still supported but marked as legacy. New projects should use TeaVM:
- Smaller bundle size
- Better Kotlin support
- More of the libGDX API works (reflection is still limited, but broader)
- gdx-liftoff defaults to TeaVM for new project setup

### gdx-setup replaced
The classic `gdx-setup.jar` tool is no longer maintained. Use **gdx-liftoff** (community tool, community-maintained but endorsed by core maintainers) for creating new libGDX projects.

## General API Notes (not version-pinned — verify current state)

### SpriteBatch Shader Contract
Stable since 1.9.x. Custom shaders for `SpriteBatch` must accept:
- Attributes: `a_position`, `a_color`, `a_texCoord0`
- Uniforms: `u_projTrans`, `u_texture`

If this changes in a future version, the breakage will surface immediately (shader won't compile or batch throws).

### ShapeRenderer thread safety
Unchanged — single-threaded, like all libGDX drawing APIs.

### AssetManager
Core API stable. Minor additions (custom `AssetLoader` convenience methods) have appeared but nothing has been removed.

### Scene2D
Core Actor/Stage API stable across 1.11 → 1.13. Minor additions. No breakages in `Table`, `Stage`, `Skin`, `Actions` that I'm aware of at the reference date.

## Migration Checklist (1.11 → 1.13.1)

If upgrading an older project:

1. [ ] Replace all `com.badlogic.gdx.backends.lwjgl.*` imports with `com.badlogic.gdx.backends.lwjgl3.*`
2. [ ] Rewrite the desktop launcher to use `Lwjgl3ApplicationConfiguration`
3. [ ] Bump Android `minSdkVersion` and `targetSdkVersion` per current wiki
4. [ ] Update `gdxVersion` in `gradle.properties`
5. [ ] Update Gradle wrapper (`./gradlew wrapper --gradle-version <current>`)
6. [ ] Update Android Gradle Plugin version in root `build.gradle`
7. [ ] Update Java source/target compatibility to 17 (or whatever current libGDX pins)
8. [ ] If using GWT and starting fresh work, consider migrating to TeaVM
9. [ ] Run `./gradlew lwjgl3:run` and `./gradlew test` to catch remaining API breaks
10. [ ] Review any extensions (Box2D, Ashley, gdx-ai) — extension versions must match core libGDX version

## Sources

- https://libgdx.com/news/
- https://github.com/libgdx/libgdx/blob/master/CHANGES
- https://libgdx.com/wiki/start/project-setup
- https://libgdx.com/wiki/deployment (per-backend migration notes)
