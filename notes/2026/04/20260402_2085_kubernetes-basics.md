---
title: Kubernetes basics reference
slug: kubernetes-basics
tags: [kubernetes, dev, cheatsheet]
description: Core Kubernetes concepts and kubectl commands I use regularly
---

# Kubernetes basics reference

## Core concepts

**Pod**: smallest deployable unit. One or more containers, shared network and storage.

**Deployment**: manages a set of replica Pods. Handles rolling updates, rollbacks.

**Service**: stable network endpoint for a set of Pods. Types: ClusterIP (internal), NodePort, LoadBalancer.

**Namespace**: logical cluster partitioning. Resources in a namespace are isolated from other namespaces by default.

**ConfigMap / Secret**: externalized configuration. ConfigMap for non-sensitive; Secret for sensitive (base64 encoded, not encrypted by default — use Sealed Secrets or Vault for real encryption).

## kubectl essentials

```bash
# Context and cluster
kubectl config get-contexts
kubectl config use-context my-cluster

# Get resources
kubectl get pods                       # all pods in current namespace
kubectl get pods -n kube-system        # in a specific namespace
kubectl get pods -o wide               # with extra info (node, IP)
kubectl get all                        # pods, services, deployments

# Describe (events, conditions, detailed state)
kubectl describe pod my-pod-abc123

# Logs
kubectl logs my-pod-abc123
kubectl logs -f my-pod-abc123          # follow
kubectl logs my-pod-abc123 -c sidecar  # specific container

# Exec into a container
kubectl exec -it my-pod-abc123 -- bash

# Port forward
kubectl port-forward pod/my-pod 8080:8080
kubectl port-forward svc/my-service 8080:80

# Apply / delete
kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml
kubectl delete pod my-pod-abc123
```

## Rolling restart

```bash
kubectl rollout restart deployment/my-app
kubectl rollout status deployment/my-app
kubectl rollout undo deployment/my-app   # roll back
```

Reference: https://kubernetes.io/docs/reference/kubectl/
Cheatsheet: https://kubernetes.io/docs/reference/kubectl/quick-reference/
