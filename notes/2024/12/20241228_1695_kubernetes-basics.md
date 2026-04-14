---
title: Kubernetes basics — working notes
slug: kubernetes-basics
tags: [kubernetes, devops, infrastructure, containers]
---

# Kubernetes basics — working notes

Starting to use Kubernetes on a project. These are notes from learning, not authoritative reference. See the [official Kubernetes docs](https://kubernetes.io/docs/home/).

## Core concepts

**Pod**: The smallest deployable unit. Contains one or more containers sharing network and storage. Pods are ephemeral — they're killed and recreated, not updated in place.

**Deployment**: Manages a set of identical Pods. Handles rolling updates, rollbacks, scaling. You rarely create Pods directly — you create Deployments.

**Service**: A stable network endpoint in front of a set of Pods. Pods have changing IP addresses; Services have stable addresses.

**Namespace**: Virtual cluster within a cluster. Use for environment separation (dev/staging) or team separation.

**ConfigMap**: Store non-secret configuration data to inject into Pods.

**Secret**: Like ConfigMap but for sensitive data (base64-encoded, with more access controls).

## Basic workflow

```bash
kubectl get pods                          # List pods
kubectl get pods -n my-namespace          # In a specific namespace
kubectl describe pod my-pod               # Detailed pod info
kubectl logs my-pod                       # View logs
kubectl logs my-pod -f                    # Follow logs
kubectl exec -it my-pod -- bash           # Shell into pod
kubectl apply -f deployment.yaml          # Apply a manifest
kubectl delete -f deployment.yaml         # Delete resources
kubectl rollout status deployment/web     # Check rollout
kubectl rollout undo deployment/web       # Rollback
```

## Minimal deployment manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: myapp:1.2.3
        ports:
        - containerPort: 3000
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "500m"
```

## What's still unclear to me

- Networking model between services — specifically how Ingress controllers work in practice
- RBAC configuration — have read the docs but not set up from scratch yet
- Persistent volume claims for stateful applications
