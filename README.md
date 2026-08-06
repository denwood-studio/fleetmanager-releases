# FleetManager — releases

Build artefacts and the auto-update feed for FleetManager, Denwood Studio's desktop control plane
for Claude Code across git worktrees.

**There is no source code here.** Every release on this repo is published automatically by the
release workflow in the (private) source repo, from artefacts that have already been signed with
Denwood's Developer ID, notarized by Apple, and stapled.

## What each release contains

| File | Purpose |
| --- | --- |
| `FleetManager-<version>-arm64.dmg` | Drag-to-Applications installer — what you want for a first install |
| `FleetManager-<version>-arm64-mac.zip` | The `.app`, used by the in-app updater |
| `latest-mac.yml` | Update manifest read by `electron-updater` |
| `*.blockmap` | Differential-download indexes |

Apple Silicon only.

## Installing

Download the `.dmg` from the [latest release](../../releases/latest) and drag FleetManager to
Applications. The build is notarized, so it opens with no Gatekeeper prompt and no
right-click → Open.

After that, FleetManager updates itself: it checks on launch and every six hours, downloads in the
background, and asks before restarting.

## Reporting issues

Issues are tracked privately. Contact Denwood Studio.
