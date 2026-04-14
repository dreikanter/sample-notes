---
title: Vite vs Webpack — decision notes
slug: vite-vs-webpack
tags: [frontend, build-tools, javascript]
---

# Vite vs Webpack — decision notes

Evaluating whether to migrate the frontend project from Webpack 5 to Vite. Notes from a day of research and experimentation.

**Why consider migration**

Dev server cold start: Webpack rebuilds the whole bundle; Vite serves ES modules natively through the browser and only transforms what's requested. On our project, cold start is ~18 seconds in Webpack; Vite benchmark on a similar codebase size is ~0.5 seconds.

HMR: Vite's HMR is faster because it only invalidates the module that changed. Webpack HMR is more complex and slower on large trees.

**What makes migration non-trivial**

- Our Webpack config has significant custom loaders for our internal asset pipeline
- Several dependencies use CommonJS-only modules and need `@vitejs/plugin-legacy`
- The Webpack `DefinePlugin` usage needs to be converted to `import.meta.env` (Vite convention)
- We have a non-standard project structure (monorepo) that Vite handles differently

**Test results on a branch**

Set up Vite on a feature branch excluding the custom loaders. Dev server cold start: 1.2 seconds (was 18 s). HMR: near-instantaneous. Build time: comparable to Webpack for prod.

**Decision**

Worth migrating, but not trivial. Estimate 3–5 days to do it properly. Will raise in next sprint planning.

**Concerns**

SSR mode in Vite (using `vite-plugin-ssr`) is less mature than Webpack's SSR story. Our SSR usage is limited but it's a risk factor.

[Vite documentation](https://vitejs.dev/guide/)
