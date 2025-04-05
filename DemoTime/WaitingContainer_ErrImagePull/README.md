# Bad Image Demo

This Kubernetes demo shows how a pod enters a `Waiting` state with a reason of `ErrImagePull` when it tries to use a non-existent image.

## Objective

To demonstrate what happens when Kubernetes fails to pull a container image due to:

- Typo in the image name
- Image not pushed to a container registry
- Using a private image without proper credentials

## Instructions

### 1. Save the Pod Manifest

Create a file named `bad-image-demo.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: bad-image-demo
spec:
  containers:
  - name: demo
    image: someimagethatdoesnotexist:latest
```

### 2. Apply the Manifest

```bash
kubectl apply -f bad-image-demo.yaml
```

### 3. Check Pod Status

```bash
kubectl get pod bad-image-demo
```

You should see a status like:

```
NAME              READY   STATUS             RESTARTS   AGE
bad-image-demo    0/1     ErrImagePull       0          10s
```

### 4. Describe the Pod

```bash
kubectl describe pod bad-image-demo
```

Look under the **Events** section. You should see an error like:

```
Failed to pull image "someimagethatdoesnotexist:latest": rpc error: ...
```

## Expected Output

The container stays in the `Waiting` state, and the reason shown will be either `ErrImagePull` or `ImagePullBackOff`.

## Clean Up

To delete the pod:

```bash
kubectl delete pod bad-image-demo
```

## Notes

- This is useful for understanding container lifecycle and troubleshooting image issues.
- If you're using a private registry, make sure imagePullSecrets is configured.
