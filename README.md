# Overview

The article will document the journey I went through deploying a VMware Tanzu Kubernetes Grid (TKG) onto Azure.

My objective was to:
- [x] Deploy an Azure Standalone Tanzu Kubernetes Grid Management Cluster
- [x] Append a Workload Cluster to the environment
- [x] Ensure that the cluster API are only accessible from within the vNet

To restrict the cluster API to within the vNet, the  TKG clusters need to be deployed as Private Clusters. By default, Azure management and workload clusters are public. They can be configure to be private, which means their API server uses an Azure internal load balancer (ILB) and is therefore only accessible from within the cluster’s own VNet or peered VNets

*[Tanzu Kubernetes Grid v2.4.x is the last version of TKG that supports the creation of TKG management and workload clusters on Azure. The ability to create TKG workload clusters on Azure will be removed in the Tanzu Kubernetes Grid v2.5 release.](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-release-notes.html)*

# Pre-Deployment Prerequisites

Prior to deploying the bootstramp virtual machine and TKG clusters, there are several prerequisites that need to be addressed:
- Creation of Service Principal
- Virtual Network Configuration
  - Subnets
  - Network Security Groups
- Internet Egress Requirements

## Creation of Service Principal
  
## Virtual Network Configuration

### Subnet Configuration

### Network Security Groups

## Internet Egress Requirements

If you are utilising a firewall or proxy solution that requires URLs to explicitly Allowed through for internet egress, the below tables documents what I was able to capture.

### Azure
|URL|Purpose|Source|
|-----|-----|-----|
|login.microsoftonline.com|Azure portal authentication|Bootstrap machine|
|*.aadcdn.msftauth.net|Azure portal authentication|Bootstrap machine|
|*.aadcdn.msftauthimages.net|Azure portal authentication|Bootstrap machine|
|*.aadcdn.msauthimages.net|Azure portal authentication|Bootstrap machine|
|*.logincdn.msftauth.net|Azure portal authentication|Bootstrap machine|
|login.live.com|Azure portal authentication|Bootstrap machine|
|*.msauth.net|Azure portal authentication|Bootstrap machine|
|*.aadcdn.microsoftonline-p.com|Azure portal authentication|Bootstrap machine|
|*.microsoftonline-p.com|Azure portal authentication|Bootstrap machine|
|*.portal.azure.com|Azure portal framework|Bootstrap machine|
|*.hosting.portal.azure.net|Azure portal framework|Bootstrap machine|
|*.reactblade.portal.azure.net|Azure portal framework|Bootstrap machine|
|management.azure.com|Azure portal framework|Bootstrap machine|
|*.ext.azure.com|Azure portal framework|Bootstrap machine|
|*.graph.windows.net|Azure portal framework|Bootstrap machine|
|*.graph.microsoft.com|Azure portal framework|Bootstrap machine|
|azure.archive.ubuntu.com|Updating Ubuntu|Bootstrap machine |

https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-safelist-urls?tabs=public-cloud


### Docker 
|URL|Purpose|Source|
|-----|-----|-----|
|download.docker.com|Requires for the installation of Docker Engine|Bootstrap machine|

https://docs.docker.com/engine/install/ubuntu/

### VMware 
|Description|URL|Source|
|-----|-----|-----|
|projects.registry.vmware.com|VMware plugins registry hosts images, binaries and configuration files used by the Tanzu CLI to perform core functions like creating clusters and managing access. Tanzu Standard package repository stores images for packaged services that the Tanzu CLI installs into clusters.| Bootstrap machine|
|registry.tkg.vmware.run|Uses Harbor to host images that TKG uses to bootstrap management and workload clusters. Images in this registry are scanned for vulnerabilities and are safe to operate in all environments|Bootstrap machine|

https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-reqs-proxy-allowlist.html

### Kind

*Optional if you would like to install Kind to view the clusters*

|URL|Purpose|Source|
|-----|-----|-----|
|kind.sigs.k8s.io||Bootstrap machine||
|github.com||Bootstrap machine|
|objects.githubusercontent.com||Bootstrap machine|

https://kind.sigs.k8s.io/docs/user/quick-start/

# Deployment

## Bootstrap Machine Preparation

### Azure Cli

Installation of Azure Cli onto bootstrap virtual machine

References
https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt

# Issues
