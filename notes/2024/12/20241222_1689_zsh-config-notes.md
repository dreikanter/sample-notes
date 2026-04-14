# Zsh configuration notes

Working notes on my shell setup. Everything lives in `~/.zshrc` and a few sourced files.

## Prompt customisation

Using [Starship](https://starship.rs) prompt rather than writing my own. Cross-shell, fast, and configurable in `~/.config/starship.toml`. My config shows: directory, git branch/status, language version (Node/Python), exit code of last command.

Compared to pure zsh prompt customisation (PROMPT variable with escape codes), Starship is much less painful to maintain.

## Aliases I actually use

```zsh
# Git
alias gs='git status'
alias gd='git diff'
alias gdc='git diff --cached'
alias gl='git log --oneline -20'
alias gp='git push'
alias gpl='git pull'

# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias ll='ls -lah'

# Dev
alias dc='docker compose'
alias tf='terraform'
alias k='kubectl'

# Quick edits
alias zshrc='$EDITOR ~/.zshrc && source ~/.zshrc'
```

## Useful zsh features worth enabling

```zsh
# History
HISTSIZE=10000
SAVEHIST=10000
setopt HIST_IGNORE_DUPS
setopt SHARE_HISTORY          # Share history between sessions

# Completion
autoload -Uz compinit
compinit
zstyle ':completion:*' menu select

# Directory navigation
setopt AUTO_CD                # Type directory name to cd
setopt AUTO_PUSHD             # Push to dir stack on cd
```

## Plugins (using Antidote as a manager)

- `zsh-autosuggestions`: suggests completions from history in grey
- `zsh-syntax-highlighting`: colours commands (red for invalid, green for valid)
- `zsh-history-substring-search`: Up/Down arrow searches history for substring

The [Zsh documentation](https://zsh.sourceforge.io/Doc/) is comprehensive but dense. For quick lookup, the [zsh-lovers man page](https://grml.org/zsh/zsh-lovers.html) is more practical.
