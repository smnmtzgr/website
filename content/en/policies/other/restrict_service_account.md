---
title: "Restrict Service Account"
category: Sample
policyType: "validate"
description: >
    Restrict Pod resources to use a known service account can be useful to ensure workload identity. This policy uses a ConfigMap resource as an external data source to map service accounts to images. Example: 'sa-name: ["registry/image-name"]'
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/restrict_service_account.yaml" target="-blank">/other/restrict_service_account.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-service-account
  annotations:
    policies.kyverno.io/title: Restrict Service Account
    policies.kyverno.io/category: Sample
    policies.kyverno.io/severity: medium
    policies.kyverno.io/minversion: 1.3.5
    policies.kyverno.io/description: >-
      Restrict Pod resources to use a known service account can be useful to
      ensure workload identity. This policy uses a ConfigMap resource as an
      external data source to map service accounts to images.
      Example: 'sa-name: ["registry/image-name"]'
spec:
  background: false
  validationFailureAction: enforce
  rules:
  - name: validate-service-account
    context:
    - name: saMap
      configMap:
        name: sa-map
        namespace: staging
    match:
      resources:
        kinds:
        - Pod
        namespaces:
        - staging
    validate:
      message: "Invalid service account {{ request.object.spec.serviceAccountName }} for image {{ images.containers.*.registry | [0] }}/{{ images.containers.*.name | [0] }}"
      deny:
        conditions:
        - key: "{{ images.containers.*.registry | [0] }}/{{ images.containers.*.name | [0] }}"
          operator: NotIn
          value: "{{ saMap.data.\"{{ request.object.spec.serviceAccountName }}\" }}" 

```
