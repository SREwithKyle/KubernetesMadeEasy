# Kubernetes Demo: Pod Pending due to `hostPort` Conflict

This demo shows how a Pod can get stuck in the `Pending` state when multiple Pods try to bind to the same `hostPort` on a single-node cluster.

## Prerequisites

- A working Kubernetes cluster (e.g., Minikube, kind)
- `kubectl` configured and pointing to your cluster

> This demo assumes a **single-node** cluster.

## Steps

### 1. Create the first Pod using `hostPort`

```bash
kubectl apply -f pod-with-hostport.yaml
```

```yaml
# pod-with-hostport.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostport-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
      hostPort: 8080
```

### 2. Create a second Pod with the same `hostPort`

```bash
kubectl apply -f pod-with-hostport-2.yaml
```

```yaml
# pod-with-hostport-2.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostport-pod-2
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
      hostPort: 8080
```

### 3. Inspect the second Pod

```bash
kubectl describe pod hostport-pod-2
```

Look for a message like:

```
0/1 nodes are available: 1 node(s) had a conflict.
```

## Expected Outcome

- `hostport-pod` will run successfully.
- `hostport-pod-2` will remain in the `Pending` state due to the `hostPort` conflict.
