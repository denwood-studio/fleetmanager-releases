# FleetManager

FleetManager is a desktop app that runs and watches Claude Code sessions across your git worktrees. The sidebar lists your repositories and worktrees, each session lives in its own terminal tab, and a dashboard shows which sessions are working, finished, or waiting for you — so you can run several at once without watching every terminal.

This repo holds no source code, only the signed, notarized and stapled release builds and the auto-update feed.

## Requirements

- macOS on Apple silicon (releases are arm64 builds)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — the `claude` command must be found on your shell's PATH; sessions can't start without it
- git, with at least one git repository to work in

Optional, and checked under Settings → Setup:

- The GitHub CLI (`gh`), signed in — without it the PR and Merge buttons stay hidden
- VS Code with its `code` command on your PATH
- A Linear personal API key — without it ticket titles and the ticket popout stay empty

## Install

1. Download `FleetManager-<version>-arm64.dmg` from the [latest release](https://github.com/denwood-studio/fleetmanager-releases/releases/latest).
2. Open it and drag FleetManager to Applications.

FleetManager checks for updates on launch and every six hours, downloads them in the background, and asks before restarting to install.

## What each release contains

| File | Purpose |
| --- | --- |
| `FleetManager-<version>-arm64.dmg` | Drag-to-Applications installer — what you want for a first install |
| `FleetManager-<version>-arm64-mac.zip` | The `.app`, used by the in-app updater |
| `latest-mac.yml` | Update manifest read by `electron-updater` |
| `*.blockmap` | Differential-download indexes |

Apple Silicon only.

## First run

On first launch FleetManager walks you through three steps:

1. **Welcome** — a short introduction to the app.
2. **Choose a repository** — pick a git repository you work in, the folder you'd run `git status` in. FleetManager lists its worktrees in the sidebar; adding it changes nothing in the repo.
3. **Start your first session** — FleetManager opens a Claude Code session in that folder. If Claude Code asks whether you trust the folder, answer in the terminal.

Between steps 2 and 3 you're asked whether to turn on session status detection. This adds hook commands to Claude Code's global `~/.claude/settings.json`; the prompt lists the exact commands before anything is written. They're what let FleetManager tell whether a session is working, finished, or waiting for you. You can skip it and run without status, and enable it later under Settings → Setup. Settings → Setup also lets you re-check requirements and replay onboarding.

## What each surface shows

- **Projects and worktrees** — the sidebar lists your projects and, under each, its worktrees and their sessions. ＋ on a project creates a new worktree; removing a worktree deletes it together with its branch. ＋ on a worktree starts a Claude session there.
- **Terminals in tabs** — each Claude Code session, and any plain terminal, is a live terminal in its own tab.
- **Dashboard** — every session across all projects, grouped into *Needs you*, *Ready to advance*, *Running* and *Idle*, with a count of each. The **Fleet** view shows the same sessions as a graph.
- **Linear** — your assigned tickets. A ticket with no worktree has a **Start** button, as long as a project is open. It creates a worktree on a branch named for the lowercased ticket id (`abc-123`), runs your new worktree commands, and opens a Claude session seeded with the ticket's title and description. With a Linear team key set on the project, **Start** is hidden on tickets with another key; with none set, it shows on every ticket without a worktree. Tickets that already have a worktree show their session status and PR instead.
- **State** — a per-worktree tab showing the branch, its ticket when the branch carries a ticket id, its pull request and checks, git status and recent commits.
- **Settings → Projects** — two fields per project:
  - **Linear team key** — 2–6 letters or digits, starting with a letter (for example `ABC`). It turns on ticket links, ticket titles in tooltips and the State tab's ticket section for branches carrying that key's ids, and hides **Start** on tickets with a different key. Empty turns ticket links off, and **Start** shows on every ticket without a worktree.
  - **New worktree commands** — one command per line, run in each new worktree right after it is created, whether you made it with ＋ on a project or with a ticket's **Start**. They run in your login shell, joined with ` ; `: every command runs even if an earlier one fails, and only the last command's exit status counts. The 10-minute limit covers the whole run, not each command. Output streams live into a "setup" tab on the new worktree and is kept in a log under `~/.fleetmanager/bootstrap/`. Empty runs nothing. If the last command fails or the run times out, an error naming the log appears, and the worktree is still created. The commands don't run until you trust the repo, so FleetManager asks first. For a ticket's **Start**, the Claude session isn't launched until you answer; if you decline, the worktree is left with no session.
- **"Needs you" notifications** — when a session stops to wait for you, macOS shows a notification; clicking it takes you to that session. This needs session status detection turned on.

## Support

Email [hello@denwoodstudio.com](mailto:hello@denwoodstudio.com), or open an issue on the [releases page](https://github.com/denwood-studio/fleetmanager-releases/issues). If something misbehaves, open Settings, tick Enable debug log, and reproduce the problem. Then, under Diagnostics, click Copy diagnostics and paste the result into an email to us rather than a public issue. It covers versions, setup health and the debug log, never terminal output, file contents or your Linear key.
