---
title: CSS architecture — notes on what I've actually used
slug: css-architecture
tags: [css, web, architecture]
description: Honest notes on CSS methodology experience
---

# CSS architecture — notes on what I've actually used

Having used BEM, CSS Modules, utility-first (Tailwind), and CSS-in-JS (styled-components) across different projects, these are my honest assessments.

**BEM**

Works well for medium-sized projects without a build step. The naming convention is verbose but enforces a discipline that prevents a lot of specificity problems. The `.block__element--modifier` convention makes the structure of the HTML readable from the CSS alone.

Problems: verbose class names are annoying to type. Inheritance between components is awkward. Doesn't solve the "dead CSS" problem — you don't know what's safe to delete.

**CSS Modules**

Class names are locally scoped per file. What looks like `.card` in `Card.module.css` compiles to `.Card_card_3a8f2`. This solves the global namespace problem elegantly. Works particularly well in component-based JavaScript frameworks.

Problem: still requires design discipline. CSS Modules give you scope, not structure.

**Tailwind**

I resisted it for a long time. Now I use it by default for new projects. The utility-first approach feels wrong until you've used it for a week and then it feels right. HTML gets verbose, but you never touch a CSS file and never worry about naming.

The main real cost: visual design velocity requires knowing the utility class names, which takes a few weeks to internalize. The Tailwind docs are excellent.

**CSS-in-JS**

Styled-components works and solves some real problems (colocated styles, prop-driven styling). The runtime cost is real though, especially at scale. I've moved away from it for new projects.

Tailwind docs: https://tailwindcss.com/docs
