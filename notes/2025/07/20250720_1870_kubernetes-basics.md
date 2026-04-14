---
title: Kubernetes concepts I always have to re-explain to myself
slug: kubernetes-basics
tags: [kubernetes, devops, reference]
description: Core Kubernetes concepts, clearly defined
---

# Kubernetes concepts I always have to re-explain to myself

Not a getting-started guide. Just the concepts I find myself redescribing each time I work in a new cluster.

**Pod vs Deployment vs ReplicaSet**

A *Pod* is the smallest deployable unit — one or more containers that share network and storage, scheduled together on a node. You rarely create Pods directly.

A *ReplicaSet* ensures a specified number of Pod replicas are running at any time. If a Pod dies, the ReplicaSet creates a replacement.

A *Deployment* manages ReplicaSets. When you update a Deployment (new image tag, config change), it creates a new ReplicaSet and gradually shifts traffic — this is rolling updates. If something goes wrong, you roll back by shifting back to the old ReplicaSet.

**Service types**

`ClusterIP` (default): accessible only within the cluster. For service-to-service communication.

`NodePort`: opens a port on every node; routes to the service. Used for testing, rarely production.

`LoadBalancer`: provisions a cloud load balancer. How you typically expose services to the internet.

`ExternalName`: DNS alias to an external service. No proxying.

**ConfigMaps vs Secrets**

ConfigMaps are for non-sensitive configuration: feature flags, connection strings without credentials, environment-specific config. Secrets are for sensitive data and are base64-encoded at rest (not encrypted by default — you need etcd encryption enabled for that).

**Resource requests and limits**

`requests` is what the scheduler uses to find a node. `limits` is what kubelet enforces at runtime. If a container exceeds its memory limit, it's OOMKilled. If it exceeds CPU limit, it's throttled.

kubectl cheatsheet: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
