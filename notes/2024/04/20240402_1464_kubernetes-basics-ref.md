# Kubernetes basics reference

Working through the fundamentals. These are the concepts and commands I needed to understand first.

## Core objects

**Pod:** Smallest deployable unit. Usually one container, sometimes sidecar pattern. Pods are ephemeral — don't address them directly in production.

**Deployment:** Manages ReplicaSets. Handles rolling updates, rollbacks, scaling. This is what you create for stateless apps.

**Service:** Stable network endpoint for pods. Types:
- `ClusterIP` — internal only
- `NodePort` — exposes on each node's IP
- `LoadBalancer` — provisions cloud load balancer
- `ExternalName` — DNS alias

**ConfigMap / Secret:** Decouple configuration from image. Secret is base64-encoded (not encrypted by default in etcd — use encryption at rest or external vault).

**Namespace:** Virtual cluster within a cluster. For environment isolation or team separation.

## Essential kubectl

```bash
# Cluster state
kubectl get nodes
kubectl get pods -n my-namespace
kubectl get all

# Describe (debugging)
kubectl describe pod <pod-name>
kubectl describe service <svc-name>

# Logs
kubectl logs <pod-name>
kubectl logs <pod-name> -f --tail=50    # follow
kubectl logs <pod-name> -c sidecar       # specific container

# Execute
kubectl exec -it <pod-name> -- /bin/sh

# Apply config
kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml

# Scale
kubectl scale deployment myapp --replicas=3

# Rollout
kubectl rollout status deployment/myapp
kubectl rollout undo deployment/myapp
```

## Port forwarding for local access

```bash
kubectl port-forward svc/myservice 8080:80
kubectl port-forward pod/mypod 8080:8080
```

## Resource requests and limits

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```

Requests: what the scheduler uses for placement. Limits: hard ceiling. Set limits always; OOMKilled pods are no fun to debug.

Docs: https://kubernetes.io/docs/home/
