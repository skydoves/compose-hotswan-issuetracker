<p align="center">
<img src="resources/logo.png" />
</p>

<p align="center">
  <a href="https://plugins.jetbrains.com/plugin/30551-compose-hotswan/"><img alt="JetBrains Plugin" src="https://img.shields.io/jetbrains/plugin/v/30551.svg?label=JetBrains%20Plugin"/></a>
  <a href="https://android-arsenal.com/api?level=28"><img alt="API" src="https://img.shields.io/badge/API-28%2B-brightgreen.svg?style=flat"/></a>
  <a href="https://discord.gg/jaTcyK5XCr"><img alt="Discord" src="https://img.shields.io/badge/Discord-Compose%20HotSwan-5865F2?logo=discord&logoColor=white"/></a>
  <a href="https://github.com/doveletter"><img alt="Profile" src="https://skydoves.github.io/badges/dove-letter.svg"/></a><br>
</p>

<p align="center">
<a href="https://hotswan.dev/">Compose HotSwan</a> is a JetBrains IDE plugin & compiler plugin that brings instant hot reload to Jetpack Compose and Compose Multiplatform. Edit your Compose UI code, save the file, and see the change on the app that is already running: on a real Android device, in the iOS simulator, and in your Compose Desktop window, from the same save. No rebuild, no reinstall, no restart.
</p>

## How It Works

[Compose HotSwan](https://hotswan.dev/) 2.0 runs its own interpreter inside your app. Earlier versions asked the Android runtime for permission to redefine a class and lived inside what it would allow; 2.0 stopped asking. When you save a file, HotSwan compiles only the changed code and applies it to the running app, then triggers Compose recomposition. That is what lets structural edits, up to replacing an entire screen, land in place with your navigation and state intact. For constant-only edits, the fast path skips the build entirely and applies in under 50 milliseconds.

For a detailed breakdown, visit the [How It Works](https://hotswan.dev/docs/how-it-works) documentation.

## Issue Tracker

This repository serves as the public issue tracker for [Compose HotSwan](https://hotswan.dev/). You can use this repository to report bugs, request features, and track known issues.

- **Bug reports**: If you encounter unexpected behavior, crashes, or compilation errors, please [open an issue](https://github.com/skydoves/compose-hotswan-issuetracker/issues/new) with your IDE version, plugin version, Kotlin version, and steps to reproduce.
- **Feature requests**: Have an idea for improving HotSwan? Open an issue describing your use case and the expected behavior.
- **Questions**: For general questions, check the [Troubleshooting](https://hotswan.dev/docs/troubleshooting) documentation or the [FAQ](https://hotswan.dev/faq) first.
- **Community**: Join the [Discord server](https://discord.gg/jaTcyK5XCr) to discuss HotSwan, share feedback, and connect with other users.

## Features

### Hot Reload

- **[Instant hot reload](https://hotswan.dev/docs/how-it-works)**: Apply UI changes to a running Android app without rebuilding or restarting. Your navigation stack, scroll position, ViewModel state, and `remember {}` values all stay intact.
- **[Literal patching](https://hotswan.dev/docs/literal-patching)**: Constant-only edits (string templates, numbers, hex colors, XML resource values) bypass the build pipeline entirely and apply in under 50ms. Ideal for fine-tuning colors, spacing, and copy.
- **[Structural changes](https://hotswan.dev/docs/supported-changes)**: Add and remove composables, change branching, wrap and unwrap layout, and rewrite a whole screen. Modify composable bodies, non-composable functions, modifiers, animation specs, resource values, data class properties, and ViewModel methods. Edits can span several files in one save, and a new composable does not have to live in the same file as its caller.
- **[State preservation](https://hotswan.dev/docs/state-preservation)**: Navigation back stack, scroll position, focus, IME state, animation progress, `remember`/`rememberSaveable`, and ViewModel instances survive every reload.
- **[Multi-module](https://hotswan.dev/docs/how-it-works)**: File paths are automatically resolved to the correct Gradle module. Changes in any module are compiled and pushed independently.
- **[Kotlin Multiplatform](https://hotswan.dev/docs/kotlin-multiplatform)**: One save reloads Android, Compose Desktop, and every booted iOS simulator. Apply the plugin to the module that owns the app: your Android application module, or the module declaring `binaries.framework` for iOS.

### Multi-Device & Preview

- **[Multi-target reload](https://hotswan.dev/docs/multi-device)**: Attach an Android device, a Compose Desktop window, and booted iOS simulators, and they all follow the same save. Catching a shared composable that is right on one platform and wrong on another is what this is for. The Android leg reloads one device per save, and HotSwan names which one.
- **[Preview Runner](https://hotswan.dev/docs/preview)**: Render a `@Preview` composable directly on a physical device without a full rebuild, with real device behavior and actual data. Android only.
- **[Preview Screenshot](https://hotswan.dev/docs/screenshot-testing)**: Discover the `@Preview` functions in your project, capture them on a real device, and generate a browsable HTML catalog with module grouping and dark/light theme support. No test code required. Android only.

### Design Collaboration

- **[Snapshot timeline](https://hotswan.dev/docs/snapshot)**: Every hot reload automatically captures a device screenshot paired with a git diff. Browse the visual history in the IDE tool window, time-travel back to revert code to any snapshot, and export self-contained visual reports for designers to review.

### AI Integration

- **[AI-assisted reload](https://hotswan.dev/docs/hot-reload-with-ai)**: Use Claude Code, Cursor, GitHub Copilot, or any AI tool that edits files on disk. HotSwan detects file changes, compiles, and pushes updates to the device automatically.
- **[MCP Server](https://hotswan.dev/docs/mcp-server)**: Connect AI assistants via the Model Context Protocol to iterate on your UI with natural language, seeing each change reflected on the device in real time.
- **[Agent Skill](https://hotswan.dev/docs/agent-skill)**: Drop-in instruction files (General + MCP variants) that teach AI coding assistants how to use HotSwan effectively, including reload-friendly editing patterns and screenshot/diagnose workflows.

### Build Integration

- **Debug only**: The Gradle plugin adds the HotSwan runtime as `debugImplementation` only. Release builds compile normally, with no HotSwan transformation and no runtime dependency.

Explore the full feature set at [hotswan.dev/docs](https://hotswan.dev/docs).

## Getting Started

### 1. Install the Android Studio Plugin

Open your Android Studio and navigate to **Settings** > **Plugins** > **Marketplace**, search for **Compose HotSwan**, and install it. Restart your IDE when prompted.

![install](resources/install.png)

### 2. Add the Gradle Plugin

Add the plugin to the `[plugins]` section of your `libs.versions.toml` file. Check the [latest version](https://hotswan.dev/docs/releases) for the version number.

```toml
[plugins]
hotswan-compiler = { id = "com.github.skydoves.compose.hotswan.compiler", version = "2.0.0" }
```

Register the plugin in your root `build.gradle.kts` with `apply false`:

```kotlin
plugins {
    alias(libs.plugins.hotswan.compiler) apply false
}
```

Then apply it in the module that owns the app. For Android that is your application module; for a Compose Multiplatform project it is the Android application module, or the module declaring `binaries.framework` for iOS:

```kotlin
plugins {
    alias(libs.plugins.hotswan.compiler)
}
```

Sync your project. The plugin auto configures everything for debug builds.

### 3. Start Hot Reloading

1. Build and run your app on a device or emulator as usual.
2. Open the HotSwan panel: **View** > **Tool Windows** > **HotSwan**.
3. Select your connected device and click **Start**.
4. Edit any Kotlin file, press **Cmd+S** (or **Ctrl+S**), and watch the device update.

### Gradle Configuration

You can customize the plugin behavior in your `build.gradle.kts`:

```kotlin
hotSwanCompiler {
    debugOnly = true               // Instrument debug variants only (default: true)
    literalPatching = true         // The constant-edit fast path (default: true)
    instrumentObjects = true       // Admit top level `object` declarations (default: true)
    desktopEnabled = true          // Instrument Compose Desktop compilations (default: true)
    dispatchRewriteEnabled = true  // Master switch (default: true)
}
```

Every option already defaults to the value that makes hot reload work, so applying the plugin is the whole setup. Each boolean also reads a Gradle property, so you can flip one for a single run with `-Photswan.<name>=false`.

For full configuration options, visit the [Gradle Configuration](https://hotswan.dev/docs/gradle-configuration) documentation.

## Requirements

| Requirement | Version |
|---|---|
| IDE | IntelliJ IDEA 2025.1+ / Android Studio Narwhal 2025.1+, no upper bound |
| Kotlin | 2.3.x to 2.4.x |
| Android Gradle Plugin | 9.x |
| Android device | API 28+, physical or emulator |
| iOS | `iosSimulatorArm64` simulator, Compose Multiplatform 1.11.0, Apple Silicon host |
| Desktop | Any Compose Desktop JVM |

See the full [Requirements](https://hotswan.dev/docs/requirements) documentation for IDE version compatibility details.

## Documentation

Visit [hotswan.dev/docs](https://hotswan.dev/docs) for the complete documentation.

**Getting Started**
- [Overview](https://hotswan.dev/docs)
- [Why HotSwan](https://hotswan.dev/docs/why-hotswan)
- [How It Works](https://hotswan.dev/docs/how-it-works)
- [Requirements](https://hotswan.dev/docs/requirements)
- [Gradle Configuration](https://hotswan.dev/docs/gradle-configuration)

**Hot Reload**
- [Supported Changes](https://hotswan.dev/docs/supported-changes)
- [Fast Pathing](https://hotswan.dev/docs/literal-patching)
- [State Preservation](https://hotswan.dev/docs/state-preservation)
- [Kotlin Multiplatform](https://hotswan.dev/docs/kotlin-multiplatform)

**Multi-Target & Preview**
- [Multi-Target](https://hotswan.dev/docs/multi-device)
- [Preview Runner](https://hotswan.dev/docs/preview)
- [Preview Screenshot](https://hotswan.dev/docs/screenshot-testing)

**Snapshot & AI**
- [Snapshot](https://hotswan.dev/docs/snapshot)
- [Hot Reload with AI](https://hotswan.dev/docs/hot-reload-with-ai)
- [MCP Server](https://hotswan.dev/docs/mcp-server)
- [Agent Skill](https://hotswan.dev/docs/agent-skill)

**Reference**
- [Limitations](https://hotswan.dev/docs/limitations)
- [Version Compatibility](https://hotswan.dev/docs/compatibility)
- [Troubleshooting](https://hotswan.dev/docs/troubleshooting)
- [Lifetime License](https://hotswan.dev/docs/lifetime-license)
- [Releases](https://hotswan.dev/docs/releases)

## Blog Posts

Read about the ideas, internals, and use cases behind Compose HotSwan on the [official blog](https://hotswan.dev/blog).

**Start here**
- [Compose HotSwan 2.0.0: The Future of Hot Reload on Android, iOS, and Desktop](https://hotswan.dev/blog/compose-hotswan-2-0-release): What the new interpreter engine changes, why structural edits now apply in place, and how one save reaches three targets.
- [Compose HotSwan v2 Beta: Hot Reload for Structural Changes, Whole Screens, and New Classes](https://hotswan.dev/blog/compose-hotswan-v2-beta): The engineering story behind v2, written while it was still in beta.

**Hot Reload Internals**
- [Jetpack Compose Hot Reload: The Complete Guide to Instant UI Updates](https://hotswan.dev/blog/jetpack-compose-hot-reload): A ground-up guide to what hot reload is, what it can and cannot do, and how the options compare.
- [Compose Hot Reload: Real-Time UI Updates on Running Android Devices](https://hotswan.dev/blog/compose-hot-reload): How HotSwan eliminates the build-wait-navigate loop and reloads Compose UI changes on a running device.
- [Why Your Android Build Takes So Long for a One Line Change](https://hotswan.dev/blog/android-build-pipeline): A deep dive into the Android build pipeline (Gradle, Kotlin compilation, dexing, install) that explains where the seconds go.
- [HotSwan vs Live Edit: Which Is Better for Compose Development?](https://hotswan.dev/blog/hotswan-vs-live-edit): A side-by-side comparison with Android Studio's built-in Live Edit.

**Multiplatform**
- [Desktop Hot Reload for Compose Multiplatform: One Save Updates Your Android Device and Desktop Window](https://hotswan.dev/blog/compose-multiplatform-desktop-hot-reload): Bringing the same reload to the Compose Desktop target.

**Live Tuning Workflows**
- [Tuning Compose Animations Without Rebuilding: Hot Reload for Dynamic Design](https://hotswan.dev/blog/compose-animation-hot-reload): Real-time animation iteration for durations, easing curves, and colors with sub-second feedback.
- [Hot Reloading AGSL Shaders Without a Rebuild: A Compose Walkthrough](https://hotswan.dev/blog/compose-agsl-shader-tuning): Every constant inside an AGSL shader and every Kotlin side knob tunable on a running device via the fast path.
- [Tuning Compose Themes Live: A Visual Feedback Loop for UI Design](https://hotswan.dev/blog/compose-palette-mcp): HotSwan Palette uses an AI agent to generate and compare theme variants side-by-side without rebuilds.
- [From ViewModel to Pixels: Hot Reloading Compose Side Effects in One Loop](https://hotswan.dev/blog/compose-side-effects-hot-reload): Iterating on `LaunchedEffect`, `produceState`, and the code behind the screen rather than just the screen.

**Preview & Design**
- [Compose Preview Renders Differently Than Your Real Device. Here's Why.](https://hotswan.dev/blog/compose-preview-vs-device): Why `@Preview` (layoutlib) diverges from real devices, and what that means for design fidelity.
- [Compose Preview Driven Development with Instant Feedback](https://hotswan.dev/blog/compose-preview-driven-development): Structuring maintainable previews and extending them to on-device rendering with zero rebuild time.
- [Compose Preview Screenshots in CI: A Real Device Catalog on Every Commit](https://hotswan.dev/blog/compose-preview-screenshots-ci): Turning the preview catalog into something your CI produces and your team reviews.

## Lifetime License

Compose HotSwan offers a [Lifetime License](https://hotswan.dev/docs/lifetime-license) for permanent access to all features, available through a one-time sponsorship of $200 or more via GitHub Sponsors. It also includes a lifetime subscription to the [Dove Letter](https://github.com/doveletter) newsletter.

## Release Notes

Check the latest releases and changelogs at [hotswan.dev/docs/releases](https://hotswan.dev/docs/releases).

## Community

Join the [Discord server](https://discord.gg/jaTcyK5XCr) to discuss Compose HotSwan, ask questions, share your experience, and connect with other users.

## Find this repository useful? :heart:

Support it by joining __[stargazers](https://github.com/skydoves/compose-hotswan-issuetracker/stargazers)__ for this repository. :star: <br>
Also __[follow](https://github.com/skydoves)__ me for my next creations!
