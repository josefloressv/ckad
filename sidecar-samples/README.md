# Sidecar Examples
https://kubernetes.io/docs/concepts/workloads/pods/init-containers/

*Sidecar containers in K8s are initContainers with restartPolicy: Always*

**Example 1** pod-with-sidecar-container.yaml
Shows basic example how to integrate sidecontainers
```bash
k apply -f pod-with-sidecar-container.yaml
k logs -f job-with-sidecar --all-containers
```