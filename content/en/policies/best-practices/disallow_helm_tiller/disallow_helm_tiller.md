---
title: "Disallow Helm Tiller"
category: Sample
policyType: "validate"
description: >
    Tiller has known security challenges. It requires administrative privileges and acts as a shared resource accessible to any authenticated user. Tiller can lead to privilege escalation as restricted users can impact other users.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/disallow_helm_tiller/disallow_helm_tiller.yaml" target="-blank">/best-practices/disallow_helm_tiller/disallow_helm_tiller.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-helm-tiller
  annotations:
    policies.kyverno.io/title: Disallow Helm Tiller
    policies.kyverno.io/category: Sample
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      Tiller has known security challenges. It requires administrative privileges and acts as a shared
      resource accessible to any authenticated user. Tiller can lead to privilege escalation as
      restricted users can impact other users.
spec:
  validationFailureAction: audit
  rules:
    - name: validate-helm-tiller
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: "Helm Tiller is not allowed"
        pattern:
          spec:
            containers:
              - name: "*"
                image: "!*tiller*"

```
