# Jobs
A Job runs a one-time task to completion (e.g., batch jobs or scripts). It ensures the pod runs successfully at least once. For parallel or repeated jobs, you can configure completions and parallelism.

## Main Steps When Working with a Job:
1. Define the task in a Job YAML (command, image, etc.).
2. Optionally set completions, parallelism, backoffLimit.
3. Apply the YAML with kubectl apply.
4. Monitor with kubectl get jobs and kubectl get pods.

## Example


```bash
kubectl apply -f line-counter-job.yaml
kubectl get jobs
kubectl get pods
kubectl logs <pod-name>

k delete jobs --all 
```

Retry Job example

```bash
k apply -f retry-job.yaml

# Check
k get pod -w
```

**Parallel Jobs**
Kubernetes Jobs can run multiple pods in parallel using the completions and parallelism fields. Use them when you want many identical tasks (like processing files, workloads, batches) to run at the same time.

```bash
k apply -f parallel-job.yaml

# Check
k get pod -w
k get job -w
```