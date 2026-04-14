# Bash scripting patterns

Useful patterns for scripts that go slightly beyond simple one-liners.

## Script preamble

```bash
#!/usr/bin/env bash
set -euo pipefail

# -e: exit on error
# -u: treat unset variables as errors
# -o pipefail: pipe fails if any component fails
```

## Argument parsing

```bash
USAGE="Usage: $0 [--env ENV] [--dry-run] FILE"

DRY_RUN=false
ENV="production"
FILE=""

while [[ $# -gt 0 ]]; do
    case $1 in
        --env)        ENV="$2"; shift 2 ;;
        --dry-run)    DRY_RUN=true; shift ;;
        -h|--help)    echo "$USAGE"; exit 0 ;;
        -*)           echo "Unknown option: $1" >&2; exit 1 ;;
        *)            FILE="$1"; shift ;;
    esac
done

[[ -z "$FILE" ]] && { echo "Error: FILE required" >&2; exit 1; }
```

## Logging functions

```bash
log()  { echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*"; }
warn() { echo "[WARN] $*" >&2; }
die()  { echo "[ERROR] $*" >&2; exit 1; }
```

## Check command availability

```bash
require() {
    command -v "$1" &>/dev/null || die "$1 is not installed"
}
require curl
require jq
```

## Safe temp directory

```bash
TMPDIR=$(mktemp -d)
trap 'rm -rf "$TMPDIR"' EXIT
```

## String operations

```bash
# Lower/uppercase (bash 4+)
lower="${VAR,,}"
upper="${VAR^^}"

# Check if string contains substring
[[ "$haystack" == *"$needle"* ]] && echo "found"

# Strip prefix/suffix
var="${path#*/}"    # remove shortest prefix matching */
var="${path##*/}"   # remove longest prefix (basename)
var="${file%.txt}"  # remove suffix
```

Advanced guide: [https://www.gnu.org/software/bash/manual/bash.html](https://www.gnu.org/software/bash/manual/bash.html)
