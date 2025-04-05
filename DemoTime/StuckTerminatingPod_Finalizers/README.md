
# 🧪 Kubernetes Finalizer Demo

This demo shows how a pod can get "stuck" in the cluster even after completing, simply by using a finalizer.

---

## 📌 Objective

To demonstrate how Kubernetes respects finalizers during resource deletion, causing the pod to remain in a `Completed` state with the pod object still present in the cluster.

---

## 📄 1. Apply the Pod with a Finalizer

Create a file named `pod-with-finalizer.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: finalizer-pod
  namespace: default
  finalizers:
    - demo.kyle/finalizer
spec:
  containers:
    - name: nginx
      image: nginx
```

Apply the pod:

```bash
kubectl apply -f pod-with-finalizer.yaml
```

---

## 🗑️ 2. Delete the Pod

```bash
kubectl delete pod finalizer-pod
```

Even though the pod shows `Completed`, it will not be fully deleted because the finalizer is still present.

---

## 🔍 3. Confirm the Pod is Stuck

Check that the finalizer is still attached:

```bash
kubectl get pod finalizer-pod -o json | jq '.metadata.finalizers'
```

Also check if a `deletionTimestamp` has been set:

```bash
kubectl get pod finalizer-pod -o json | jq '.metadata.deletionTimestamp'
```

---

## 🛠️ 4. Unblock the Pod Deletion (Remove Finalizer)

To remove the finalizer manually:

```bash
kubectl patch pod finalizer-pod -p '{"metadata":{"finalizers":[]}}' --type=merge
```

The pod will now be deleted immediately.

---

## ✅ Summary

- Finalizers block deletion until they're removed.
- Kubernetes won't delete the resource until the finalizer list is empty.
- This is a useful mechanism for safe cleanup — but can cause stuck resources if not handled properly.
