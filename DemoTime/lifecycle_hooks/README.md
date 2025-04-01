# 🚀 Kubernetes Lifecycle Hook Demo (PostStart + PreStop)

This project demonstrates how to use **Kubernetes container lifecycle hooks** (`PostStart` and `PreStop`) to log container events in an `nginx` pod.

---

## 📚 What You'll Learn

- How to trigger a command immediately after a container starts (`PostStart`)
- How to execute logic right before a container stops (`PreStop`)
- How to persist lifecycle hook logs using a `PersistentVolumeClaim` (PVC)
- How to expose logs via `nginx` and access them using `curl` or a browser

---

## 📦 Files Included

- `hooks.yaml`: Deployment spec with lifecycle hooks and a PVC
- `README.md`: This guide

---

## 🔧 How It Works

### 📌 Lifecycle Hooks
Kubernetes lets you define actions during the container lifecycle:

| Hook       | When it Executes                        | Purpose                              |
|------------|------------------------------------------|--------------------------------------|
| `PostStart`| Right after the container starts         | Run setup scripts or log events      |
| `PreStop`  | Just before the container is terminated  | Clean up resources or log shutdown   |

### 🗂️ Logging to Persistent Volume
We use a `PersistentVolumeClaim` (PVC) mounted to the nginx content directory so logs are **retained across pod restarts**.

The hooks write to `/usr/share/nginx/html/index.html`, and nginx serves that file at `/`.

---

## 📋 Steps to Deploy

### 1. Deploy the PVC and Deployment
```sh
kubectl apply -f hooks.yaml
```

### 2. Get the Pod Name
```sh
kubectl get pods -l app=nginx-hook-demo
```

### 3. Port-forward to Access Logs
```sh
kubectl port-forward <pod-name> 8080:80
```

### 4. View Lifecycle Logs
```sh
curl http://localhost:8080/
```

You should see:
```
<pre>PostStart hook ran at Tue Apr 1 14:00:00 UTC 2025
PreStop hook ran at Tue Apr 1 14:05:00 UTC 2025</pre>
```

### 5. Trigger the PreStop Hook
```sh
kubectl delete pod <pod-name>
```

Then get the **new pod name** and repeat the port-forward to verify logs persisted.

---

## 🧠 Notes

- `PostStart` runs **after the container starts**, but not necessarily before the ENTRYPOINT.
- `PreStop` must complete **within the pod's termination grace period**.
- Using a PVC ensures logs survive **pod restarts**, not just container restarts.

---

## 🚫 Limitations

- `emptyDir` volumes (used in simpler demos) **do not persist** when the pod is deleted.
- This setup does not include log rotation or external logging tools (e.g., Fluent Bit, Loki).

---

Happy Hooking! 🚀

