---
title: "Validate Probes"
category: Sample
policyType: "validate"
description: >
    Sample policy to check that liveness and readiness probes are not set to the same values.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/ensure_probes_different.yaml" target="-blank">/other/ensure_probes_different.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: validate-probes  
  annotations:
    # Only applies to pods originating from DaemonSet, Deployment, or StatefulSet.
    pod-policies.kyverno.io/autogen-controllers: DaemonSet,Deployment,StatefulSet  
    policies.kyverno.io/title: Validate Probes
    policies.kyverno.io/category: Sample
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      Sample policy to check that liveness and readiness probes are not set to the same values.
spec:
  validationFailureAction: audit
  background: false
  rules:
    # Checks the first container in a Pod.
    - name: validate-probes-c0
      match:
        resources:
          kinds:
          - Pod
      validate:
        message: "Liveness and readiness probes cannot be the same."
        # A `deny` rule is different in structure than a `validate` rule and inverts the check. It uses `conditions` written in JMESPath notation upon which to base its decisions.
        deny:
          conditions:
          # In this condition, it checks the entire map structure of the `readinessProbe` against that of the `livenessProbe`. If both are found to be equal, the Pod creation
          # request will be denied.
          - key: "{{ request.object.spec.containers[0].readinessProbe }}"
            operator: Equals
            value: "{{ request.object.spec.containers[0].livenessProbe }}"
    # Checks the second container in a Pod.
    - name: validate-probes-c1
      match:
        resources:
          kinds:
          - Pod
      validate:
        message: "Liveness and readiness probes cannot be the same."
        deny:
          conditions:
          - key: "{{ request.object.spec.containers[1].readinessProbe }}"
            operator: Equals
            value: "{{ request.object.spec.containers[1].livenessProbe }}"
    # Checks the third container in a Pod.
    - name: validate-probes-c2
      match:
        resources:
          kinds:
          - Pod
      validate:
        message: "Liveness and readiness probes cannot be the same."
        deny:
          conditions:
          - key: "{{ request.object.spec.containers[2].readinessProbe }}"
            operator: Equals
            value: "{{ request.object.spec.containers[2].livenessProbe }}"
    # Checks the fourth container in a Pod.
    - name: validate-probes-c3
      match:
        resources:
          kinds:
          - Pod
      validate:
        message: "Liveness and readiness probes cannot be the same."
        deny:
          conditions:
          - key: "{{ request.object.spec.containers[3].readinessProbe }}"
            operator: Equals
            value: "{{ request.object.spec.containers[3].livenessProbe }}"
```
