
## File Locations

|File Name/Type|Location|Purpose|
|-----|-----|-----|


## Azure Activity Logs

The Azure activity logs on the resource group where the management and workload groups are being deployed to can provide useful information as to why deployment my be failing.

In the below explain, we can see that this deployment is failing because there is an Azure policy in place that prevents the creation on public IP's.
![image](img/AzureActivityLog-PolicyIssue.png)
