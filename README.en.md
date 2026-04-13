# devcontainer-with-claude

> [Leia em Português](README.md)

Dev Container template for Python and Node.js projects with Claude Code integrated.

## What is this

Dev Containers let you run a complete development environment inside a Docker container, ensuring everyone who clones the repo gets the exact same setup — no more "works on my machine".

This template goes further: it comes with [Claude Code](https://claude.ai/code) pre-installed and configured, mounting your local Claude credentials and memory directly into the container. The AI assistant is ready to go the moment the container starts, with all the context you already have on your host machine.

## Why use this

Every time I start a new project, I want to hit the ground running with Claude already configured — no time wasted installing Node, authenticating Claude Code, or copying settings manually. This template solves that: clone it, open in VSCode, container starts, Claude works. In minutes the project is ready for AI-assisted development.

Another key reason: safely using `claude --dangerously-skip-permissions`. This flag removes all permission prompts from Claude Code, letting it operate fully autonomously — but running it directly on the host machine is risky, as Claude can modify files and execute commands without asking. Inside the container the risk is contained: whatever happens in the container stays in the container, keeping the host system safe.

The container doesn't expose SSH keys, so Claude can commit but **cannot push** — autonomous operations stay contained in the local environment.

## What's included

- **Python 3.12** as the base image
- **Node.js + npm** for development and to run Claude Code
- **Claude Code** installed automatically via `postCreateCommand`
- Mounts to share host configurations:
  - `~/.claude` — Claude settings and memory
  - `~/.claude.json` — Claude Code credentials
  - `~/.gitconfig` — git configuration (name and email for commits)
- VSCode extensions: Python + Pylance

## How to use

1. Copy the `.devcontainer/` folder to your project
2. Replace `<NAME-OF-PROJECT>` with your project name in `devcontainer.json`
3. Open the project in VSCode and click **Reopen in Container**

> If the notification doesn't appear, open the command palette (`Ctrl+Shift+P` / `Cmd+Shift+P`) and run:
> `Dev Containers: Reopen in Container`

**Quick tip:** to download just the `devcontainer.json` without cloning the repository, run at the root of your project:

```bash
mkdir -p .devcontainer && curl -sL https://raw.githubusercontent.com/brlga002/devcontainer-with-claude/main/.devcontainer/devcontainer.json -o .devcontainer/devcontainer.json
```

## Customization

### Run `npm install` automatically when the container starts

By default the `postCreateCommand` only installs Claude Code. If your Node.js project needs dependencies installed automatically, edit the field in `devcontainer.json`:

```json
"postCreateCommand": "npm install && npm install -g @anthropic-ai/claude-code"
```

Or combined with Python:

```json
"postCreateCommand": "pip install -r requirements.txt 2>/dev/null || true && npm install && npm install -g @anthropic-ai/claude-code"
```

## Prerequisites

- [Docker](https://www.docker.com/)
- [VSCode](https://code.visualstudio.com/) with the [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension
- Claude Code authenticated on the host machine (`~/.claude.json` must exist)
