# Bash scripting patterns

Things I keep having to look up when writing shell scripts.

## Script header

```bash
#!/usr/bin/env bash
set -euo pipefail
```

- `-e`: exit on any error
- `-u`: treat unset variables as errors
- `-o pipefail`: pipe fails if any command in the pipeline fails

Add `set -x` for debugging (prints each command before running).

## Variables and quoting

```bash
NAME="Alice"
echo "$NAME"      # correct — always quote variable references
echo "${NAME}!"   # correct — braces for disambiguation
echo $NAME        # works but risky with spaces or special chars
```

Default values:
```bash
HOST="${HOST:-localhost}"   # use HOST if set, else "localhost"
PORT="${PORT:=8080}"        # same but also assigns the variable
```

## Conditionals

```bash
if [[ -f "$file" ]]; then echo "file exists"; fi
if [[ -d "$dir" ]]; then echo "directory exists"; fi
if [[ -z "$var" ]]; then echo "var is empty"; fi
if [[ -n "$var" ]]; then echo "var is not empty"; fi
if [[ "$a" == "$b" ]]; then echo "equal"; fi
```

Use `[[` (double bracket) over `[` — it's safer, supports `&&` and `||` inside.

## Loops

```bash
for f in *.txt; do
    echo "Processing $f"
done

while IFS= read -r line; do
    echo "$line"
done < input.txt
```

## Functions

```bash
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" >&2
}

die() {
    log "ERROR: $*"
    exit 1
}
```

## Argument handling

```bash
usage() { echo "Usage: $0 [-v] <input> <output>"; exit 1; }
VERBOSE=0
while getopts "vh" opt; do
    case $opt in
        v) VERBOSE=1 ;;
        h) usage ;;
        *) usage ;;
    esac
done
shift $((OPTIND-1))
INPUT="${1:?Input required}"
OUTPUT="${2:?Output required}"
```

Reference: https://www.shellcheck.net/ — run all your scripts through this.
Guide: https://google.github.io/styleguide/shellguide.html
