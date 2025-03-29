# Demo: Persistent Volume across Pod Restart

### 🔍 Explanation:
- We define a PersistentVolume with a hostPath that stores files on the node’s file system.

- A PersistentVolumeClaim requests storage from the PersistentVolume.

- The Pod mounts the PVC and continuously writes timestamps to persistent-file.txt.

- If the container crashes and restarts, the file persistent-file.txt retains previous entries, demonstrating persistence.

### Commands to Run:

```sh
# Apply the PersistentVolume and PersistentVolumeClaim
kubectl apply -f persistent-volume.yaml
kubectl apply -f persistent-volume-claim.yaml

# Verify PVC status
kubectl get pvc

# Apply the Pod
kubectl apply -f pvc-pod.yaml

# View the Pod logs
kubectl logs -f pvc-pod

# Now, delete the pod to simulate a restart
kubectl delete pod pvc-pod

# Re-apply the Pod
kubectl apply -f pvc-pod.yaml

# View logs again to confirm persistence
kubectl logs -f pvc-pod
```