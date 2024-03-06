# Overview

The article will document the journey I went through deploying a Private VMware Tanzu Kubernetes Grid (TKG) onto Azure.

*[Tanzu Kubernetes Grid v2.4.x is the last version of TKG that supports the creation of TKG management and workload clusters on Azure. The ability to create TKG workload clusters on Azure will be removed in the Tanzu Kubernetes Grid v2.5 release.](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-release-notes.html)*

I will be covering the below:
- [x] Deploying a private standalone TKG Management cluster onto Azure
- [x] Adding a private Workload cluster to the environment

*By default, Azure management and workload clusters are public. They can be configure to be private, which means their API server utilises an Azure internal load balancer (ILB) and is therefore only accessible from within the cluster’s own VNet or peered VNets*

# Pre-Deployment Prerequisites

Prior to deploying the bootstramp virtual machine and TKG clusters, there are several prerequisites that need to be addressed:
- Creation of Service Principal
- Virtual Network Configurations
  - Subnets
  - Network Security Groups
- Internet Egress Requirements

## Creation of Service Principal
  
## Virtual Network Configuration

The section will cover configuration required at the Azure networking level.

### Subnet Configuration

For my implementation of TKG on Azure I created 3 subnets:
- Bootstrap Subnet - This subnet is where I deployed the virtual machine that I utilised for the deployment of the Management cluster and various operartional tasks.
- Control Subnet - This subnet is where I deployed the the Management cluster
- Workload Subnet - This subnet is where I deployed the Workload cluster.

### Network Security Groups

TKG on Azure requires two Network Security Groups (NSGs) to be defined for the VNet which need to **exist within the VNet resource group**:
- An NSG named CLUSTER-NAME-controlplane-nsg and associated with the cluster’s control plane (Management) subnet
- An NSG named CLUSTER-NAME-node-nsg and associated with the cluster’s worker node (Workload) subnet

*e.g. if the Management cluster was named *tkg01-mgmt* the NSGs would need to be called:*
- *tkg01-mgmt-controlplane-nsg*
- *tkg01-mgmt-node-nsg*

*Giving NSGs names that do not follow the format above may prevent deployment*

#### Bootstrap Subnet Network Security Group
|Direction|Source|Destination|Port|Protocol|
|-----|-----|-----|-----|-----|
|Outbound|Bootstrap Subnet|Management Subnet<br>Workload Subnet|22|TCP|
|Outbound|Bootstrap Subnet|Management Subnet<br>Workload Subnet|6443|TCP|

#### Management Subnet Network Security Group
|Direction|Source|Destination|Port|Protocol|
|-----|-----|-----|-----|-----|
|Inbound|Bootstrap Subnet|Management Subnet|22|TCP|
|Inbound|Bootstrap Subnet|Management Subnet|6443|TCP|
|Outbound|Management Subnet|Workload Subnet|22|TCP|
|Outbound|Management Subnet|Workload Subnet|6443|TCP|

#### Workload Subnet Network Security Group
|Direction|Source|Destination|Port|Protocol|
|-----|-----|-----|-----|-----|
|Inbound|Bootstrap Subnet|Workload Subnet|22|TCP|
|Inbound|Bootstrap Subnet|Workload Subnet|6443|TCP|
|Outbound|Management Subnet|Management Subnet|22|TCP|
|Outbound|Management Subnet|Management Subnet|6443|TCP|

https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-reqs-prep-azure.html

## Internet Egress Requirements

If you are utilising a firewall or proxy solution that requires URLs to explicitly allowed through for internet egress, the below tables documents what I was able to capture.

### Azure
|URL|Ports|Purpose|Source|
|-----|-----|-----|-----|
|login.microsoftonline.com|443|Azure portal authentication|Bootstrap machine|
|*.aadcdn.msftauth.net|443|Azure portal authentication|Bootstrap machine|
|*.aadcdn.msftauthimages.net|443|Azure portal authentication|Bootstrap machine|
|*.aadcdn.msauthimages.net|443|Azure portal authentication|Bootstrap machine|
|*.logincdn.msftauth.net|443|Azure portal authentication|Bootstrap machine|
|login.live.com|443|Azure portal authentication|Bootstrap machine|
|*.msauth.net|443|Azure portal authentication|Bootstrap machine|
|*.aadcdn.microsoftonline-p.com|443|Azure portal authentication|Bootstrap machine|
|*.microsoftonline-p.com|443|Azure portal authentication|Bootstrap machine|
|*.portal.azure.com|443|Azure portal framework|Bootstrap machine|
|*.hosting.portal.azure.net|443|Azure portal framework|Bootstrap machine|
|*.reactblade.portal.azure.net|443|Azure portal framework|Bootstrap machine|
|management.azure.com|443| Azure portal framework|Bootstrap machine|
|*.ext.azure.com|443|Azure portal framework|Bootstrap machine|
|*.graph.windows.net|443|Azure portal framework|Bootstrap machine|
|*.graph.microsoft.com|443|Azure portal framework|Bootstrap machine|
|azure.archive.ubuntu.com|80, 443|Updating Ubuntu|Bootstrap machine |

https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-safelist-urls?tabs=public-cloud


### Docker 
|URL|Port|Purpose|Source|
|-----|-----|-----|-----|
|download.docker.com|443|Requires for the installation of Docker Engine|Bootstrap machine|
|api.segment.io|443|	Analytics|Bootstrap machine|
|cdn.segment.com|443|	Analytics|Bootstrap machine|
|api.wootric.com|443|	Analytics|Bootstrap machine|
|cdn.wootric.com|443| Analytics|Bootstrap machine|
|notify.bugsnag.com|443|	Error reports|Bootstrap machine|
|sessions.bugsnag.com|443|	Error reports|Bootstrap machine|
|auth.docker.io|443|	Authentication|Bootstrap machine|
|cdn.auth0.com|443|	Authentication|Bootstrap machine|
|login.docker.com|443|	Authentication|Bootstrap machine|
|desktop.docker.com|443|	Update|Bootstrap machine|
|hub.docker.com|443|	Docker Pull/Push|Bootstrap machine|
|registry-1.docker.io|443|	Docker Pull/Push|Bootstrap machine|
|production.cloudflare.docker.com|443|	Docker Pull/Push|Bootstrap machine|
|docker-pinata-support.s3.amazonaws.com|433|	Troubleshooting|Bootstrap machine|
|api.dso.docker.com|443|	Docker Scout service|Bootstrap machine|

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/desktop/allow-list/)

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

# Deployment of Management Cluster

This section will cover the deployment of the TKG Management cluster.

## Bootstrap Machine Preparation

- Installation of Azure Cli
- Installation of Tanzu Cli
- Installation 

### Azure Cli

Installation of Azure Cli onto bootstrap virtual machine

References
https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt

### Tanzu Cli

As per the Tanzu Cli [Product Interoperability Matrix](https://interopmatrix.vmware.com/Interoperability?col=1772,&row=0,) only Tanzu Cli **1.0.0** and **1.1.0** are supported for TKG **2.4.1**.

![image](img/TanzuCliMatrix.png)

If you follow the below instructions, this will install the latest Tanzu Cli (1.2.0) which may cause issues.

![image](img/TanzuCli-LatestVersion.png)


To install Tanzu Cli 1.1.0 run the following commands.

```
sudo apt update
sudo apt install -y ca-certificates curl gpg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://packages.vmware.com/tools/keys/VMWARE-PACKAGING-GPG-RSA-KEY.pub | sudo gpg --dearmor -o /etc/apt/keyrings/tanzu-archive-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/tanzu-archive-keyring.gpg] https://storage.googleapis.com/tanzu-cli-os-packages/apt tanzu-cli-jessie main" | sudo tee /etc/apt/sources.list.d/tanzu.list
sudo apt update
sudo apt install -y tanzu-cli=1.1.0
```


# Troubleshooting

## File Locations

|File Name/Type|Location|Purpose|
|-----|-----|-----|
