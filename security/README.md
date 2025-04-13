# Security Context
`securityContext` in Pods or containers defines privilege and access control settings like user/group IDs, file system permissions, or privilege escalation. It’s used to harden workloads.

## Main steps when working with securityContext:
	1.	Define at Pod level for all containers or at container level for granular control.
	2.	Use fields like `runAsUser`, `runAsGroup`, `fsGroup`, `readOnlyRootFilesystem`, and `allowPrivilegeEscalation`.
	3.	Combine with RBAC and PodSecurity Standards for stronger enforcement.

## Example
Scenario: You might want a Pod to run as a non-root user (runAsUser: 1000), mount files with specific group (fsGroup: 2000), and prevent privilege escalation (allowPrivilegeEscalation: false).

```bash
# Create the pod
k apply -f secure-pod.yaml

# Run commands to verify
kubectl exec -it secure-pod -- sh
id # Verify Current User
su # Attempt to Change to Root User:
mknod /tmp/test c 1 1 # Attempt to Mount a Filesystem
rm -rf /etc  # Attempt to delete /etc folder
echo "127.0.0.1 abcd" >> /etc/hosts

```

Test in an unsecure pod
```bash
# Create the pod
k apply -f unsecure-pod.yaml

# Run commands in the unsecure pod
kubectl exec -it unsecure-pod -- sh

```

Override example
```bash
# Create the pod
k apply -f secure-override-pod.yaml

# Run commands in the unsecure pod
kubectl exec -it secure-override-pod -- id

```