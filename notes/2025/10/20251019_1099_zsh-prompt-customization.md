---
title: Zsh prompt customization quick reference
slug: zsh-prompt-customization
tags: [zsh, shell, reference]
description: Practical notes on building a useful PS1 without a framework
---

# Zsh prompt customization quick reference

Avoiding Oh My Zsh or Starship for a machine where I want a lean startup time. Building a prompt by hand is straightforward once you know the escape sequences.

## Key prompt variables

- `PROMPT` (or `PS1`) — left prompt
- `RPROMPT` — right-aligned prompt, cleared on command execution
- `PROMPT2` — continuation prompt (after `\`)
- `setopt PROMPT_SUBST` — required for variables and functions to expand inside the prompt

## Useful expansions

```
%n    username
%m    short hostname
%~    current directory (abbreviated $HOME to ~)
%?    exit status of last command
%!    current history number
%D{%H:%M}  time formatted with strftime
%(?.green.red)  conditional: if last exit was 0, green; else red
```

## Git status in prompt

A minimal approach using `vcs_info`:

```zsh
autoload -Uz vcs_info
precmd() { vcs_info }
zstyle ':vcs_info:git:*' formats ' (%b)'
PROMPT='%~ ${vcs_info_msg_0_}%# '
```

This is slower than caching but fine for repos under ~10k files.

## Timing commands

Wrap slow prompt expansions in `precmd` and cache the result in a variable. Avoid calling `git` twice. On a cold repo with 50k files, `git status` for the prompt can add 200ms.

Full reference for prompt escapes: https://zsh.sourceforge.io/Doc/Release/Prompt-Expansion.html

The `zsh/datetime` module gives `$EPOCHREALTIME` for sub-second timing in scripts without calling `date`.
