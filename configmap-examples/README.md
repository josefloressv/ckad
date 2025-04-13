# Config Map examples

*A ConfigMap is used to decouple configuration data (non-sensitive) from container images, letting you reuse and modify configs without rebuilding containers.*

**example1-cm-variables.yaml**
Thios example create a basic config map with variables and use it in a Pod as ENV VARs

```bash
# Apply the changes
k apply -f example1-cm-variables.yaml

# Check the logs
k logs app-pod

# delete pod
k delete -f example1-cm-variables.yaml
```
**example2-cm-file.yaml**

```bash
# Apply the changes
k apply -f example1-cm-file.yaml

# Check the logs
k logs nginx-pod

# Quick check the page from the same pod
k exec -it nginx-pod -- curl 127.0.0.1

# Check from temporary pod
k get pod -o wide
kubectl run test --rm -it --image=curlimages/curl --restart=Never -- curl http://10.1.1.38
k expose pod nginx-pod --name=nginx-service --port=80
kubectl run test --rm -it --image=curlimages/curl --restart=Never -- curl http://nginx-service

# delete pod
k delete -f example1-cm-file.yaml
```


Other test
- If a key is missing from the ConfigMap, the Pod will not start unless you use optional: false. (CreateContainerConfigError)

Imperative commands
```bash
# Create a quick configmap with variables
k create cm my-vas --from-literal=APP_ENV=dev --from-literal=APP_LOG_LEVEL=debug
# Create a quick configmap from file
k create cm nginx-config --from-file=index.html=index.html

# Quick validation
k get cm -o yaml
```