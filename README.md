# ckad
CKAD PlayGround

Certified Kubernetes Application Developer Curriculum
https://github.com/cncf/curriculum/blob/master/CKAD_Curriculum_v1.32.pdf

20% - Application Design and Build
20% - Application Deployment
15% - Application observability and maintena
25% - Application Environment, Configuration and Security
20% - Services & Networking

General commandas
```bash
# Check Control Plane Components:
kubectl get componentstatuses

# Check Node roles
kubectl get nodes

# Taint control plane to schedule workload (for testing)
kubectl taint nodes <master-node-name> node-role.kubernetes.io/master-

# Describe pod
kubectl get pod my-pod -o yaml

# Force to delete a pod
kubectl delete pod mypod --grace-period=0 --force

# Quickly create a template pod yaml
kubectl create pod mypod --image=nginx --dry-run=client -o yaml > mypod.yaml

# Show events from a pod
k get events | grep -i new-replica-set-56gzk

# Get pods from all namespaces
kubectl get pods --all-namespaces
kubectl get pod -A
```

Explain commands
```bash
kubectl explain pod
kubectl explain pod.metadata
kubectl explain pod.spec.containers
```

Templating commands
```bash
k create ns dev --dry-run=client -o yaml
k run mypod --image=nginx --labels=app=myapptier=frontend --dry-run=client -o yaml
k create deploy mydeploy --image=nginx --replicas=3 --labels=app=myapp,tier=backend --dry-run=client -o yaml
k expose deploy mydeploy --name=mydeploy-service --port=80 --dry-run=client -o yaml
```

Imperative excercise with Deployments
```bash
k create ns dev
k config set-context --current --namespace=dev
k create deploy mydeploy --image=nginx --replicas=5
k expose deploy mydeploy --port=80 --name=mydeploy-service
k scale deploy mydeploy --replicas=2
k run test --image=busybox --rm -it -- sh
nslookup md-service
nc -vz md-service 80
k delete ns dev
```

ReplicaSet
```bash
k get rs
k get rs myrs -o yaml > rs.yaml
k scale rs myrs --replicas 5
k delete rs myrs
```

Deployments
```bash
# Templating a deployment
kubectl create deployment myapp \
  --image=nginx \
  --replicas=3 \
  --dry-run=client -o yaml > myapp-deployment.yaml

# Get deployments from all namespaces
kubectl get deploy --all-namespaces
kubectl get deploy -A
```

Namespaces
```bash
kubectl create ns dev
kubectl get pods -n dev
# Set by default a namespace in the current context to avoid mistakes during the exam because you’re working in the wrong namespace.
kubectl config set-context --current --namespace=dev
```

FQDN test
```bash
# pattern
# <service-name>.<namespace>.svc.cluster.local

# Use Busybox and nslookup for communication troubleshooting
kubectl run test --image=busybox --restart=Never -n MyNamespace --rm --it -- sh
nslookup db-service.dev.svc.cluster.local
nslookup db-service
nc -vz db-service.dev.svc.cluster.local 6379

```