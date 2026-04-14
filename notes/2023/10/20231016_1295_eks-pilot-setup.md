---
title: EKS pilot setup — notes from first day
slug: eks-pilot-setup
tags: [kubernetes, aws, infrastructure, work]
description: Notes from the first day of the EKS pilot migration.
---

# EKS pilot setup — notes from first day

The EKS pilot started today. We're migrating the notification service — stateless, low traffic, good isolation. Perfect candidate.

Context: [[20230909_1259]], [[20231003_1283]], [[20230917_1267]]

## Cluster setup

Marcus did the cluster configuration via the Terraform module he wrote last week. Key decisions:
- Managed node groups (not self-managed) — AWS handles node patching
- t3.medium nodes for the pilot (can scale up)
- VPC CNI for networking — native AWS VPC IPs, no overlay network complexity

The cluster came up cleanly. IAM roles for service accounts (IRSA) configured for the notification service to access SES and SNS without instance-level credentials.

## Deploying the notification service

Containerized — already had a Dockerfile. The k8s manifests:

**Deployment:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: notification-service
spec:
  replicas: 2
  selector:
    matchLabels:
      app: notification-service
  template:
    spec:
      containers:
        - name: notification
          image: [ECR_URI]:latest
          env:
            - name: NODE_ENV
              value: production
          resources:
            requests:
              memory: "128Mi"
              cpu: "100m"
            limits:
              memory: "256Mi"
              cpu: "500m"
```

**Learnings:**

- Always set resource requests and limits. Without them, a memory leak can consume a whole node.
- The ECR authentication to EKS worked via IRSA — needed an OIDC provider configured on the cluster.
- `kubectl logs -f deployment/notification-service` follows logs across all replicas — convenient.

Service responded correctly to health checks after about 3 minutes. Initial validation looks good.

AWS EKS docs: https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html
