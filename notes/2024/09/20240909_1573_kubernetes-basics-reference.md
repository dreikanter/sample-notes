---
title: Kubernetes basics reference
slug: kubernetes-basics-reference
tags: [kubernetes, devops, reference]
description: Core Kubernetes concepts and kubectl commands
public: true
---

# Kubernetes basics reference

I interact with Kubernetes irregularly enough that I need to rebuild context each time. This note is that context.

## Core objects

**Pod:** Smallest deployable unit. One or more containers that share network and storage.
**Deployment:** Manages a set of replica Pods. Handles rolling updates and rollbacks.
**Service:** Stable network endpoint for a set of Pods. Load balances across replicas.
**ConfigMap / Secret:** Inject configuration and secrets into Pods.
**Namespace:** Logical cluster separation.

## kubectl commands I use

```bash
# Context
kubectl config get-contexts
kubectl config use-context <name>
kubectl config set-context --current --namespace=<ns>

# Get resources
kubectl get pods -n <namespace>
kubectl get pods -A            # all namespaces
kubectl describe pod <name>    # full detail
kubectl get events --sort-by=.metadata.creationTimestamp

# Logs
kubectl logs <pod>
kubectl logs <pod> -c <container>  # multi-container pod
kubectl logs <pod> --previous      # crashed container

# Exec into a pod
kubectl exec -it <pod> -- /bin/sh

# Apply / delete
kubectl apply -f deployment.yaml
kubectl delete pod <pod>          # pod will be recreated if managed by deployment
kubectl rollout restart deployment/<name>
kubectl rollout status deployment/<name>

# Port forward for local debugging
kubectl port-forward svc/<service> 8080:80
```

## Useful patterns

```bash
# Get pod name dynamically
POD=$(kubectl get pod -l app=myapp -o jsonpath='{.items[0].metadata.name}')
kubectl exec -it "$POD" -- bash
```

[Kubernetes documentation](https://kubernetes.io/docs/)
