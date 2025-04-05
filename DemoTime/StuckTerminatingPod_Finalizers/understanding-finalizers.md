
# 📘 Understanding Kubernetes Finalizers

Finalizers are a powerful mechanism in Kubernetes that allow resources to implement **pre-deletion hooks** — giving controllers or systems a chance to perform cleanup **before the actual deletion** of a resource.

---

## ❓ What is a Finalizer?

A finalizer is just a **string** added to the `.metadata.finalizers` field of a Kubernetes object. Kubernetes will **not delete the object** until all finalizers are removed from the list.

---

## 🧠 Why Use Finalizers?

They are used to:
- Perform cleanup of external resources (e.g., cloud disks, DNS records)
- Wait for dependent objects to be deleted (like in `foregroundDeletion`)
- Prevent deletion until some logic completes

---

## 🧪 What Does a Finalizer Look Like?

```yaml
metadata:
  finalizers:
    - example.com/my-cleanup-hook
```

This is **just a string**. You can use any string that uniquely identifies your cleanup logic or controller.

Kubernetes does not validate the string, nor does it do anything automatically with it. It's up to a **controller** to detect and remove the finalizer once its job is done.

---

## ⚠️ Important Notes

- If no controller removes the finalizer, the resource gets **stuck** in `Terminating`.
- You can manually remove finalizers using:
  ```bash
  kubectl patch <resource> <name> -p '{"metadata":{"finalizers":[]}}' --type=merge
  ```

---

## 🔄 Lifecycle With Finalizer

1. Object created with a finalizer.
2. You issue `kubectl delete <resource>`.
3. Kubernetes sets `.metadata.deletionTimestamp` but does **not** remove the object.
4. Controller notices the finalizer, does cleanup, and removes the string.
5. Kubernetes deletes the object once the finalizers list is empty.

---

## ✅ Summary

- Finalizers are **just strings** in metadata.
- Kubernetes **waits** for finalizers to be removed before deleting an object.
- Use them wisely — and clean them up to avoid stuck resources!
