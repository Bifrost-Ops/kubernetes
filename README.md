# Bifrost Ops — Kubernetes

Reproducible Kubernetes reference stack using K3s, Cilium and Argo CD.

## Status
Planning scaffold only. No cluster installation or deployment is triggered.

## Intended sequence
1. Verify host OS, capacity, DNS, NTP and network reachability.
2. Document K3s version, topology and datastore choice.
3. Confirm Cilium compatibility and avoid conflicting CNIs.
4. Bootstrap K3s and validate node readiness.
5. Install Cilium using pinned versions; verify pod/service networking.
6. Bootstrap Argo CD and reconcile version-controlled applications.
7. Install observability components from the `observability` repository.

## Recovery principles
Keep encrypted backups of cluster datastore and required secrets. A Git revert does not restore persistent volumes or etcd/SQLite state. Document cluster reset separately from application rollback.

See [central documentation](https://github.com/Bifrost-Ops/docs).
