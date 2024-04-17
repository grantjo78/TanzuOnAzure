
# Arc-enable Tanzu Management Cluster


>[!NOTE]
The following command will connect to the Arc via a public endpoint.
```
name="tkg-mgmt-01"
resourceGroupName="rg-tkg-arc"

az connectedk8s connect \
    --name $name \
    --resource-group $resourceGroupName
```

![image](/5.0_Arc-enabled_Tanzu/5.2_Deployment/img/clusterConnect-Install.png)


![image](/5.0_Arc-enabled_Tanzu/5.2_Deployment//img/AzureArc-Overview.png)


![image](/5.0_Arc-enabled_Tanzu/5.2_Deployment//img/AzureArc-Resource.png)