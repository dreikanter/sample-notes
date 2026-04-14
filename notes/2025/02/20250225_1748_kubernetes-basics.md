---
title: Kubernetes basics — concepts I keep re-explaining
slug: kubernetes-basics
tags: [kubernetes, devops, reference]
---

# Kubernetes basics — concepts I keep re-explaining

Written mostly to consolidate my own understanding rather than as a general tutorial.

## Core objects

**Pod:** The smallest deployable unit. One or more containers sharing network and storage. Pods are ephemeral — you generally don't manage them directly.

**Deployment:** Manages a ReplicaSet, which manages pods. Handles rolling updates, rollbacks, desired replica count. This is what you usually create.

**Service:** Provides a stable network endpoint to a set of pods (which come and go). Three main types:
- `ClusterIP` (internal only)
- `NodePort` (exposed on each node's IP)
- `LoadBalancer` (cloud provider creates an external LB)

**ConfigMap / Secret:** Externalize configuration from container images. Secrets are base64-encoded (not encrypted by default).

**Namespace:** Logical isolation within a cluster. Useful for separating environments or teams.

## Kubectl cheatsheet

```bash
kubectl get pods -n namespace
kubectl describe pod podname -n namespace
kubectl logs podname -n namespace -f
kubectl exec -it podname -n namespace -- /bin/sh
kubectl apply -f manifest.yaml
kubectl delete -f manifest.yaml
kubectl rollout status deployment/myapp
kubectl rollout undo deployment/myapp
kubectl scale deployment myapp --replicas=3
kubectl get events --sort-by='.lastTimestamp'
```

## Things that confused me early

- A `Service` does not contain pods — it selects them by label
- `kubectl apply` is idempotent; `kubectl create` is not
- Resource limits and requests are separate: requests for scheduling, limits for enforcement

Interactive learning: [https://kubernetes.io/docs/tutorials/kubernetes-basics/](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
