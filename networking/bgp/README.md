# Cilium BGP-first templates

**NOT DEPLOYABLE AS-IS.** Example ASNs, addresses and pools are documentation-only. Verify exact Cilium CRDs with the pinned Helm/chart release before applying.

Files:
- `peer-config.yaml`: IPv4 address family and explicit advertisement selector.
- `cluster-config.example.yaml`: separate example peer topology for a selected cluster.
- `advertisement.yaml`: opt-in LoadBalancer service VIPs only.
- `lb-ip-pool.example.yaml`: illustrative, disabled-by-default address pool.
- `service.example.yaml`: example Service with explicit BGP opt-in label.

## Before applying
1. Confirm ACI L3Out or external-router peering design; **normal EPGs do not themselves create a BGP neighbor**.
2. Confirm node reachability, MTU, peer routing, VRF, CIDRs, TCP/179 path and approved prefix filters.
3. Install compatible Cilium release with BGP control plane enabled and validate CRDs.
4. Allocate real, non-overlapping Service VIP pool with route ownership and filtering.
5. Replace RFC5737 placeholders and ASN; set distinct cluster IDs/names.
6. Check configuration: `kubectl get ciliumbgpclusterconfigs,ciliumbgppeerconfigs,ciliumbgpadvertisements`.
7. Check peers with supported Cilium CLI/agent status for the installed release, and inspect upstream BGP RIB/FIB.
8. Verify traffic to a labeled LoadBalancer service, failover and route withdrawal.
9. Never turn on PodCIDR advertisement without a separate native-routing decision and test plan.

## Rollback
Remove the opt-in label from a test Service and verify upstream route withdrawal **before** disabling BGP control plane. Do not apply destructive network changes to production without a change window and tested alternate access.
