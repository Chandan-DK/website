---
title: "Restrict Binding System Groups"
category: Security
version: 1.6.0
subject: RoleBinding, ClusterRoleBinding, RBAC
policyType: "validate"
description: >
    Certain system groups exist in Kubernetes which grant permissions that are used for certain system-level functions yet typically never appropriate for other users. This policy prevents creating bindings to some of these groups including system:anonymous, system:unauthenticated, and system:masters.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/restrict_binding_system_groups/restrict-binding-system-groups.yaml" target="-blank">/other/restrict_binding_system_groups/restrict-binding-system-groups.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-binding-system-groups
  annotations:
    policies.kyverno.io/title: Restrict Binding System Groups
    policies.kyverno.io/category: Security
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: RoleBinding, ClusterRoleBinding, RBAC
    kyverno.io/kyverno-version: 1.8.0
    policies.kyverno.io/minversion: 1.6.0
    kyverno.io/kubernetes-version: "1.23"
    policies.kyverno.io/description: >-
      Certain system groups exist in Kubernetes which grant permissions that
      are used for certain system-level functions yet typically never appropriate
      for other users. This policy prevents creating bindings to some of these
      groups including system:anonymous, system:unauthenticated, and system:masters.
spec:
  validationFailureAction: audit
  background: true
  rules:
    - name: restrict-anonymous
      match:
        any:
        - resources:
            kinds:
              - RoleBinding
              - ClusterRoleBinding
      validate:
        message: "Binding to system:anonymous is not allowed."
        pattern:
          roleRef:
            name: "!system:anonymous"
    - name: restrict-unauthenticated
      match:
        any:
        - resources:
            kinds:
              - RoleBinding
              - ClusterRoleBinding
      validate:
        message: "Binding to system:unauthenticated is not allowed."
        pattern:
          roleRef:
            name: "!system:unauthenticated"
    - name: restrict-masters
      match:
        any:
        - resources:
            kinds:
              - RoleBinding
              - ClusterRoleBinding
      validate:
        message: "Binding to system:masters is not allowed."
        pattern:
          roleRef:
            name: "!system:masters"

```