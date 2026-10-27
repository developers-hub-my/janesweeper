# JaneSweeper

Keeps disk usage under a target (default 60%) by cleaning caches, build output and old downloads on a schedule.
Anything removed goes to **quarantine first** and can be restored until it is purged.

By **Developers Hub Sdn Bhd** · https://devhub.my · https://janesweeper.netlify.app

This repo holds the compiled downloads only. Get them from **[Releases](https://github.com/developers-hub-my/janesweeper/releases/latest)**.

## Downloads

| File | For |
|---|---|
| `JaneSweeper-<ver>-macos-universal.zip` | macOS desktop app (Apple Silicon + Intel). Install the CLI too |
| `jsweep-<ver>-darwin-arm64` | CLI, macOS Apple Silicon |
| `jsweep-<ver>-darwin-amd64` | CLI, macOS Intel |
| `jsweep-<ver>-linux-amd64` | CLI, Linux x86-64 servers |
| `jsweep-<ver>-linux-arm64` | CLI, Linux ARM servers |
| `SHA256SUMS` | Checksums |

Windows is not available yet.

## Install the CLI

macOS (Apple Silicon shown; use `darwin-amd64` on Intel):

```sh
VER=0.1.0
curl -fLo jsweep "https://github.com/developers-hub-my/janesweeper/releases/download/v$VER/jsweep-$VER-darwin-arm64"
chmod +x jsweep
xattr -d com.apple.quarantine jsweep 2>/dev/null   # binary is not notarised yet
sudo mv jsweep /usr/local/bin/
jsweep init        # writes ~/.config/jsweep/config.yaml; cleans nothing
jsweep doctor
```

Linux server:

```sh
VER=0.1.0
curl -fLo jsweep "https://github.com/developers-hub-my/janesweeper/releases/download/v$VER/jsweep-$VER-linux-amd64"
sudo install -m 755 jsweep /usr/local/bin/jsweep
sudo jsweep init --profile server   # server profile is report-only by default
sudo jsweep doctor
```

Check a download: `shasum -a 256 -c SHA256SUMS --ignore-missing`.

## Install the desktop app (macOS)

1. Install the CLI first (above). The app runs `jsweep` for every action.
2. Unzip `JaneSweeper-<ver>-macos-universal.zip` and move `JaneSweeper.app` to `/Applications`.
3. The app is not signed yet, so the first time: right-click it, choose **Open**, then **Open** again.
   Or run `xattr -dr com.apple.quarantine /Applications/JaneSweeper.app`.

## First run

```sh
jsweep scan                 # what would be cleaned, changes nothing
jsweep clean --dry-run      # the plan
jsweep clean                # moves the plan to quarantine
jsweep quarantine list      # restore with: jsweep restore <id>
jsweep schedule install     # run on the OS scheduler
```

`jsweep help` lists every command. Exit codes: 0 ok · 1 error · 2 config invalid · 3 still over target · 4 cap hit · 130 cancelled.

## Safety

- Deny list, symlink, git-tracked, project-marker and busy-project guards cannot be turned off.
- Default action is quarantine. The server profile only reports unless you change it.

Questions: hello@devhub.my

© 2026 Developers Hub Sdn Bhd. All rights reserved.
