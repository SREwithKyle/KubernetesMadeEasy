# Kubernetes Demo: Pod Pending due to Insufficient Resources

This demo simulates a Pod stuck in the `Pending` state because it requests more CPU or memory than is available in the cluster.

## Prerequisites

- A working Kubernetes cluster (e.g., Minikube, kind)
- `kubectl` configured and pointing to your cluster

> This demo assumes a cluster with limited CPU/memory (e.g., 2 CPUs, 2Gi memory).

## Steps

### 1. Check current node resources

```bash
kubectl describe nodes | grep -A5 "Allocatable"
```

### 2. Deploy a Pod with high resource requests

```bash
kubectl apply -f pod-high-resources.yaml
```

```yaml
# pod-high-resources.yaml
apiVersion: v1
kind: Pod
metadata:
  name: high-memory-pod
spec:
  containers:
  - name: memory-hog
    image: nginx
    resources:
      requests:
        memory: "5Gi"
        cpu: "2000m"
```

### 3. Describe the Pod to check the scheduling error

```bash
kubectl describe pod high-memory-pod
```

Look for a message like:

```
0/1 nodes are available: 1 Insufficient memory.
```

## Expected Outcome

- The Pod will stay in `Pending` because the requested memory and CPU exceed the node’s allocatable resources.
