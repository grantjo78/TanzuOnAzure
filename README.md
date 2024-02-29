# Prerequisites

## Outbound Internet Egress Requirements

### Docker 

|URL|Purpose|
|-----|-----|
|https://download.docker.com|Requires for the installation of Docker Engine|

References
https://docs.docker.com/engine/install/ubuntu/


### VMware 
|Description|URL|
|-----|-----|
|projects.registry.vmware.com|VMware plugins registry hosts images, binaries and configuration files used by the Tanzu CLI to perform core functions like creating clusters and managing access.
Tanzu Standard package repository stores images for packaged services that the Tanzu CLI installs into clusters.|
|registry.tkg.vmware.run|Uses Harbor to host images that TKG uses to bootstrap management and workload clusters. Images in this registry are scanned for vulnerabilities and are safe to operate in all environments|

References
https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-reqs-proxy-allowlist.html

### Kind

*Optional if you would like to install Kind to view the clusters*

|URL|Purpose|
|-----|-----|
|https://kind.sigs.k8s.io||
|github.com||
|objects.githubusercontent.com|

References
https://kind.sigs.k8s.io/docs/user/quick-start/
