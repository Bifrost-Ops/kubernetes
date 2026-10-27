# GitOps bootstrap contract

## Principle
GitHub stores the desired configuration, while **each cluster is independently bootstrapped** before Argo CD can reconcile it.

## Proposed flow
1. Install K3s with a pinned version and reviewed cluster configuration.
2. Install Cilium according to its K3s compatibility guide. Confirm Flannel is disabled and decide separately whether kube-proxy replacement is enabled.
3. Confirm cluster DNS, pod/service routing, TLS and Kubernetes API reachability.
4. Bootstrap a pinned Argo CD release in the management cluster.
5. Configure Git read access, repository credentials and cluster registration via an external secret manager.
6. Register the service cluster with scoped Kubernetes RBAC and verified ACI contracts.
7. Apply reviewed Argo CD Applications with **manual sync first**, then enable automated sync per application after testing.
8. Deploy observability and confirm Grafana dashboards and telemetry from both clusters.

## Disaster recovery
Keep an independent, documented Argo CD installation procedure and K3s datastore backup outside the management cluster. Store desired Application definitions in Git, not solely inside the live cluster.

## No auto-deploy by default
No executable Argo CD Application or cluster credentials are committed until the deployment endpoints, namespaces, chart versions and secrets strategy are approved.
