---
title: "Disallow Host Path"
category: Pod Security Standards (Baseline)
policyType: "validate"
description: >
    HostPath volumes let pods use host directories and volumes in containers. Using host resources can be used to access shared data or escalate privileges and should not be allowed.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/baseline/disallow-host-path/disallow-host-path.yaml" target="-blank">/pod-security/baseline/disallow-host-path/disallow-host-path.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-host-path
  annotations:
    policies.kyverno.io/category: Pod Security Standards (Baseline)
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      HostPath volumes let pods use host directories and volumes in containers.
      Using host resources can be used to access shared data or escalate privileges
      and should not be allowed.
spec:
  validationFailureAction: audit
  background: true
  rules:
    - name: host-path
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: >-
          HostPath volumes are forbidden. The fields spec.volumes[*].hostPath must not be set.
        pattern:
          spec:
            =(volumes):
              - X(hostPath): "null"

```
