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
<a href="https://hotswan.dev/">Compose HotSwan</a> is a JetBrains IDE plugin & compiler plugin that enables instant hot reload for Jetpack Compose on "real" Android devices. Edit your Compose UI code, save the file, and see your changes reflected on a real device in seconds, without rebuilding or restarting the app.
</p>

## How It Works

[Compose HotSwan](https://hotswan.dev/) uses incremental Kotlin compilation combined with runtime class swapping on the Android Runtime (ART) to deliver fast, reliable hot reload on real Android devices. When you save a file, HotSwan compiles only the changed code, extracts modified classes, pushes them to the connected device, and triggers Compose recomposition. The entire pipeline typically completes in under a few seconds. For constant-only edits, it completes in under 50 milliseconds via the literal patching fast path.

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
- **[Broad change support](https://hotswan.dev/docs/supported-changes)**: Modify composable bodies, non-composable functions, modifiers, animation specs, conditional logic, resource values, data class properties, and ViewModel methods. Add new composable functions, reorder existing calls, and edit extension/suspend/vararg functions.
- **[State preservation](https://hotswan.dev/docs/state-preservation)**: Navigation back stack, scroll position, focus, IME state, animation progress, `remember`/`rememberSaveable`, and ViewModel instances survive every reload.
- **[Multi-module](https://hotswan.dev/docs/how-it-works)**: File paths are automatically resolved to the correct Gradle module. Changes in any module are compiled and pushed independently.
- **[Kotlin Multiplatform](https://hotswan.dev/docs/kotlin-multiplatform)**: Hot reload works for the Android target of KMP projects, covering both the Android application module and shared KMP modules compiled into the app.

### Multi-Device & Preview

- **[Multi-device broadcast](https://hotswan.dev/docs/multi-device)**: Connect any number of devices and edit once to see changes everywhere simultaneously. Useful for responsive layout testing across screen sizes, verifying behavior across API levels, and demo preparation.
- **[Preview Runner](https://hotswan.dev/docs/preview)**: Render `@Preview` composables directly on a physical device in under 0.5 seconds without a full rebuild. Iterate on previews with real device behavior and actual data.
- **[Preview Screenshot](https://hotswan.dev/docs/screenshot-testing)**: Automatically discover every `@Preview` function in your project, capture screenshots on a real device, and generate a browsable HTML catalog with module grouping and dark/light theme support. No test code required.

### Design Collaboration

- **[Snapshot timeline](https://hotswan.dev/docs/snapshot)**: Every hot reload automatically captures a device screenshot paired with a git diff. Browse the visual history in the IDE tool window, time-travel back to revert code to any snapshot, and export self-contained visual reports for designers to review.

### AI Integration

- **[AI-assisted reload](https://hotswan.dev/docs/hot-reload-with-ai)**: Use Claude Code, Cursor, GitHub Copilot, or any AI tool that edits files on disk. HotSwan detects file changes, compiles, and pushes updates to the device automatically.
- **[MCP Server](https://hotswan.dev/docs/mcp-server)**: Connect AI assistants via the Model Context Protocol to iterate on your UI with natural language, seeing each change reflected on the device in real time.
- **[Agent Skill](https://hotswan.dev/docs/agent-skill)**: Drop-in instruction files (General + MCP variants) that teach AI coding assistants how to use HotSwan effectively, including reload-friendly editing patterns and screenshot/diagnose workflows.

### Build Integration

- **Debug only**: The Gradle plugin adds the client library as `debugImplementation` only. Release builds have zero overhead.

Explore the full feature set at [hotswan.dev/docs](https://hotswan.dev/docs).

## Getting Started

### 1. Install the Android Studio Plugin

Open your Android Studio and navigate to **Settings** > **Plugins** > **Marketplace**, search for **Compose HotSwan**, and install it. Restart your IDE when prompted.

![install](resources/install.png)

### 2. Add the Gradle Plugin

Add the plugin to the `[plugins]` section of your `libs.versions.toml` file. Check the [latest version](https://hotswan.dev/docs/releases) for the version number.

```toml
[plugins]
hotswan-compiler = { id = "com.github.skydoves.compose.hotswan.compiler", version = "version" }
```

Register the plugin in your root `build.gradle.kts` with `apply false`:

```kotlin
plugins {
    alias(libs.plugins.hotswan.compiler) apply false
}
```

Then apply it in your app module's `build.gradle.kts`:

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
    enabled = true      // Master switch (default: true)
    debugOnly = true    // Apply only to debug builds (default: true)
}
```

For full configuration options, visit the [Gradle Configuration](https://hotswan.dev/docs/gradle-configuration) documentation.

## Requirements

| Requirement | Minimum Version |
|---|---|
| Android API | 28+ (API 30+ recommended) |
| IDE | IntelliJ IDEA 2024.3+ / Android Studio Meerkat+ |
| Kotlin | 2.3.0+ |
| Android Gradle Plugin | 8.7.3+ |

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
- [Literal Patching](https://hotswan.dev/docs/literal-patching)
- [State Preservation](https://hotswan.dev/docs/state-preservation)
- [Kotlin Multiplatform](https://hotswan.dev/docs/kotlin-multiplatform)

**Multi-Device & Preview**
- [Multi-Device](https://hotswan.dev/docs/multi-device)
- [Preview Runner](https://hotswan.dev/docs/preview)
- [Preview Screenshot](https://hotswan.dev/docs/screenshot-testing)

**Snapshot & AI**
- [Snapshot](https://hotswan.dev/docs/snapshot)
- [Hot Reload with AI](https://hotswan.dev/docs/hot-reload-with-ai)
- [MCP Server](https://hotswan.dev/docs/mcp-server)
- [Agent Skill](https://hotswan.dev/docs/agent-skill)

**Reference**
- [Limitations](https://hotswan.dev/docs/limitations)
- [Troubleshooting](https://hotswan.dev/docs/troubleshooting)
- [Lifetime License](https://hotswan.dev/docs/lifetime-license)
- [Releases](https://hotswan.dev/docs/releases)

## Blog Posts

Read about the ideas, internals, and use cases behind Compose HotSwan on the [official blog](https://hotswan.dev/blog).

**Hot Reload Internals**
- [Compose Hot Reload: Real-Time UI Updates on Running Android Devices](https://hotswan.dev/blog/compose-hot-reload): How HotSwan eliminates the build-wait-navigate loop and reloads Compose UI changes on a running device in under a second.
- [Why Your Android Build Takes So Long for a One Line Change](https://hotswan.dev/blog/android-build-pipeline): A deep dive into the Android build pipeline (Gradle, Kotlin compilation, dexing, install) that explains where the seconds go.

**Live Tuning Workflows**
- [Tuning Compose Animations Without Rebuilding: Hot Reload for Dynamic Design](https://hotswan.dev/blog/compose-animation-hot-reload): Real-time animation iteration for durations, easing curves, and colors with sub-second feedback.
- [Hot Reloading AGSL Shaders Without a Rebuild: A Compose Walkthrough](https://hotswan.dev/blog/compose-agsl-shader-tuning): Every constant inside an AGSL shader and every Kotlin side knob tunable on a running device via literal patching.
- [Tuning Compose Themes Live: A Visual Feedback Loop for UI Design](https://hotswan.dev/blog/compose-palette-mcp): HotSwan Palette uses an AI agent to generate and compare theme variants side-by-side without rebuilds.

**Preview & Design**
- [Compose Preview Renders Differently Than Your Real Device. Here's Why.](https://hotswan.dev/blog/compose-preview-vs-device): Why `@Preview` (layoutlib) diverges from real devices, and what that means for design fidelity.
- [Compose Preview Driven Development with Instant Feedback](https://hotswan.dev/blog/compose-preview-driven-development): Structuring maintainable previews and extending them to on-device rendering with zero rebuild time.

## Lifetime License

Compose HotSwan offers a [Lifetime License](https://hotswan.dev/docs/lifetime-license) for permanent access to all features, available through a one-time sponsorship of $200 or more via GitHub Sponsors. It also includes a lifetime subscription to the [Dove Letter](https://github.com/doveletter) newsletter.

## Release Notes

Check the latest releases and changelogs at [hotswan.dev/docs/releases](https://hotswan.dev/docs/releases).

## Community

Join the [Discord server](https://discord.gg/jaTcyK5XCr) to discuss Compose HotSwan, ask questions, share your experience, and connect with other users.

## Find this repository useful? :heart:

Support it by joining __[stargazers](https://github.com/skydoves/compose-hotswan-issuetracker/stargazers)__ for this repository. :star: <br>
Also __[follow](https://github.com/skydoves)__ me for my next creations!
