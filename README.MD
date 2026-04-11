<div align="center">

<br>

```
     █████╗ ██████╗ ███████╗
    ██╔══██╗██╔══██╗██╔════╝
    ███████║██║  ██║█████╗  
    ██╔══██║██║  ██║██╔══╝  
    ██║  ██║██████╔╝███████╗
    ╚═╝  ╚═╝╚═════╝ ╚══════╝
```

### **Agentic Dev Environment**

*Run multiple AI agents side-by-side in the same repo.*

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)
[![Electron](https://img.shields.io/badge/electron-41-47848F.svg?style=flat-square&logo=electron&logoColor=white)](https://electronjs.org)
[![node-pty](https://img.shields.io/badge/node--pty-real%20terminals-333.svg?style=flat-square)](https://github.com/microsoft/node-pty)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](CONTRIBUTING.md)

<br>

<sub>A desktop workspace for running Claude Code, Codex, and any CLI agent<br>in a grid of real terminal panes — all sharing the same project.</sub>

<br>

---

</div>

<br>

## The idea

Most AI coding tools give you **one** assistant. ADE gives you a **grid** of them.

Open a project, split it into terminal panes, and run a different agent in each one. One debugs a pricing bug, another refactors your filters, another writes docs, another investigates why staging broke. All at the same time. All in the same repo.

```
 ╭─ skinmerge ─────────────────────────────────────────────────────╮
 │                                                                  │
 │  ┌─ claude code ──────┐  ┌─ codex ───────────┐  ┌─ bash ──────┐ │
 │  │                    │  │                    │  │              │ │
 │  │  fixing StatTrak   │  │  writing API docs  │  │  npm test    │ │
 │  │  pricing bug...    │  │  for /trade-up     │  │  ✓ 14 pass  │ │
 │  │                    │  │                    │  │              │ │
 │  ├─ claude code ──────┤  ├─ codex ───────────┤  ├─ git ───────┤ │
 │  │                    │  │                    │  │              │ │
 │  │  refactoring       │  │  writing tests     │  │  5 files     │ │
 │  │  filter pipeline   │  │  for auth module   │  │  changed     │ │
 │  │                    │  │                    │  │              │ │
 │  └────────────────────┘  └────────────────────┘  └──────────────┘ │
 │                                                                  │
 ╰──────────────────────────────────────────────────────────────────╯
```

<br>

## Quick start

```bash
git clone https://github.com/user/Agentic-Development-Environment.git
cd Agentic-Development-Environment
npm install
npm run dev
```

> [!NOTE]
> Requires **Node.js 18+** and a C++ toolchain for node-pty.
> See [node-gyp installation](https://github.com/nodejs/node-gyp#installation) if `npm install` fails.

<br>

## Features

<table>
<tr>
<td width="50%">

### Workspace tabs
Each tab is a project. Create a workspace, pick a directory, choose your grid size (2–9 panes). Switch tabs — terminals stay alive in the background.

</td>
<td width="50%">

### Real terminals
Not a chat UI. Every pane is a full PTY via **node-pty** + **xterm.js**. Full color, scrollback, resize. Run any CLI tool.

</td>
</tr>
<tr>
<td width="50%">

### Voice typing
Speak into the focused pane. Uses the Web Speech API. Toggle with <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>V</kbd>.

</td>
<td width="50%">

### Cross-platform
Automatically detects available shells. PowerShell, WSL, and cmd on Windows. zsh and bash on macOS/Linux.

</td>
</tr>
</table>

<br>

## How it works

```
  ┌─────────────────────────────────────────────────────┐
  │  renderer.js                                        │
  │  workspace state, xterm instances, tab management   │
  │                                                     │
  │         ▼ IPC (preload.js)  ▲                       │
  ├─────────────────────────────────────────────────────┤
  │  electron/main.js                                   │
  │  window lifecycle, IPC routing                      │
  │                                                     │
  │         ▼                                           │
  ├─────────────────────────────────────────────────────┤
  │  terminal-manager.js                                │
  │  shell detection, PTY spawning, session lifecycle   │
  │                                                     │
  │         ▼                                           │
  │  node-pty  ──────►  real shell processes            │
  └─────────────────────────────────────────────────────┘
```

<br>

## Project structure

```
electron/
├── main.js                 app lifecycle, IPC handlers
├── preload.js              context bridge (renderer ↔ main)
└── terminal-manager.js     PTY spawning, shell profile detection

backend/src/
├── contracts.ts            types, interfaces, port definitions
├── WorkspaceBackend.ts     orchestrator (projects, sessions, git, voice)
└── platform.ts             OS detection, shell resolution, path helpers

index.html                  UI shell
renderer.js                 frontend (workspaces, tabs, terminal grid)
```

<br>

## Tech

| | |
|:--|:--|
| **Desktop** | Electron |
| **Terminals** | xterm.js + node-pty |
| **Frontend** | Vanilla JS — no framework, no build step |
| **Backend** | TypeScript with ports & adapters |
| **Packaging** | electron-builder (Windows NSIS, macOS DMG) |

<br>

## Roadmap

| Status | Feature |
|:------:|---------|
| ✅ | Workspace tabs with real terminal grids |
| ✅ | PTY shell detection (PowerShell, WSL, cmd, zsh, bash) |
| ✅ | Voice typing via Web Speech API |
| ✅ | Native folder picker for workspace directory |
| 🔲 | Provider selector per pane (Claude Code / Codex / shell) |
| 🔲 | Git panel — branch, diffs, staging, commits |
| 🔲 | Chat history persistence |
| 🔲 | Pane resize and drag-to-rearrange |
| 🔲 | Agent file attribution (which agent changed what) |
| 🔲 | Settings and preferences |
| 🔲 | Plugin system |

<br>

## Contributing

PRs welcome. Fork it, branch it, change it, open a PR.

If you're using an AI agent to contribute, read **[`AGENTS.md`](AGENTS.md)** first — it has architecture context, conventions, and guardrails so your agent doesn't go off the rails.

<br>

## Building

```bash
# development
npm run dev

# package for distribution
npm run dist:win     # → Windows NSIS installer
npm run dist:mac     # → macOS DMG
```

<br>

---

<div align="center">
<sub>Built for the workflow where one agent isn't enough.</sub>
<br><br>
<a href="https://github.com/user/Agentic-Development-Environment/issues">Report a bug</a> · <a href="https://github.com/user/Agentic-Development-Environment/issues">Request a feature</a>
</div>
