---
title: "Restrict node label changes"
category: Sample
policyType: "validate"
description: >
    This policy prevents changes or deletions to label called `foo` on cluster nodes. Use of this policy requires removal of the Node resource filter in the Kyverno ConfigMap ([Node,*,*]). Due to Kubernetes CVE-2021-25735, this policy requires, at minimum, one of the following versions of Kubernetes: v1.18.18, v1.19.10, v1.20.6, or v1.21.0.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/restrict_node_label_changes.yaml" target="-blank">/other/restrict_node_label_changes.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: protect-node-label-foo
  annotations:
    policies.kyverno.io/title: Restrict node label changes
    policies.kyverno.io/category: Sample
    policies.kyverno.io/description: >-
      This policy prevents changes or deletions to label called `foo` on
      cluster nodes. Use of this policy requires removal of the Node resource filter
      in the Kyverno ConfigMap ([Node,*,*]). Due to Kubernetes CVE-2021-25735, this policy
      requires, at minimum, one of the following versions of Kubernetes:
      v1.18.18, v1.19.10, v1.20.6, or v1.21.0.
spec:
  validationFailureAction: enforce
  background: false
  rules:
  - name: prevent-label-value-changes
    match:
      resources:
        kinds:
        - Node
    validate:
      message: "Modifying the `foo` label on a Node is not allowed."
      deny:
        conditions:
        - key: "{{ request.object.metadata.labels.foo }}"
          operator: "NotEquals"
          value: ""
        - key: "{{ request.object.metadata.labels.foo }}"
          operator: "NotEquals"
          value: "{{ request.oldObject.metadata.labels.foo }}"
  - name: prevent-label-key-removal
    match:
      resources:
        kinds:
        - Node
    preconditions:
    - key: "{{ request.operation }}"
      operator: "Equals"
      value: UPDATE
    - key: "{{ request.oldObject.metadata.labels.foo }}"
      operator: "Equals"
      value: "*"
    validate:
      message: "Removing the `foo` label on a Node is not allowed."
      pattern:
        metadata:
          labels:
            foo: "*"
```
