# Zsh config notes and useful snippets

Notes from cleaning up my `.zshrc` this weekend. Some things I keep forgetting.

## Useful built-ins

```zsh
# setopt to enable useful options
setopt AUTO_CD          # cd by typing directory name
setopt HIST_IGNORE_DUPS # don't store duplicate history entries
setopt SHARE_HISTORY    # share history across sessions
setopt EXTENDED_GLOB    # enable ** and other glob extensions

# Word deletion behaves sanely
bindkey '^W' backward-kill-word
```

## History configuration

```zsh
HISTFILE="$HOME/.zsh_history"
HISTSIZE=10000
SAVEHIST=10000
```

## Useful aliases

```zsh
alias ll='ls -lAh --color=auto'
alias gst='git status'
alias gcm='git commit -m'
alias gco='git checkout'
alias ..='cd ..'
alias ...='cd ../..'
```

## Functions I actually use

```zsh
# Create directory and cd into it
mkcd() { mkdir -p "$1" && cd "$1" }

# Search command history with fzf
fh() { print -z "$(history -1 0 | fzf --tac | sed 's/ *[0-9]* *//')" }
```

## Plugins I use

- **zsh-autosuggestions** — suggests from history as you type, accept with →
- **zsh-syntax-highlighting** — syntax colors in real time; makes typos obvious
- **fzf** integration — fuzzy search for files, history, directories

## Resources

- [zsh documentation](https://zsh.sourceforge.io/Doc/)
- [GitHub: ohmyzsh](https://github.com/ohmyzsh/ohmyzsh) — useful to read even if you don't use the framework, because the source shows good patterns

