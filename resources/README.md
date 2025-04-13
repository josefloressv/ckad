# Kubernetes Resources
Kube scheduler uses requests to place the pod on a node. If usage exceeds limits, the container can be throttled (CPU) or killed (Memory).
- **requests** = minimum guaranteed resources (like ECS soft limit)
- **limits** = maximum allowed resources (like ECS hard limit)

## Main Steps

1. Define resources inside each container of a pod.
2. Set `resources.requests.cpu` and `.memory`
3. Set `resources.limits.cpu` and `.memory`

How to define
```yaml
resources:
  requests:
    cpu: "250m"      # Minimum 0.25 core, m=millicores
    memory: "256Mi"  # Minimum 256Mi memory Mi=Mebibytes
  limits:
    cpu: "500m"      # Max 0.5 core
    memory: "512Mi"  # Max 512Mi memory
```

## Limit Example + Stress Tool
We’ll use the polinux/stress image (or progrium/stress) to simulate CPU/Memory pressure.
At the moment is configured to throttled the CPU `args: ["--cpu", "2"] # Stress 2 CPU workers`

```bash
# Apply the changes
k apply -f stress-cpu-pod.yaml

# Check in metrics
# Check logs and details
k logs stress-cpu
k describe pod stress-cpu | grep -A5 "Last State:"

```

Metric server to see the metrics or use Docker Destop UI
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.7.2/components.yaml
kubectl get deployment metrics-server -n kube-system

Now let's stress Memory and generate a OOMKilled error.
Stress 1 VM worker with 300MB of memory

```bash
# Apply the changes
k apply -f stress-memory-pod.yaml

# Check in metrics
# Check logs and details
k logs stress-memory
k describe pod stress-memory | grep -A5 "Last State:"
```

