# Taints and Tolerations
Taints and tolerations work together to control which Pods can be scheduled on which Nodes.
- Taint: added to a node, repels pods that don’t tolerate it.
- Toleration: added to a pod, allows it to be scheduled on tainted nodes.

# Quick example
If a node has a taint like key=value:NoSchedule, no pod will be scheduled on it unless the pod has a matching toleration. Useful to reserve nodes for specific workloads.

Taint a node to repel general pods:
```bash
k get nodes

# Taint
kubectl taint nodes docker-desktop app=reserved:NoSchedule

# Run a pod to test, will not start
k run nginx --image=nginx
k get pod -w

# default-scheduler  0/1 nodes are available: 1 node(s) had untolerated taint {key: value}. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling.
k describe pod nginx
```

Pod with toleration
``` bash
k apply -f pod-with-toleration.yaml 
```

Remove taint
```bash
kubectl taint nodes docker-desktop key:NoSchedule-
```