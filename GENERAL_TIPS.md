# General tips CKAD

use short command versions or aliases when possible

```bash
# kubectl
k
alias k=kubectl

# pods
k get pods

# Deployments
k get deploy

# ConfigMaps
k get cm

# Services
k get svc

# Service Accounts
k get sa
```

When creating multiple Resources, use descriptive names. This ensures easier reference within Pod definitions.

💡 NOTE: Be careful when editing pods. If you define a field like securityContext twice,  the last one (e.g., securityContext: {}) overrides previous settings.