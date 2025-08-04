

## Creating an NCC object.
Endpoint: POST https://accounts.azuredatabricks.net/api/2.0/accounts/{{accountId}}/network-connectivity-configs
Permission: Databricks Account Admin
Payload: name (<string>), region (<string>)
Attach the NCC object to one or more workspaces.
Endpoint: PATCH https://accounts.azuredatabricks.net/api/2.0/accounts/{{accountId}}/workspaces/{{<workspace-id>}}
Permission: Databricks Account Admin
Payload: network_connectivity_config_id (<string>)
Obtain the NCC object details and record the subnet IDs.
Endpoint: GET https://accounts.azuredatabricks.net/api/2.0/accounts/{{accountId}}/workspaces/{{workspaceId}}/network-connectivity-configs
Permission: Databricks Workspace Admin
Add the subnet IDs to the firewall of the Azure storage accounts.
Endpoint: (Azure CLI command) az storage account network-rule add 
Parameters: --resource-group, --account-name, --subscription, --subnet


Add the subnet IDs to the firewall of the Azure storage accounts.
Endpoint: (Azure CLI command) az storage account network-rule add 
Parameters: --resource-group, --account-name, --subscription, --subnet