# Bash scripting reference

The parts I keep looking up.

## Script header and safety options

```bash
#!/usr/bin/env bash
set -euo pipefail
# -e: exit on error
# -u: error on undefined variables
# -o pipefail: pipe fails if any command fails
```

## Variables and strings

```bash
name="world"
echo "Hello, ${name}!"

# Default if unset
echo "${VAR:-default_value}"

# Assign if unset
: "${VAR:=default}"

# String length
echo "${#name}"

# Substring: ${var:offset:length}
echo "${name:0:3}"  # "wor"

# Pattern removal
file="archive.tar.gz"
echo "${file%%.*}"   # "archive" (greedy from right)
echo "${file%.*}"    # "archive.tar" (non-greedy)
echo "${file#*.}"    # "tar.gz" (from left)
```

## Conditionals

```bash
if [[ -f "$file" ]]; then
  echo "exists and is regular file"
elif [[ -d "$file" ]]; then
  echo "is directory"
fi

# Common tests
[[ -e path ]]    # exists
[[ -f path ]]    # regular file
[[ -d path ]]    # directory
[[ -z "$s" ]]    # string is empty
[[ -n "$s" ]]    # string is non-empty
[[ "$a" == "$b" ]]
[[ "$a" =~ ^[0-9]+$ ]]  # regex match
```

## Loops

```bash
for item in one two three; do
  echo "$item"
done

for file in *.txt; do
  echo "Processing: $file"
done

while read -r line; do
  echo "$line"
done < input.txt
```

## Functions

```bash
greet() {
  local name="$1"
  echo "Hello, ${name}"
}
greet "World"
```

## Arrays

```bash
arr=(one two three)
echo "${arr[0]}"         # first element
echo "${arr[@]}"         # all elements
echo "${#arr[@]}"        # length
arr+=("four")            # append
for item in "${arr[@]}"; do echo "$item"; done
```

## Error handling

```bash
command || { echo "command failed"; exit 1; }
trap 'echo "Error on line $LINENO"' ERR
```

Reference: https://www.gnu.org/software/bash/manual/bash.html
