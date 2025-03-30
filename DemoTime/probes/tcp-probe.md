### Steps - TCP Probe

1. Apply `tcp-probe.yaml`
2. `kubectl debug -it mysql-tcp-probe --image=busybox --target=mysql`
3. In a new tab, run `kubectl get po -w` to watch the pod status
4. in the debug container, run `kill 1`, which `1` is the process of the mysqld container
5. You'll see the container got restarted in the `kubectl get po -w` tab