## HTTP Probe Demo

### Steps

1. `k apply -f http-probe.yaml`
2. To monitor, run `kubectl get po -w` or `kubectl describe po | grep Event -A 15`
3. Simulate liveness probe failure, run `kubectl exec http-probe-demo -- rm /usr/share/nginx/html/index.html`
4. The liveness probe would then fail and restart the container

```
k get po -w
NAME              READY   STATUS    RESTARTS        AGE
http-probe-demo   1/1     Running   1 (3m21s ago)   7m3s
http-probe-demo   1/1     Running   2 (1s ago)      7m23s
```

```
k describe po | grep Events -A 15
Events:
  Type     Reason       Age                  From               Message
  ----     ------       ----                 ----               -------
  Normal   Scheduled    8m8s                 default-scheduler  Successfully assigned default/http-probe-demo to node01
  Warning  FailedMount  8m7s                 kubelet            MountVolume.SetUp failed for volume "kube-api-access-brz58" : failed to sync configmap cache: timed out waiting for the condition
  Normal   Pulled       8m                   kubelet            Successfully pulled image "nginx" in 6.463s (6.463s including waiting). Image size: 72180980 bytes.
  Normal   Pulled       4m26s                kubelet            Successfully pulled image "nginx" in 490ms (490ms including waiting). Image size: 72180980 bytes.
  Normal   Pulling      46s (x3 over 8m6s)   kubelet            Pulling image "nginx"
  Normal   Created      46s (x3 over 7m59s)  kubelet            Created container: nginx
  Normal   Started      46s (x3 over 7m59s)  kubelet            Started container nginx
  Warning  Unhealthy    46s (x6 over 4m46s)  kubelet            Liveness probe failed: HTTP probe failed with statuscode: 403
  Normal   Killing      46s (x2 over 4m26s)  kubelet            Container nginx failed liveness probe, will be restarted
  Normal   Pulled       46s                  kubelet            Successfully pulled image "nginx" in 506ms (506ms including waiting). Image size: 72180980 bytes.
```