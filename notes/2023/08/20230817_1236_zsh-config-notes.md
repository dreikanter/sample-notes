---
title: Zsh config notes and useful patterns
slug: zsh-config-notes
tags: [shell, zsh, reference]
---

# Zsh config notes and useful patterns

Reference: https://zsh.sourceforge.io/Doc/Release/zsh_toc.html

## History configuration

```zsh
HISTFILE=~/.zsh_history
HISTSIZE=50000
SAVEHIST=50000
setopt SHARE_HISTORY          # share history across sessions
setopt HIST_IGNORE_DUPS       # don't record duplicate adjacent commands
setopt HIST_IGNORE_SPACE      # commands starting with space not recorded
setopt HIST_REDUCE_BLANKS     # trim blanks from history entries
```

## Directory navigation

```zsh
setopt AUTO_CD          # type directory name without cd
setopt AUTO_PUSHD       # cd pushes old directory onto stack
setopt PUSHD_IGNORE_DUPS
DIRSTACKSIZE=15
```

`dirs -v` shows the stack. `cd -<n>` or `~<n>` jumps to position n.

## Glob extensions

```zsh
setopt EXTENDED_GLOB

# Files modified in last 24 hours
ls *(.m-1)

# Directories only
ls -d **/*(/)

# Empty files
ls *(.L0)
```

## Useful aliases I keep

```zsh
alias ll='ls -lah'
alias g='git'
alias gs='git status'
alias gd='git diff'
alias be='bundle exec'
alias k='kubectl'
alias py='python3'
```

## Functions

```zsh
# Create directory and cd into it
mkcd() { mkdir -p "$1" && cd "$1" }

# Show PATH entries one per line
path() { echo $PATH | tr ':' '\n' }
```

## Oh My Zsh vs. Zsh from scratch

After years with OMZ, I stripped back to a minimal `.zshrc` plus Starship for the prompt. Startup time went from ~800ms to ~120ms. Worth the setup time.

Starship: https://starship.rs/
