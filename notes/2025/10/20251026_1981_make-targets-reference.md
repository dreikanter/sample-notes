---
title: Makefile targets reference — common patterns
slug: make-targets-reference
tags: [make, build, reference, devtools]
---

# Makefile targets reference — common patterns

I use Makefiles as project task runners even when the project isn't C. The recipe-based structure is useful; the dependency tracking is occasionally useful; the ubiquity means anyone on the team can use it without installing anything.

## Basic structure

```makefile
.PHONY: build test lint clean

build:
	go build ./...

test:
	go test ./... -race

lint:
	golangci-lint run

clean:
	rm -rf dist/
```

`.PHONY` declares targets that aren't file names—without it, make would check for a file named `test` and skip the rule if it exists.

## Variables

```makefile
IMAGE_NAME := myapp
VERSION    := $(shell git describe --tags --always)

docker-build:
	docker build -t $(IMAGE_NAME):$(VERSION) .
```

`:=` expands immediately; `=` expands lazily (at use). For shell commands, always use `:=`.

## Pattern rules

```makefile
%.html: %.md
	pandoc -o $@ $<
```

`$@` = target, `$<` = first prerequisite, `$^` = all prerequisites.

## Conditional execution

```makefile
ifeq ($(CI),true)
  TEST_FLAGS := -v -timeout 120s
else
  TEST_FLAGS :=
endif
```

## A useful default target

```makefile
.DEFAULT_GOAL := help

help:  ## Show this help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | \
		awk 'BEGIN {FS = ":.*?## "}; {printf "  %-20s %s\n", $$1, $$2}'
```

Add `## Description` comments after target colons for self-documenting Makefiles.

Reference: [GNU make documentation](https://www.gnu.org/software/make/manual/)
