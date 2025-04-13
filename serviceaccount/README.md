# ServiceAccount
- A ServiceAccount provides an identity for a pod to interact with the Kubernetes API.
- You attach it to a Pod/Deployment.
- Combine with RBAC (Role, RoleBinding) to control what that identity can do.


## Main Steps
1. Create a custom ServiceAccount.
2. Define a Role with limited permissions (e.g., list pods).
3. Bind the Role to the ServiceAccount using a RoleBinding.
4. Use the ServiceAccount in a Deployment.
5. Generate a token manually.
6. (Optional) Extract the token manually to use with kubectl.

## Example Breakdown
This example:
- Creates custom-sa
- Binds it to a Role that can get/list pods in the namespace
- Deploys busybox with that SA

```bash
k apply -f sa-deploy.yaml
```

Test

```bash
# 1. Read the token
TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)

# 2. Read the CA cert (optional if using `-k` for insecure)
CACERT=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt

# 3. Discover API Server (usually passed via env var)
APISERVER=https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT

# 4. Make the request to list Pods in the current namespace
curl -s --cacert $CACERT \
     -H "Authorization: Bearer $TOKEN" \
     $APISERVER/api/v1/namespaces/default/pods

# 5. Make the request to list Pods at the cluster level. Fails
curl -s --cacert $CACERT \
     -H "Authorization: Bearer $TOKEN" \
     $APISERVER/api/v1/pods

```
