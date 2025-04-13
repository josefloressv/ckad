# Secrets
Similar to ConfigMaps, handling Secrets typically involves two steps:
1. Creating the Secret.
2. Injecting the Secret into a Pod.

For this examples we will use this application https://github.com/josefloressv/flask_mysql_db_app
The application image: `josefloressv/flask_mysql_app`
require these variables

```bash
export DB_HOST=mysql-service
export DB_USER=root
export DB_PASSWORD=T0pSecret
```

Deploy
```bash
# Create the secret for the app
k apply -f 01-secret.yam

# Create the Mysql pod with the service service-mysql
k apply -f 02-mysql-db.yam

# Create the Pod that maps the secret as a reference and create the service service-pod-map-secret-ref by the port 8081
k apply -f 03-pod-map-secret-ref.yaml

# Create the Pod that maps the secret as key by key and create the service service-pod-map-secret-keys by the port 8082
k apply -f 04-map-secret-keys.yaml

```

Validate

```bash
# Secret
k get secret app-creds -o yaml

# Pods and services
k get pod,svc

# Check with a temporary containter
k run testservice --image=busybox --rm -it -- sh
nslookup service-pod-map-secret-ref
nslookup service-pod-map-secret-keys
k run test --image=curlimages/curl --rm -it -- sh
curl http://service-pod-map-secret-ref:8081
curl http://service-pod-map-secret-keys:8082

```

Create Secrets imperative

```bash
# This automatically ofuscate the values
k create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=xxxx --from-literal=DB_Password=xxxx
```