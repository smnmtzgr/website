---
title: "Require Pod Probes"
category: Best Practices
policyType: "validate"
description: >
    Liveness and readiness probes need to be configured to correctly manage a pods  lifecycle during deployments, restarts, and upgrades. For each pod, a periodic  `livenessProbe` is performed by the kubelet to determine if the pod's containers  are running or need to be restarted. A `readinessProbe` is used by services  and deployments to determine if the pod is ready to receive network traffic.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/require_probes/require_probes.yaml" target="-blank">/best-practices/require_probes/require_probes.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-pod-probes
  annotations:
    pod-policies.kyverno.io/autogen-controllers: DaemonSet,Deployment,StatefulSet
    policies.kyverno.io/title: Require Pod Probes
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      Liveness and readiness probes need to be configured to correctly manage a pods 
      lifecycle during deployments, restarts, and upgrades. For each pod, a periodic 
      `livenessProbe` is performed by the kubelet to determine if the pod's containers 
      are running or need to be restarted. A `readinessProbe` is used by services 
      and deployments to determine if the pod is ready to receive network traffic.
spec:
  validationFailureAction: audit
  rules:
  - name: validate-livenessProbe-readinessProbe
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Liveness and readiness probes are required."
      pattern:
        spec:
          containers:
          - livenessProbe:
              periodSeconds: ">0"      
            readinessProbe:
              periodSeconds: ">0"
```
