# Kubernetes package catalog

These package manifests are **planning inventories**, not executable K3s install files.

- [onprem-lab.yaml](onprem-lab.yaml): disposable two-cluster lab
- [onprem-ha.yaml](onprem-ha.yaml): 3-server HA per cluster with workers
- [cloud-hybrid.yaml](cloud-hybrid.yaml): cloud-hosted nodes and on-prem ACI connectivity

The package definitions are validated as YAML but not consumed by Terraform, Helm or K3s. Their keys are Bifrost Ops' own planning schema. The design is explained in [deployment packages](https://github.com/Bifrost-Ops/docs/blob/main/DEPLOYMENT-PACKAGES.md), [interfaces](https://github.com/Bifrost-Ops/docs/blob/main/INTERFACE-MATRIX.md) and [site questionnaire](https://github.com/Bifrost-Ops/docs/blob/main/SITE-QUESTIONNAIRE.md).

**Security:** never place cluster join tokens, credentials or kubeconfigs in these files. Do not install a second CNI without confirming the K3s bootstrap flags and datapath design.
