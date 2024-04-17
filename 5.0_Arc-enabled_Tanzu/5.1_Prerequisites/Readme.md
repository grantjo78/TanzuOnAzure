# Internet Egress Requirements

|URL|Ports|Purpose|Source|
|-----|-----|-----|-----|
|management.azure.com|443|Required for the agent to connect to Azure and register the cluster||
|<region>.dp.kubernetesconfiguration.azure.com|443|	Data plane endpoint for the agent to push status and fetch configuration information.|
|login.microsoftonline.com <br> <region>.login.microsoft.com <br> login.windows.net|443|Required to fetch and update Azure Resource Manager tokens.||
|mcr.microsoft.com <br> *.data.mcr.microsoft.com|443|	Required to pull container images for Azure Arc agents.||
|gbl.his.arc.azure.com|443|	Required to get the regional endpoint for pulling system-assigned Managed Identity certificates.||
|*.his.arc.azure.com|443| Required to pull system-assigned Managed Identity certificates.||
|k8connecthelm.azureedge.net|443|az connectedk8s connect uses Helm 3 to deploy Azure Arc agents on the Kubernetes cluster. This endpoint is needed for Helm client download to facilitate deployment of the agent helm chart.||
|guestnotificationservice.azure.com<br> *.guestnotificationservice.azure.com
sts.windows.net <br> k8sconnectcsp.azureedge.net|443|For Cluster Connect and for Custom Location based scenarios.||
|*.servicebus.windows.net|443|For Cluster Connect and for Custom Location based scenarios.||
|graph.microsoft.com|443|Required when Azure RBAC is configured.|

>[!NOTE]
>To translate the *.servicebus.windows.net wildcard into specific endpoints, use the command:
>```
> GET https://guestnotificationservice.azure.com/urls/allowlist?> api-version=2020-01-01&location=<region>
>```
> Substitute `region` with the name to the region to obtain the endpoints for.

References:
  - [Azure Arc-enabled Kubernetes network requirements](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/network-requirements?tabs=azure-cloud)
  
# Install connectedk8s extension

From the bootstrap virtual machine enter the following command to install the `connectedk8s` extension for Azure CLI.

![image](/5.0_Arc-enabled_Tanzu/5.1_Prerequisites/img/connectedk8s-Install.png)

```
az extension add --name connectedk8s
```

# Register providers for Azure Arc-enabled Kubernetes

From the bootstrap virtual machine enter the following command to register the Azure Arc resource providers.

```
az provider register --namespace Microsoft.Kubernetes
az provider register --namespace Microsoft.KubernetesConfiguration
az provider register --namespace Microsoft.ExtendedLocation
```

![image](/5.0_Arc-enabled_Tanzu/5.1_Prerequisites/img/ArcResourceProvider-Install.png)

To check the resource providers registration status, enter the following commands.

```
az provider show -n Microsoft.Kubernetes -o table
az provider show -n Microsoft.KubernetesConfiguration -o table
az provider show -n Microsoft.ExtendedLocation -o table
```

![image](/5.0_Arc-enabled_Tanzu/5.1_Prerequisites/img/ArcResourceProvider-Status.png)

