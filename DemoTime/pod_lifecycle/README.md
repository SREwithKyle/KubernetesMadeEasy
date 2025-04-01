Note: The status is **Succeeded** in `describe pod` but **Completed** in `get pod`

```
k get po -w
NAME            READY   STATUS      RESTARTS   AGE
succeeded-pod   0/1     Completed   0          8s

k describe po succeeded-pod  | grep Status:
Status:           Succeeded
```