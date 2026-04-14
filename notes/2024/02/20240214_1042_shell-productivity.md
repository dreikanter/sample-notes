---
title: Shell productivity patterns I actually use
slug: shell-productivity
tags: [linux, shell, workflow, terminal]
description: Practical shell habits accumulated over a few years of daily use
public: true
---

# Shell productivity patterns I actually use

Not a comprehensive guide — just the things that stuck after trying many more.

## History

```bash
# In .bashrc/.zshrc
HISTSIZE=100000
HISTFILESIZE=200000
# Don't record duplicates or lines starting with space
HISTCONTROL=ignoreboth
```

`Ctrl-r` for reverse search. Install `fzf` and source its keybindings file and `Ctrl-r` becomes dramatically more useful — fuzzy search through history with preview. Worth the five minutes of setup: https://github.com/junegunn/fzf

## Navigation

`cd -` switches to the previous directory. Simple, underused.

`pushd` / `popd` for a proper stack. I rarely use the stack directly but `pushd .` before going somewhere complicated means `popd` brings me back regardless of how many directories I wandered through.

## Parameter expansion

```bash
${var:-default}     # use default if var unset or empty
${var:?error msg}   # exit with error if unset
${var##*/}          # strip longest prefix matching */  (basename)
${var%.*}           # strip shortest suffix matching .* (remove extension)
```

These eliminate a surprising number of `basename`, `dirname`, and `sed` calls.

## Quick edits

```bash
fc          # open last command in $EDITOR
Ctrl-x e    # same, but mid-typing
```

tmux integration for persistent sessions: see [[20231129_1037]] for the full reference on that.

PostgreSQL JSONB notes from the same sprint: [[20240122_1040]].
