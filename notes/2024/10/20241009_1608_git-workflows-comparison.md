# Git workflow comparison — trunk-based vs gitflow

The team had a discussion about our branching strategy. Notes on the tradeoffs.

## Trunk-based development

Everyone integrates to main (or a short-lived feature branch off main, merged within a day or two). No long-running branches.

**Requires:**
- Feature flags to hide incomplete work in production
- Good CI/CD pipeline — you're integrating frequently
- Small, atomic commits with descriptive messages
- Trust that the test suite catches regressions

**Benefits:**
- Continuous integration (actual integration, not just the tool)
- Merge conflicts are tiny and frequent rather than large and infrequent
- Deployment is simple — main is always releasable
- Aligns with modern delivery practices

**Good for:** Teams shipping software as a service, frequent deployment cadence, small-to-medium teams.

## GitFlow

Long-running develop, main, release, hotfix, and feature branches with defined merge paths.

**Benefits:**
- Explicit release process — good for scheduled releases
- Hotfix path is clear
- Suitable for teams maintaining multiple production versions

**Costs:**
- Merge conflicts accumulate between long-running branches
- Integration happens late (at merge time, not during development)
- Overhead of maintaining branch discipline

**Good for:** Software with versioned releases (libraries, packaged software), large distributed teams.

## What we do and what we should do

We ship continuously to a SaaS product. GitFlow is unnecessary overhead. We've been doing something between the two — feature branches living 3-10 days — which carries the costs of both without the full benefits of either.

My recommendation: move toward trunk-based with feature flags. The [Trunk-Based Development site](https://trunkbaseddevelopment.com/) has good practical guidance.
