---
title: "Disallow Host Ipc"
category: Pod Security Standards (Baseline)
policyType: "validate"
description: >
    Sharing the host's PID namespace allows visibility of process on the host, potentially exposing process information. Sharing the host's IPC namespace allows the container process to communicate with processes on the host. To avoid pod container from having visibility to host process space, validate that 'hostPID' and 'hostIPC' are set to 'false'.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/baseline/disallow-host-ipc/disallow-host-ipc.yaml" target="-blank">/pod-security/baseline/disallow-host-ipc/disallow-host-ipc.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-host-ipc
  annotations:
    policies.kyverno.io/category: Pod Security Standards (Baseline)
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description:
      Sharing the host's PID namespace allows visibility of process
      on the host, potentially exposing process information. Sharing the host's IPC namespace allows
      the container process to communicate with processes on the host. To avoid pod container from
      having visibility to host process space, validate that 'hostPID' and 'hostIPC' are set to 'false'.
spec:
  validationFailureAction: audit
  rules:
    - name: validate-hostIPC
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: "Use of host PID ands IPC namespaces is not allowed"
        pattern:
          spec:
            =(hostPID): "false"
            =(hostIPC): "false"

```
