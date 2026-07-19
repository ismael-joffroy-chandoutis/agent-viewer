**English** · [Français](README.fr.md)

# Agent Viewer

A kanban board for managing multiple Claude Code agents running in tmux sessions. Spawn, monitor, and interact with agents from a single web UI.

<img src="monde.jpg" alt="agent-viewer" width="100%">

<img width="1466" height="725" alt="Screenshot 2026-02-09 at 14 54 21" src="https://github.com/user-attachments/assets/cd31b988-f649-4e92-9844-7a1ece9aa634" />

Manage your agents from your mobile phone with Tailscale

![IMG_7782](https://github.com/user-attachments/assets/c7298d61-dd37-4d0f-8b0a-d9d1f0231782)


## Prerequisites

- [Node.js](https://nodejs.org/) (v18+)
- [tmux](https://github.com/tmux/tmux)
- [Claude CLI](https://docs.anthropic.com/en/docs/claude-code) (`claude` command available in your PATH)

### Install prerequisites (macOS)

```bash
brew install node tmux
npm install -g @anthropic-ai/claude-code
```

## Setup

```bash
git clone <repo-url> && cd agent-viewer
npm install
```

## Usage

```bash
npm start
```

Open http://localhost:4200 in your browser.

### Configuration

| Variable | Default     | Description              |
|----------|-------------|--------------------------|
| `PORT`   | `4200`      | Server port              |
| `HOST`   | `localhost`  | Bind address (`0.0.0.0` for network access) |

Example:

```bash
HOST=0.0.0.0 PORT=3000 npm start
```

## Remote Access via Tailscale

You can access Agent Viewer from your phone (or any device) by using [Tailscale](https://tailscale.com/).

### 1. Install Tailscale on your Mac

```bash
brew install tailscale
```

Or download from [tailscale.com/download](https://tailscale.com/download).

### 2. Install Tailscale on your phone

Download the Tailscale app from the [App Store](https://apps.apple.com/app/tailscale/id1470499037) or [Google Play](https://play.google.com/store/apps/details?id=com.tailscale.ipn). Sign in with the same account.

### 3. Start the server

```bash
npm start
```

The server binds to `0.0.0.0` by default, so it's already accessible on all network interfaces including Tailscale.

### 4. Open on your phone

Find your Mac's Tailscale IP (shown in the Tailscale app or via `tailscale ip`), then visit:

```
http://<tailscale-ip>:4200
```

If you have [MagicDNS](https://tailscale.com/kb/1081/magicdns) enabled, you can use your machine name instead:

```
http://<machine-name>:4200
```

## Features

- **Spawn agents**: Click `[+ SPAWN]` or press `N`, enter a project path and prompt. Each agent launches in its own tmux session running `claude`.
- **Kanban columns**: Agents are sorted into Running, Idle, and Completed columns based on their state.
- **Auto-discovery**: Existing tmux sessions running Claude are automatically detected and added to the board.
- **Live output**: Click `VIEW OUTPUT` to see the full terminal output with ANSI color rendering.
- **Send messages**: Type in the prompt field on any card and press `Ctrl+Enter` to send follow-up messages to an agent.
- **File uploads**: Drag and drop files onto a card or click `FILE` to send files to an agent.
- **Re-spawn**: Completed agents can be re-spawned with a new prompt from the same project directory.
- **Attach**: Click `ATTACH` to copy the `tmux attach` command for direct terminal access.
- **Git branch detection**: Each card automatically shows the current git branch of the agent's working directory.
- **Notify on complete**: Check "NOTIFY ON COMPLETE" when spawning to trigger a system notification when the agent finishes (requires `~/.claude/scripts/notify.sh`).
- **Interactive prompts**: When an agent pauses on a plan approval or a numbered select menu, the card detects it, shows a `PLAN APPROVAL` or `SELECT` badge, and surfaces `↑ ↓ ENTER ESC` controls so you can navigate and answer without attaching to the terminal.
- **Plan feedback**: On a plan-approval prompt, type directly into the message field to route your text to Claude's "tell Claude what to change" option instead of approving as-is.
- **Stop**: Click `STOP` to interrupt a running agent.

### Interactive prompts

When an agent stops to ask a question, the card mirrors the prompt and gives you the keys to answer it.

A plan-approval prompt on a card, with the `PLAN` navigation controls:

![Plan approval on a card](docs/screenshots/plan-card.png)

A numbered select menu surfaced on a card, with `SELECT` controls:

![Select prompt on a card](docs/screenshots/select-card.png)

The same prompts, seen in full inside the output viewer:

![Plan approval in the output viewer](docs/screenshots/plan-output-viewer.png)

![Select prompt in the output viewer](docs/screenshots/select-output-viewer.png)

## License

Licensed under Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0). See [LICENSE.md](LICENSE.md). To cite this work, see [CITATION.cff](CITATION.cff) or use GitHub's "Cite this repository" button.

## Development

Architecture, core systems, and internals are documented in [CLAUDE.md](CLAUDE.md), which also guides Claude Code when working in this repository.

By [Ismaël Joffroy Chandoutis](https://ismaeljoffroychandoutis.com).
