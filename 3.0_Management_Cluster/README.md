To deploy the TKG management cluster from the [Bootstap virtual machine](/2.0_Bootstrap_Virtual_Machine/README.md), the following steps are performed:

- Creation of Management Cluster configuration File
- 

# Creation of Management Cluster configuration file

The management cluster can be deployed in two ways:
1.  [Deploy Management Clusters with the Installer Interface](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-deploy-ui.html)
2.  [Deploy Management Clusters from a Configuration File](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-deploy-ui.html) 

The configuration file option allows for greater flexibility of configuration and faster/repeatable deployments.

The settings relevant for the deployment of the management cluster onto Azure are covered below.

`CLUSTER_NAME`: Specifies the name to give the cluster.
`CLUSTER_PLAN`:

```
CLUSTER_NAME: tkg01-mgmt-dev
CLUSTER_PLAN: dev
CLUSTER_ANNOTATIONS: 'description:,location:'
CLUSTER_CIDR: 100.96.0.0/11

```

`INFRASTRUCTURE_PROVIDER`: specifies which platform the management cluster will be deployed onto. This needs to be set to `azure`.

```
INFRASTRUCTURE_PROVIDER: azure
```
`AZURE_ENVIRONMENT`: Azure cloud enviroment to be utilied (AzurePublicCloud. AzureChinaCloud, AzureUSGovernmentCloud) <br>
`AZURE_TENANT_ID`: Tenant ID for the Microsoft Entra ID environment that cluster will be deployed into. <br>
`AZURE_SUBSCRIPTION_ID`: Subscribe ID for the Subscription that the cluster will be deployed into. <br>
`AZURE_RESOURCE_GROUP`: Resource group name of the resource group that the cluster will be deployed into. <br>
`AZURE_CLIENT_ID`: Service Principal Application Id that was created as part of the [prerequisites](/1.0_Prerequisites/README.md). 

>[!IMPORTANT]
>The Application Id  is required to be Base64 encoded.

`AZURE_CLIENT_SECRET`: Service Principal secret that was created as part of the [prerequisites](/1.0_Prerequisites/README.md). <br>

>[!IMPORTANT]
>The secret  is required to be Base64 encoded.

`AZURE_LOCATION`: Region where management cluster will be deployed to. <br>

```
AZURE_TENANT_ID: 123456789-1112-1314f-1516-1718192021
AZURE_SUBSCRIPTION_ID: 7bd70808-dc91-47fd-ae31-3b32ac66a85b
AZURE_RESOURCE_GROUP: arg-rt-hdev-tanzu-control
AZURE_CLIENT_ID: <encoded:YWJjZGVmZ2hpamtsbW5vcHF1cnN0dXd4eXo=>
AZURE_CLIENT_SECRET: <encoded:MTIzNDU2Nzg5MTA=>
AZURE_LOCATION: australiaeast
```

`AZURE_VNET_NAME`: Name of the virtual network that the management cluster will utilise. <br>
`AZURE_VNET_CIDR`: Network CIDR range of the virtual network. <br>
`AZURE_VNET_RESOURCE_GROUP`: Resource group name that the virtual network exists in. <br>
`AZURE_CONTROL_PLANE_MACHINE_TYPE`: Management cluster control plane virtual machine SKU to be utilised.<br>
`AZURE_CONTROL_PLANE_SUBNET_NAME`: Subnet name that the management clusters control plane will be deployed into. <br>
`AZURE_CONTROL_PLANE_SUBNET_CIDR`: Subnet CIDR that the management clusters control plane will be deployed into. <br>
`AZURE_NODE_MACHINE_TYPE`:  Management cluster workload node virtual machine SKU to be utilised.<br>
`AZURE_NODE_SUBNET_NAME`: Subnet name that the management clusters workload nodes will be deployed into. <br>
`AZURE_NODE_SUBNET_CIDR`: Subnet CIDR that the management clusters workload nodes will be deployed into. <br>

```
AZURE_VNET_NAME: vnt-rt-hdev-tanzupoc-10.243.70.0
AZURE_VNET_RESOURCE_GROUP: arg-rt-hdev-tanzupoc
AZURE_VNET_CIDR: 10.243.70.0/26
AZURE_CONTROL_PLANE_MACHINE_TYPE: Standard_D8s_v5
AZURE_CONTROL_PLANE_SUBNET_NAME: Control
AZURE_CONTROL_PLANE_SUBNET_CIDR: 10.243.70.16/28
AZURE_NODE_MACHINE_TYPE: Standard_D8s_v5
AZURE_NODE_SUBNET_NAME: Workload
AZURE_NODE_SUBNET_CIDR: 10.243.70.32/28
```

`AZURE_ENABLE_PRIVATE_CLUSTER`: To enable cluster to be a Private Cluster, set value to `true`<br>
`AZURE_FRONTEND_PRIVATE_IP`: If using a Private Cluster, specify the internal IP address to be utilised for the load balancer<br>

```
AZURE_ENABLE_PRIVATE_CLUSTER: true
AZURE_FRONTEND_PRIVATE_IP: 10.243.70.25
```

`AZURE_SSH_PUBLIC_KEY_B64`: SSH public key that was created as part of the [prerequisites](/1.0_Prerequisites/README.md). 

>[!IMPORTANT]
>The public key is required to be Base64 encoded.

```
AZURE_SSH_PUBLIC_KEY_B64: c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFDQVFDN3Z1eFc2TjZhVzZmNklFU1pXL3VNK0Z3ekhzenFqdmJGTEFmcTFOVUhKR0pHRmJPb21EQm92UktBMnNaRlRKUXk0QnFhN3RuaHNjdDRFUHQxUGQvUFVBSWpEYm9jc2JRbUQrVUdlS2dmYVU0WnNqYXFwS3VDdTh1OHdJWVJWUitoVHJnYW42cHprNGZMT0hQRkxyeSs5QmZ0eUVlV3dMbldwYStjYTkyRlBST2psWUZYNGt6YlgydXhxZGw2RHMwRGl2bVkwdTFNcHUxZFB4SnpBVXhDZnJ5QU0vbnRrOWtDS3RZRGRjalNzMkZTR0s0TWlJYU1PTmNWd3lCNHk1d3dOaUJ0VDB5WGRRQTJFY

```
# Creation of Management Cluster 

```
tanzu management-cluster create --file tkg-mgmt-01.yaml 
```

![image](img/ClusterCreate.png)

References
- [Deploy Management Clusters from a Configuration File](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-deploy-file.html))
- [How to base64 encode and decode from command-line](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-base64-encode-and-decode-from-command-line/)
- [Configuration File Variable Reference](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/config-ref.html)