# administraton-tasks chart

## Create secure namespaces

To create a namespace that will have:
- A deny-all Ingress and Egress NetworkPolicy
- An allow-dns NetworkPolicy (so that pods in the namespace can access `kube-dns`):

```yaml
namespaces:
  - name: team1
    secure: true
```

This requires you to create NetworkPolicies for all deployments that you create (you will have to include it in the chart). No NetworkPolicies will be created if you specify `secure: false`.

## Assign cluster administrators

You can assign users to have `cluster-admin` role for all namespaces:

```yaml
clusterAdmins:
  create: true
  users:
    - user@example.com
```

## Assign roles to the Kubernetes Dashboard

```yaml
dashboardPermissions:
  create: true
  clusterRoles:
    - view
    - secret-lister
    - cluster-object-viewer
```
