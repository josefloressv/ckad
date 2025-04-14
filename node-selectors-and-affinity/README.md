# Node Selectors and Node Affinity

## Node Selector
Node selectors allow you to schedule a Pod on a specific set of nodes using key-value labels. It’s the simplest form of node selection.

### Main Steps When Using Node Selectors
- Label your target node: kubectl label node <node-name> <key>=<value>
- Use nodeSelector in your Pod spec to match that label.

### Quick example
If a node is labeled disktype=ssd, you can force a Pod to run only on that node.

```bash
# Label the node
kubectl label node docker-desktop disktype=ssd

# Run the container
k apply -f pod-node-selector.yaml

# Check
k get pod -o wide

```

## Node Affinity
Node Affinity is a way to control which nodes your Pod can be scheduled on, based on node labels. It’s like nodeSelector but more expressive and flexible (supports soft rules too).

### Main Types:
1. `requiredDuringSchedulingIgnoredDuringExecution` – Hard rule (Pod won’t be scheduled if not met).
2. `preferredDuringSchedulingIgnoredDuringExecution` – Soft rule (K8s will try to match, but won’t fail if not).

### Example
Schedule pod on nodes with label disktype=ssd

