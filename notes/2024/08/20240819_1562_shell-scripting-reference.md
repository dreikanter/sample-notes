---
title: Shell scripting reference — patterns I use
slug: shell-scripting-reference
tags: [bash, shell, reference, cli]
description: Bash scripting patterns and idioms for everyday scripts
---

# Shell scripting reference — patterns I use

Not a tutorial. Patterns I reach for when writing scripts.

## Script header

```bash
#!/usr/bin/env bash
set -euo pipefail
```

- `-e`: exit on error
- `-u`: exit on undefined variable
- `-o pipefail`: fail pipe if any command in it fails

This should be in every script. Period.

## Default values

```bash
NAME="${1:-default}"          # use $1 or "default" if unset
DEBUG="${DEBUG:-false}"       # environment variable with fallback
```

## Check if a command exists

```bash
if ! command -v jq &>/dev/null; then
  echo "jq is required but not installed" >&2
  exit 1
fi
```

## Temp files that clean themselves up

```bash
TMPFILE=$(mktemp)
trap 'rm -f "$TMPFILE"' EXIT
```

The `trap` ensures cleanup even on error or interrupt.

## Looping over files safely

```bash
while IFS= read -r -d '' file; do
  process "$file"
done < <(find . -name "*.json" -print0)
```

The `-print0` / `-d ''` combination handles filenames with spaces.

## Script directory

```bash
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
```

Gives the directory where the script lives, not where it's called from.

## Coloured output

```bash
RED='\033[0;31m'
GREEN='\033[0;32m'
NC='\033[0m'  # No Color

echo -e "${GREEN}Success${NC}"
echo -e "${RED}Error${NC}" >&2
```

Reference: [BashPitfalls wiki](https://mywiki.wooledge.org/BashPitfalls)
