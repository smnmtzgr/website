---
title: "Drop All Capabilities"
category: Best Practices
policyType: "validate"
description: >
    Capabilities permit privileged actions without giving full root access. All  capabilities should be dropped from a pod, with only those required added back.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/require_drop_all/require_drop_all.yaml" target="-blank">/best-practices/require_drop_all/require_drop_all.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: drop-all-capabilities
  annotations:
    policies.kyverno.io/title: Drop All Capabilities
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      Capabilities permit privileged actions without giving full root access. All 
      capabilities should be dropped from a pod, with only those required added back.
spec:
  validationFailureAction: audit
  rules:
  - name: check-containers
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "All capabilities should be dropped with only those required added back."
      pattern:
        spec:
          containers:
          - securityContext:
              capabilities:
                drop: ["ALL"]
          =(initContainers):
          - =(securityContext):
              (capabilities):
                drop: ["ALL"]
```
