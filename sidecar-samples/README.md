# Sidecar Examples
https://kubernetes.io/docs/concepts/workloads/pods/init-containers/

*Sidecar containers in K8s are initContainers with restartPolicy: Always*

**Example 1** pod-with-sidecar-container.yaml
Shows basic example how to integrate sidecontainers
```bash
k apply -f pod-with-sidecar-container.yaml
k logs -f job-with-sidecar --all-containers
```

Example 2
This example defines a simple Pod that has two init containers. The first waits for myservice, and the second waits for mydb. Once both init containers complete, the Pod runs the app container from its spec section.
- example2-main-pod.yaml
- example2-svc-api.yaml
- example2-svc-db.yaml

```bash
# Deploy the main pod and check the logs
k apply -f example2-main-pod.yaml
k logs -f example2-main-pod -c sidecar-check-app
k logs -f example2-main-pod -c sidecar-check-db

# Initial status
# NAME                READY   STATUS     RESTARTS   AGE
# example2-main-pod   0/1     Init:0/2   0          62s

# Deploy the app service in a separated terminal
k apply -f example2-svc-app.yaml

# NAME                READY   STATUS     RESTARTS   AGE
# example2-main-pod   0/1     Init:1/2   0          2m46s

# Deploy the db service in a separated terminal
k apply -f example2-svc-db.yaml

# NAME                                READY   STATUS         RESTARTS   AGE
# example2-main-pod                   1/1     Running        0          3m33s

# Clean resources
k delete -f example2-main-pod.yaml
k delete -f example2-svc-app.yaml
k delete -f example2-svc-db.yaml

# Check
k describe pod example2-main-pod
k logs example2-svc-db
k logs example2-main-pod
k logs example2-main-pod -c sidecar-logger
````