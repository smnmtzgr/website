---
title: "Restrict Sysctls"
category: Pod Security Standards (Baseline)
policyType: "validate"
description: >
    Sysctls can disable security mechanisms or affect all containers on a host, and should be disallowed except for an allowed "safe" subset. A sysctl is considered safe if it is namespaced in the container or the Pod, and it is isolated from other Pods or processes on the same Node.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/baseline/restrict-sysctls/restrict-sysctls.yaml" target="-blank">/pod-security/baseline/restrict-sysctls/restrict-sysctls.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-sysctls
  annotations:
    policies.kyverno.io/category: Pod Security Standards (Baseline)
    policies.kyverno.io/severity: medium
    policies.kyverno.io/description: >-
      Sysctls can disable security mechanisms or affect all containers on a
      host, and should be disallowed except for an allowed "safe" subset. A
      sysctl is considered safe if it is namespaced in the container or the
      Pod, and it is isolated from other Pods or processes on the same Node.
spec:
  validationFailureAction: audit
  background: true
  rules:
    - name: sysctls
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: >-
          Setting additional sysctls above the allowed type is disallowed.
          The field spec.securityContext.sysctls must not use any other names
          than 'kernel.shm_rmid_forced', 'net.ipv4.ip_local_port_range',
          'net.ipv4.tcp_syncookies' and 'net.ipv4.ping_group_range'.
        pattern:
          spec:
            =(securityContext):
              =(sysctls):
                - name: "kernel.shm_rmid_forced | net.ipv4.ip_local_port_range | net.ipv4.tcp_syncookies | net.ipv4.ping_group_range"
                  value: "?*"

```
