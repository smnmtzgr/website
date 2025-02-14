---
title: "Restrict Seccomp"
category: Pod Security Standards (Restricted)
policyType: "validate"
description: >
    The runtime default seccomp profile must be required, or only specific additional profiles should be allowed.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/restricted/restrict-seccomp/restrict-seccomp.yaml" target="-blank">/pod-security/restricted/restrict-seccomp/restrict-seccomp.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-seccomp
  annotations:
    policies.kyverno.io/title: Restrict Seccomp
    policies.kyverno.io/category: Pod Security Standards (Restricted)
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      The runtime default seccomp profile must be required, or only specific
      additional profiles should be allowed.
spec:
  background: true
  validationFailureAction: audit
  rules:
  - name: seccomp
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: >-
        Use of custom Seccomp profiles is disallowed. The fields
        spec.securityContext.seccompProfile.type,
        spec.containers[*].securityContext.seccompProfile.type, and
        spec.initContainers[*].securityContext.seccompProfile.type
        must be unset or set to `runtime/default`.
      pattern:
        spec:
          =(securityContext):
            =(seccompProfile):
              =(type): "runtime/default"
          =(initContainers):
          - =(securityContext):
              =(seccompProfile):
                =(type): "runtime/default"
          containers:
          - =(securityContext):
              =(seccompProfile):
                =(type): "runtime/default"

```
