1. Apply the pod with following definition:

```
apiVersion: v1
kind: Pod
metadata:
  name: exec-probe-demo
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["/bin/sh", "-c", "touch /tmp/healthy && sleep 3600"]
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 10
```


2. Remove the file with `kubectl exec exec-probe-demo -- rm /tmp/healthy`

3. You'll get this under the `Event` section of `kubectl describe po`

```
  Normal   Started    63s               kubelet            Started container busybox
  Warning  Unhealthy  6s (x3 over 26s)  kubelet            Liveness probe failed: cat: can't open '/tmp/healthy': No such file or directory
  Normal   Killing    6s                kubelet            Container busybox failed liveness probe, will be restarted
```

The restart count would increase

```
k get po -w
NAME              READY   STATUS    RESTARTS      AGE
exec-probe-demo   1/1     Running   0             88s
http-probe-demo   1/1     Running   2 (11m ago)   19m
exec-probe-demo   1/1     Running   1 (1s ago)    91s
```

4. After restart container, the file is created again from the container command