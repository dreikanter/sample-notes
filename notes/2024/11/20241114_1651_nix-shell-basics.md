---
title: Nix shell basics for project environments
slug: nix-shell-basics
tags: [nix, devtools, programming]
description: Getting started with nix-shell for reproducible dev environments.
public: true
---

# Nix shell basics for project environments

I've been using nix-shell for project-level environments as an alternative to Docker for development. The [Nix documentation](https://nix.dev/tutorials/first-steps/nix-shell) covers the fundamentals; this is my working notes.

## Why bother

A `shell.nix` file pins the exact versions of all tools (Node.js, Python, database clients, etc.) needed for a project. Anyone who has Nix installed runs `nix-shell` and gets the exact same environment. No Docker daemon required, no `brew install` drift.

## Basic shell.nix

```nix
{ pkgs ? import <nixpkgs> {} }:

pkgs.mkShell {
  buildInputs = with pkgs; [
    nodejs_20
    nodePackages.pnpm
    postgresql_16
    python311
    git
  ];

  shellHook = ''
    export DATABASE_URL="postgresql://localhost/myapp_dev"
    echo "Dev environment ready"
  '';
}
```

Run with `nix-shell` in the project directory. Exit with `exit` or Ctrl+D.

## With direnv (highly recommended)

Add `.envrc`:
```bash
use nix
```

Then `direnv allow`. Now the shell activates automatically when you `cd` into the directory and deactivates when you leave.

## Flakes (more modern)

Flakes pin nixpkgs to a specific commit, giving true reproducibility across machines and time. Slightly more setup but worth it for shared projects.

```nix
# flake.nix
{
  inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixos-24.05";
  outputs = { self, nixpkgs }: {
    devShells.x86_64-linux.default = nixpkgs.legacyPackages.x86_64-linux.mkShell {
      packages = with nixpkgs.legacyPackages.x86_64-linux; [ nodejs_20 git ];
    };
  };
}
```

Then `nix develop` instead of `nix-shell`.

## Practical notes

- Finding packages: `nix search nixpkgs <name>` or [search.nixos.org](https://search.nixos.org/packages)
- Nix is not a replacement for runtime isolation (that's Docker); it's a replacement for the tool installation layer
