---
title: "Set imagePullPolicy"
category: Sample
policyType: "validate"
description: >
    Sample policy that sets imagePullPolicy to "Always" when the "latest" tag is used.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/imagepullpolicy-always.yaml" target="-blank">/other/imagepullpolicy-always.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: imagepullpolicy-always
  annotations:
    policies.kyverno.io/title: Set imagePullPolicy
    policies.kyverno.io/category: Sample
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      Sample policy that sets imagePullPolicy to "Always" when the "latest" tag is used.
spec:
  validationFailureAction: audit
  background: false
  rules:
  - name: imagepullpolicy-always
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: >-
        The imagePullPolicy must be set to `Always` when the tag `latest` is used.
      pattern:
        spec:
          containers:
          - (image): "*:latest | !*:*"
            imagePullPolicy: "Always"
```
