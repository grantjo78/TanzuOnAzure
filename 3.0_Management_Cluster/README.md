To deploy the TKG management cluster from the [Bootstap virtual machine](/2.0_Bootstrap_Virtual_Machine/README.md), the following steps are performed:

- Creation of Management Cluster configuration File
- 

# Creation of Management Cluster configuration file

The management cluster can be deployed in two ways:
1.  [Deploy Management Clusters with the Installer Interface](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-deploy-ui.html)
2.  [Deploy Management Clusters from a Configuration File](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-deploy-ui.html) 

The configuration file option allows for greater flexibility of configuration and faster/repeatable deployments.

The settings relevant for the deployment of the management cluster onto Azure are covered below.

`INFRASTRUCTURE_PROVIDER` specifies which platform the management cluster will be deployed onto. This needs to be set to `azure`.

```
INFRASTRUCTURE_PROVIDER: azure
```
`AZURE_ENVIRONMENT`: Azure cloud enviroment to be utilied (AzurePublicCloud. AzureChinaCloud, AzureUSGovernmentCloud) <br>
`AZURE_TENANT_ID`: Tenant ID for the Microsoft Entra ID environment that cluster will be deployed into. <br>
`AZURE_SUBSCRIPTION_ID`: Subscribe ID for the Subscription that the cluster will be deployed into. <br>
`AZURE_RESOURCE_GROUP`: Resource group name of the resource group that the cluster will be deployed into. <br>
`AZURE_CLIENT_ID`: Service Principal Application Id that was created as part of the [prerequisites](/1.0_Prerequisites/README.md). The Application Id is required to be Base64 encoded.<br>
`AZURE_CLIENT_SECRET`: Service Principal secret that was created as part of the [prerequisites](/1.0_Prerequisites/README.md). The secret is required to be Base64 encoded.<br>
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
`AZURE_CONTROL_PLANE_MACHINE_TYPE`: Azure Virtual Machine sku to be utilised for the deployment of the management cluster.
`AZURE_CONTROL_PLANE_SUBNET_NAME`:
`AZURE_CONTROL_PLANE_SUBNET_CID`:
`AZURE_NODE_MACHINE_TYPE`:
`AZURE_NODE_SUBNET_NAME`:
`AZURE_NODE_SUBNET_CIDR`:

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
`AZURE_ENABLE_PRIVATE_CLUSTER`: <br>
`AZURE_FRONTEND_PRIVATE_IP`: <br?>

```
AZURE_ENABLE_PRIVATE_CLUSTER: true
AZURE_FRONTEND_PRIVATE_IP: 10.243.70.25
```

```
AZURE_SSH_PUBLIC_KEY_B64: c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFDQVFDN3Z1eFc2TjZhVzZmNklFU1pXL3VNK0Z3ekhzenFqdmJGTEFmcTFOVUhKR0pHRmJPb21EQm92UktBMnNaRlRKUXk0QnFhN3RuaHNjdDRFUHQxUGQvUFVBSWpEYm9jc2JRbUQrVUdlS2dmYVU0WnNqYXFwS3VDdTh1OHdJWVJWUitoVHJnYW42cHprNGZMT0hQRkxyeSs5QmZ0eUVlV3dMbldwYStjYTkyRlBST2psWUZYNGt6YlgydXhxZGw2RHMwRGl2bVkwdTFNcHUxZFB4SnpBVXhDZnJ5QU0vbnRrOWtDS3RZRGRjalNzMkZTR0s0TWlJYU1PTmNWd3lCNHk1d3dOaUJ0VDB5WGRRQTJFY1RNM2RQRitveFFMMFJDTG5tVC9iZFFjTVEvY2pmRlZUdlBGWHBJZytBOWh5V1Z3ODNFQWcrUHg2bTQ3NmphTTkwZkJzVlhnVTdwNEVkeHRrRFg3bG0xMnBlNnphaUZwNHVUZ0QwNTZJTXJKU2hRakdRZm1XK003WFU4ZmE2VkdHM3JES05BY3pWTHRESXcxZWxCM0IwaklabHQyZHdDWkxselF6c3U1VTlIYlVhekN4bFBIVkdlUXRtL1FYLzZ0SUtmaDdYREwybkplNUtURk1YRXFIWGt0SzhGQnZjYkZuYjhYQjZ4Y2J3eWE0Y3J0ZjNvaHEwd0NHeFV4N2RNazBxVHNnMldCQlgwQzh3djVRWkZROEwwS0MxYzNkRDY4bFF6bm1CeDZIbU9wTWF2Z2p4bFUwM0dnNWw4emtHTzVPUHhYdEUxSUxPQU8wQ1QrWVNtQnhoYTFCbGQzb2tBNXdIdmwwaWFPTFhXbDZjUVBtSzlhekVHZFl0azlnUCtESTBqZldNQ1B5TW92MzJNTU4xcFYzOG9UOXQ2VlE9PSBqb2hhbi5ncmFudEByaW90aW50by5jb20=

```

```
CLUSTER_ANNOTATIONS: 'description:,location:'
CLUSTER_CIDR: 100.96.0.0/11
CLUSTER_NAME: rt-tkg01-mgmt-dev
CLUSTER_PLAN: dev
```

```
ENABLE_AUDIT_LOGGING: ""
ENABLE_CEIP_PARTICIPATION: "false"
ENABLE_MHC: "false"
IDENTITY_MANAGEMENT_TYPE: none
LDAP_BIND_DN: ""
LDAP_BIND_PASSWORD: ""
LDAP_GROUP_SEARCH_BASE_DN: ""
LDAP_GROUP_SEARCH_FILTER: ""
LDAP_GROUP_SEARCH_GROUP_ATTRIBUTE: ""
LDAP_GROUP_SEARCH_NAME_ATTRIBUTE: cn
LDAP_GROUP_SEARCH_USER_ATTRIBUTE: DN
LDAP_HOST: ""
LDAP_ROOT_CA_DATA_B64: ""
LDAP_USER_SEARCH_BASE_DN: ""
LDAP_USER_SEARCH_FILTER: ""
LDAP_USER_SEARCH_NAME_ATTRIBUTE: ""
LDAP_USER_SEARCH_USERNAME: userPrincipalName
OIDC_IDENTITY_PROVIDER_CLIENT_ID: ""
OIDC_IDENTITY_PROVIDER_CLIENT_SECRET: ""
OIDC_IDENTITY_PROVIDER_GROUPS_CLAIM:
```
References
- [Deploy Management Clusters from a Configuration File](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/mgmt-deploy-file.html))
- [How to base64 encode and decode from command-line](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-base64-encode-and-decode-from-command-line/)
- [Configuration File Variable Reference](https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/2.4/tkg-deploy-mc/config-ref.html)