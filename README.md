# Overview

The article will document the journey I went through deploying a Private VMware Tanzu Kubernetes Grid (TKG) onto Azure.

*[Tanzu Kubernetes Grid v2.4.x is the last version of TKG that supports the creation of TKG management and workload clusters on Azure. The ability to create TKG workload clusters on Azure will be removed in the Tanzu Kubernetes Grid v2.5 release.](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-release-notes.html)*

I will be covering the below:
- [x] Deploying a private standalone TKG Management cluster onto Azure
- [x] Adding a private Workload cluster to the environment

*By default, Azure management and workload clusters are public. They can be configure to be private, which means their API server utilises an Azure internal load balancer (ILB) and is therefore only accessible from within the cluster’s own VNet or peered VNets*

[Prerequisites](1.0_Prerequisites/README.md)
[Preparing the Bootstrap virtual machine](2.0 Bootstrap Virtual Machine/README.md)
[Deploying the Management cluster](3.0 Management Cluster/README.md)
[Deployng the Workload cluster](4.0 Workload Cluster/README.md)
[Troubleshooting](5.0 Troubleshooting)

