---
title: Infrastructure migration proposal — concerns and questions
slug: infra-migration-concerns
tags: [work, infrastructure, planning]
description: Written response to the infra team's proposed migration timeline.
---

# Infrastructure migration proposal — concerns and questions

This is my written response to the migration proposal shared on August 20. Posting here before sending to give myself time to review the reasoning.

## What the proposal says

Migrate all services from ECS Fargate to EKS (Kubernetes) by end of Q4 2023. Estimated effort: six engineer-weeks. Estimated downtime during migration: zero (blue-green deployment strategy).

## My concerns

**1. Timeline is aggressive**

Six engineer-weeks sounds reasonable until you account for: the three services that have no current deployment documentation, the auth service that has a custom network configuration that hasn't been touched in 18 months and nobody fully understands, and the fact that we have a major product launch planned for mid-November.

I'd propose Q1 2024 for the bulk of migration, with a pilot service moved in Q4 to validate assumptions.

**2. Kubernetes operational burden**

ECS is operationally simple. Kubernetes is powerful but it requires a maintained understanding of the control plane, RBAC, networking model, and ongoing version upgrades. Who on the team will own Kubernetes expertise? This question needs an answer before we start.

**3. "Zero downtime" assumption**

Blue-green works if you have complete confidence in the staging environment. We don't — our staging environment has known configuration drift from production. I'd feel better with a maintenance window than with an untested zero-downtime migration for the payment service.

## What I support

- Evaluating EKS for new services starting in Q4
- Hiring or contracting someone with strong Kubernetes operations experience before migrating existing services
- A formal DR (disaster recovery) runbook as a prerequisite for migration of any production service

Reference on ECS vs. EKS considerations: https://aws.amazon.com/blogs/containers/amazon-ecs-vs-amazon-eks-making-sense-of-aws-container-services/
