---
title: Kubernetes basics — study notes ahead of migration discussion
slug: kubernetes-basics
tags: [kubernetes, infrastructure, devops]
description: Core Kubernetes concepts studied in preparation for the migration conversation.
public: true
---

# Kubernetes basics — study notes ahead of migration discussion

Context: the infrastructure migration discussion [[20230909_1259]]. I don't have deep Kubernetes experience. Getting up to speed.

Primary resource: https://kubernetes.io/docs/concepts/

## Core objects

**Pod:** The smallest deployable unit. One or more containers that share network and storage. Usually you don't create Pods directly — you use higher-level abstractions.

**Deployment:** Manages a desired state for Pods. Handles rolling updates and rollbacks. `spec.replicas` controls how many Pod instances run.

**Service:** A stable network endpoint for a set of Pods. Pods come and go; a Service provides a consistent IP/DNS name. Types: ClusterIP (internal), NodePort (expose on each node's IP), LoadBalancer (cloud load balancer).

**ConfigMap / Secret:** Configuration and sensitive data injected into Pods as environment variables or volume mounts. Secrets are base64 encoded, not encrypted — use Sealed Secrets or Vault for real secrets management.

**Namespace:** Virtual cluster. Useful for isolating environments (staging, production) within a cluster. Resource quotas can be set per namespace.

**Ingress:** HTTP routing rules — how external traffic gets to Services. Requires an ingress controller (Nginx, Traefik, AWS ALB controller).

## Why this is harder than ECS

ECS's task definition maps closely to a Docker Compose file. The operational surface area is smaller. Kubernetes has more concepts, more moving parts, and the control plane requires active management (especially during version upgrades).

The power is real — scheduling, autoscaling, and rolling deployments are more capable — but so is the complexity budget.
