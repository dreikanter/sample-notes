---
title: Sprint review — early March
slug: sprint-review-march
tags: [work, sprint, review]
---

# Sprint review — early March

First sprint of March closes Friday. Review notes.

## Completed

**On-call runbook v1.0** — merged after Devlin's additions (see [[20240302_1380]]). Now living in the repository with the rest of the operations documentation. It will rot without maintenance but at least it exists.

**Read replica stability** — no issues in three weeks of production. The billing run (our heaviest write load) ran without replication lag exceeding 2.8 seconds. Declared stable.

**Feature flag investigation** — Marcus completed a proof of concept over the week. The library choice came down to Unleash (self-hosted) versus LaunchDarkly (managed). Cost, operational simplicity, and our scale point to Unleash. Marcus is writing the evaluation memo.

## In progress

**OpenTelemetry traces** — started the integration but ran into a library version conflict with our Go dependencies. Blocked for a day; resolved Thursday. Should complete next sprint.

**Q2 planning document** — draft exists, needs the product conversation before it's final. Scheduled for Thursday.

## What didn't move

The CLI note-search tool. Not a sprint item — personal project — but noting it here because I keep moving it forward without it moving.

## Observation

Sprints with one major infrastructure item and several smaller ones are the right size. Sprints that are all maintenance feel like treading water. Sprints that are all greenfield are exhausting. This one had the right mix.

Reference for the feature flag evaluation: [OpenFeature specification](https://openfeature.dev/) as an alternative to vendor-specific SDKs.
