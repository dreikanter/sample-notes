---
title: Terminal setup — what I settled on after too many experiments
slug: terminal-setup
tags: [tools, terminal, setup]
description: My current terminal configuration choices and why
public: true
---

# Terminal setup — what I settled on after too many experiments

After several years of switching between tools, I've landed on a stable setup and stopped experimenting.

**Shell: Zsh with minimal configuration**

I tried Fish for about six months. The auto-suggestions and syntax highlighting are excellent but the POSIX incompatibility caused friction too often when copying commands from documentation. Zsh with zsh-autosuggestions and zsh-syntax-highlighting installed gets me most of what Fish offers without the compatibility issues.

I don't use Oh My Zsh. It adds startup latency and I only used about 3% of its plugins. Instead: a small .zshrc, the two plugins above, and a simple prompt.

**Prompt: Starship**

Starship is cross-shell, fast, and configured via TOML. I use a stripped-down config that shows: directory, git branch/status, command execution time for commands over 5 seconds. Nothing else.

**Terminal emulator: Ghostty**

Switched from iTerm2 about six months ago. Faster, simpler, the font rendering is excellent. Supports proper color profiles. The configuration file is straightforward. Main advantage: it's noticeably snappier on heavy output (log streaming, etc.).

**Multiplexer: tmux**

For anything involving multiple panes or sessions. My config is minimal — changed the prefix to `Ctrl-a`, enabled mouse mode for pane resizing, set the status bar to be less visually noisy. Plugins: tmux-resurrect for session persistence.

**Key bindings I rely on**

`Ctrl-r` for history search (enhanced by fzf). That's basically it.

Starship docs: https://starship.rs/config/
