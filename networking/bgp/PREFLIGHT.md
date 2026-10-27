# Cilium BGP preflight and validation

Reference: https://docs.cilium.io/en/stable/network/bgp-control-plane/bgp-control-plane-configuration/

## Schema and prerequisites
- Verify the installed Cilium version supports `CiliumBGPClusterConfig`, `CiliumBGPPeerConfig` and `CiliumBGPAdvertisement`.
- Enable `bgpControlPlane.enabled=true` in pinned Cilium Helm values; do not assume these CRDs are active merely because YAML exists.
- The default Cilium BGP speaker **initiates** sessions; it does not listen on TCP/179 unless `localPort` is explicitly set. Upstream must accept the node's source IP.
- Validate BGP source-address selection, next-hop reachability, local/peer ASN, and security policy.
- If using `authSecretRef`, create the secret in the configured Cilium BGP secrets namespace via approved secret management. Never commit password values.
- Verify node label `bifrost.io/bgp-speaker=true` is assigned to intended speakers; no match means no BGP configuration.
- Prevent multiple `CiliumBGPClusterConfig` objects selecting the same node.

## Safe inspection (requires valid kubeconfig)
```sh
kubectl get ciliumbgpclusterconfigs,ciliumbgppeerconfigs,ciliumbgpadvertisements
kubectl get ciliumbgpnodeconfigs -o wide
kubectl describe ciliumbgpclusterconfig bifrost-service-bgp
kubectl -n kube-system get pods -l k8s-app=cilium
```

Use the Cilium CLI matching the installed release to inspect peers and advertised routes; verify the upstream RIB/FIB separately.

## Routing and failure cases
- First advertise a single labeled LoadBalancer VIP, with strict upstream prefix filters.
- Remove the Service opt-in label and verify route withdrawal.
- Test one peer outage, one Cilium agent restart and one operator restart.
- Ensure workload endpoint readiness and externalTrafficPolicy behavior do not blackhole traffic.
- Record MTU, return-path, ACL/contract and packet-drop evidence.

**Do not assume that enabling BGP automatically programs native pod routing.**
