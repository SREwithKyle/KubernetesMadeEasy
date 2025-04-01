# Container Lifecycle

### Apply the pods

```
controlplane:~$  k get po
NAME             READY   STATUS             RESTARTS      AGE
pod-running      1/1     Running            0             67s
pod-terminated   0/1     CrashLoopBackOff   1 (17s ago)   22s
pod-waiting      0/1     ErrImagePull       0             2m

controlplane:~$ k describe po | grep State   
    State:          Running
    State:          Terminated
    Last State:     Terminated
    State:          Waiting
```


### Kubernetes Pod STATUS vs Container State

Understanding the difference between `STATUS` and `State` is key when diagnosing issues in Kubernetes Pods.

---

### 📊 STATUS vs State

| Field       | Where You See It              | What It Means                                                                |
|-------------|-------------------------------|------------------------------------------------------------------------------|
| **STATUS**  | `kubectl get pod <pod>`       | The overall **Pod phase**: high-level summary of what's going on.           |
| **State**   | `kubectl describe pod <pod>`  | The detailed **container-level status**, per container inside the Pod.      |

---

### 🔍 Example Breakdown from Your Output

```bash
kubectl get po
```

| Pod Name         | STATUS             | Explanation                                                                |
|------------------|--------------------|----------------------------------------------------------------------------|
| `pod-running`    | `Running`          | At least one container is running successfully.                           |
| `pod-terminated` | `CrashLoopBackOff` | Container started, crashed, and is restarting repeatedly.                 |
| `pod-waiting`    | `ErrImagePull`     | Container is stuck in **Waiting** due to failed image pull.               |

```bash
kubectl describe pod | grep State
```

| Output line             | What it tells you                                                      |
|-------------------------|------------------------------------------------------------------------|
| `State: Running`        | This container is actively running.                                   |
| `State: Terminated`     | This container ran and exited (success or error).                     |
| `Last State: Terminated`| The container crashed previously (used in restarts/crashloops).       |
| `State: Waiting`        | The container hasn't started yet (e.g. downloading image, error).     |

---

### 🧠 Summary

- `STATUS` = bird’s eye view of the **Pod**
- `State` = ground-level detail of each **Container** in the Pod
