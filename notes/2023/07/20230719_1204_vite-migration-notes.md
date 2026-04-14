---
title: Vite migration — implementation notes
slug: vite-migration-notes
tags: [frontend, vite, migration]
description: Day-by-day notes from migrating the frontend from Webpack 5 to Vite 4.
---

# Vite migration — implementation notes

Implementation started after SSR risk assessment (low risk in our case — SSR is a small part of the codebase). See [[20230602_1174]] for the decision notes.

**Day 1**

Installed Vite and `@vitejs/plugin-react`. Created `vite.config.ts` alongside the existing `webpack.config.js` — running both in parallel to compare output.

Main issue: 14 npm packages using CommonJS-only modules. Added `optimizeDeps.include` for each in the Vite config. Two packages needed explicit `ssr.noExternal` entries for the SSR paths.

**Day 2**

Converted `process.env.X` references to `import.meta.env.X` throughout the codebase. Scripted this with `sed` initially, then cleaned up manually. Env variables that need to be exposed to the browser must be prefixed with `VITE_` — this broke two staging-environment checks that used internal variable names.

Custom Webpack loaders: two in total. One (for SVG handling) was replaceable with `vite-plugin-svgr`. One (a custom YAML loader for i18n files) needed a new Vite plugin — wrote a simple Rollup plugin wrapper, 40 lines.

**Day 3**

All tests green. Dev server cold start confirmed: 1.1 seconds (was 18.3 s). HMR: near-instantaneous.

Build output comparison: Vite's prod build is slightly smaller (3% reduction in bundle size — the tree-shaking is more aggressive). Build time is comparable.

Removed the Webpack config. Archived it in git history.

**Result**

Successful migration. Total effort: 3 days. The main wins are the dev server speed and HMR — these improve the daily experience significantly.

[Vite migration from Webpack guide](https://vitejs.dev/guide/migration-from-v2.html)
