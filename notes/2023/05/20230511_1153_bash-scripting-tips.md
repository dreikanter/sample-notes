---
title: Bash scripting — things I keep forgetting
slug: bash-scripting-tips
tags: [bash, scripting, reference]
---

# Bash scripting — things I keep forgetting

**Set flags at the top**

```bash
#!/usr/bin/env bash
set -euo pipefail
```

`-e`: exit on error. `-u`: error on undefined variable. `-o pipefail`: pipe failure propagates. These three together catch most silent failures.

**Safe variable expansion**

```bash
dir="${1:-/tmp}"  # use /tmp as default if $1 not provided
echo "${var}"     # always quote; ${} over $
```

**Check if a command exists**

```bash
if ! command -v jq &>/dev/null; then
    echo "jq not found" >&2; exit 1
fi
```

**Temporary file / directory**

```bash
tmpdir=$(mktemp -d)
trap 'rm -rf "$tmpdir"' EXIT
```

The `trap` on EXIT ensures cleanup even if the script errors.

**Read file line by line**

```bash
while IFS= read -r line; do
    echo "$line"
done < input.txt
```

`IFS=` prevents stripping leading/trailing whitespace. `-r` prevents backslash interpretation.

**Array handling**

```bash
items=("one" "two" "three")
for item in "${items[@]}"; do echo "$item"; done
echo "${#items[@]}"  # count
```

**heredoc**

```bash
cat <<'EOF'
This text is literal — no variable expansion.
EOF
```

Use `<<EOF` (without quotes) to allow variable expansion.

Full reference: [Bash manual](https://www.gnu.org/software/bash/manual/bash.html) and [shellcheck.net](https://www.shellcheck.net) for linting.
