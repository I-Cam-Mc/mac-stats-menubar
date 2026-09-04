---
name: mac-stats-menubar
description: Install, configure, repair, or verify the Stats macOS menu-bar monitor for compact CPU, memory usage, and memory-pressure indicators. Use for Stats setup or missing Stats menu-bar items, not general performance diagnosis.
---

# Mac Stats Menu Bar

Configure the open-source Stats app (`eu.exelban.Stats`) on macOS with this compact menu-bar profile:

- CPU module on, with the **Mini Label** widget visible.
- RAM module on, with **Mini** and **State** visible. `State` is the green, yellow, or red memory-pressure dot.
- Start at login on.
- RAM memory-pressure alert on.
- Total CPU alert on at 90%.

Memory pressure is the useful health signal. Raw RAM utilisation can be high while macOS is using memory efficiently for caching. Green is healthy, yellow indicates increasing constraint, and red indicates serious pressure.

## Preserve the machine

- Inspect the current app, version, preferences, running state, and macOS version before changing anything.
- Preserve unrelated Stats modules and settings. Do not reset all settings or replace another monitor unless the user asks.
- If the requested profile is already present, verify it and stop. Do not reinstall for tidiness.
- Before preference changes, copy the existing `~/Library/Preferences/eu.exelban.Stats.plist` to a timestamped backup under `/private/tmp` when it exists.
- Use a native macOS computer-use tool for Stats and System Settings. Use the installed `computer-use` skill when available. Element indexes and coordinates are transient, so read fresh state after every UI change.
- Treat installing or force-reinstalling an app as a system mutation and use the normal approval path. Never remove Stats preferences or application data when repairing Homebrew state.

## Install or repair Stats

1. Verify whether `/Applications/Stats.app` exists and whether its bundle identifier is `eu.exelban.Stats`.
2. If it is absent, locate Homebrew with `command -v brew` and install the cask with `brew install --cask stats` through the required approval path.
3. If Homebrew reports Stats installed while `/Applications/Stats.app` is missing, confirm this exact stale-record condition. Then use the approved, scoped recovery:

   ```text
   brew uninstall --cask --force stats
   brew install --cask stats
   ```

4. Launch Stats and confirm the running process and bundle identifier. Do not hardcode an app version.

## Configure the compact profile

Use the Stats UI for modules and widgets:

1. In **Settings**, turn **Start at login** on.
2. Open **CPU**, ensure the module toggle is on, open **Widgets**, and turn **Mini Label** on.
3. Open **RAM**, ensure the module toggle is on, open **Widgets**, and select **Mini** plus **State**. For the exact compact profile, deselect only redundant RAM widgets, leaving `mini,state`.
4. In CPU notifications, enable total-load notifications at 90%.
5. In RAM notifications, enable the memory-pressure alert.

Prefer the UI. The two alert settings may use these version-sensitive preference keys:

```text
CPU_notifications_totalLoad_state = 1
CPU_notifications_totalLoad_value = 90
RAM_notifications_pressure_state = 1
```

If the UI cannot operate only those alert controls, a backed-up `defaults write` fallback may set these exact keys, followed by a Stats restart and read-back. Do not configure widgets by writing preferences directly.

On macOS 26 or newer, inspect **System Settings → Menu Bar** and turn **Stats** on. This OS permission is separate from Stats' own module and widget switches.

## Verify before claiming completion

Verify both saved state and visible outcome:

- `/Applications/Stats.app` exists, has bundle identifier `eu.exelban.Stats`, and is running.
- Stats shows the CPU module on and **Mini Label** enabled.
- Stats shows the RAM module on and **Mini** plus **State** enabled.
- `defaults read eu.exelban.Stats RAM_widget` reads `mini,state` for the exact profile.
- `LaunchAtLoginNext` reads `1`, and the Stats UI shows **Start at login** on.
- The three intended menu-bar signals are visibly present: CPU, memory usage, and the memory-pressure dot.
- On macOS 26 or newer, System Settings visibly shows Stats allowed in the menu bar.
- Alert keys read back as enabled with CPU threshold `90`.

The CPU Mini Label can be the app default and may not have a distinct stored preference key. Its absence from `defaults read` is not evidence that it is disabled, so verify it in the UI.

Do not claim that launch at login survived a real login cycle unless one was observed. Do not claim notification delivery was tested merely because the alert keys were saved. Avoid deliberate CPU saturation or memory-pressure stress unless the user explicitly requests a bounded test.

If native UI verification is unavailable, report exactly what configuration was read back and leave visible menu-bar completion unconfirmed.
