# Mac Stats Menu Bar

A Codex skill for installing, configuring, repairing and verifying [Stats](https://github.com/exelban/stats), the open-source macOS menu-bar system monitor.

The default compact profile shows:

- CPU as a Mini Label
- memory usage as a Mini widget
- memory pressure as a green, yellow or red State dot
- a memory-pressure alert
- a total CPU alert at 90%
- Stats at login

The skill favours memory pressure over raw RAM percentage because macOS can use most available RAM efficiently for caching without being under memory stress.

## Install

Run the bundled Codex skill installer:

```bash
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo I-Cam-Mc/mac-stats-menubar \
  --path skills/mac-stats-menubar
```

The skill will be available on the next Codex turn.

## Use

```text
Use $mac-stats-menubar to set up and verify compact CPU, memory and memory-pressure indicators on this Mac.
```

## Behaviour

The skill:

- preserves unrelated Stats modules and preferences
- backs up the existing Stats preference file before changes
- repairs a confirmed stale Homebrew cask record without deleting Stats data
- handles the separate Stats menu-bar permission introduced in macOS 26
- verifies both saved preferences and the visible menu-bar result
- avoids claiming that login persistence or notifications were tested when only their settings were confirmed
- does not deliberately stress the CPU or memory unless explicitly requested

Full visual verification requires a native macOS computer-use tool with Accessibility and Screen Recording access.
