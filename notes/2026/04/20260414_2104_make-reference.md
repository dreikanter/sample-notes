---
title: Make reference for Go projects
slug: make-reference
tags: [make, go, dev, cheatsheet]
description: A practical Makefile pattern for Go projects
---

# Make reference for Go projects

I use a Makefile in most projects for consistency. Here's the pattern I've settled on.

## Basic Go project Makefile

```makefile
.PHONY: build test lint clean run

BINARY_NAME := myapp
BUILD_DIR := ./build
MAIN_PATH := ./cmd/server

# Build
build:
	go build -o $(BUILD_DIR)/$(BINARY_NAME) $(MAIN_PATH)

# Run locally
run:
	go run $(MAIN_PATH)

# Test
test:
	go test ./... -race -count=1

test-verbose:
	go test ./... -v -race -count=1

# Coverage
cover:
	go test ./... -coverprofile=coverage.out
	go tool cover -html=coverage.out

# Lint (requires golangci-lint)
lint:
	golangci-lint run ./...

# Format
fmt:
	gofmt -w .
	goimports -w .

# Clean
clean:
	rm -rf $(BUILD_DIR)
	rm -f coverage.out

# Install tools
tools:
	go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
	go install golang.org/x/tools/cmd/goimports@latest
```

## Why `.PHONY`

Without `.PHONY`, Make checks whether a file named `test` exists before running the `test` target. If it exists, Make thinks the target is up-to-date and skips it. `.PHONY` tells Make these are always commands, not file targets.

## Variables and environment

```makefile
# Default value, overridable at command line
PORT ?= 8080
DB_PATH ?= ./data.db

run:
	PORT=$(PORT) DB_PATH=$(DB_PATH) go run $(MAIN_PATH)
```

Run with: `make run PORT=3000`

## Automatic variables

```
$@   — target name
$<   — first prerequisite
$^   — all prerequisites
```

GNU Make docs: https://www.gnu.org/software/make/manual/make.html
Good intro: https://makefiletutorial.com
