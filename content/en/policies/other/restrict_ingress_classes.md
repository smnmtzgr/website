---
title: "Restrict Ingress Classes"
category: Sample
policyType: "validate"
description: >
    It can be useful to restrict Ingress resources to a set of known ingress classes  that are allowed in the cluster. You can customize this policy to allow ingress  classes that are configured in the cluster.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/restrict_ingress_classes.yaml" target="-blank">/other/restrict_ingress_classes.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-ingress-classes
  annotations:
    policies.kyverno.io/title: Restrict Ingress Classes
    policies.kyverno.io/category: Sample
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      It can be useful to restrict Ingress resources to a set of known ingress classes 
      that are allowed in the cluster. You can customize this policy to allow ingress 
      classes that are configured in the cluster.
spec:
  rules:
  - name: validate-ingress
    match:
      resources:
        kinds:
        - Ingress
    validate:
      message: "Unknown ingress class."
      pattern:
        metadata:
          annotations:
            kubernetes.io/ingress.class: "HAProxy | nginx"
```
