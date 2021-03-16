# About
This is an Azure Resource Manager Template to deploy the Swarm Control Center to your Azure enviornment. See https://docs.swarm-analytics.com/swarm-control-center/getting-started/installation for an installation handbook.
Includes also first template for data-discovery deployment (database and data-piping functions app at the moment).


# Usage SCC
Generate a App Registration with the predefined values (this is required until registration and assignment of roles is fully supported in Azure).

```bash
instance=<instancename>
```

```bash
az ad app create  --display-name "${instance}-swarm-control-center" --app-roles @script/appRoles.json \
--required-resource-accesses @script/resourceAccesses.json \
--oauth2-allow-implicit-flow true \
--reply-urls "https://${instance}-swarm-control-center.azurewebsites.net" 

```

This results in a relevant entry:
```bash
"appId": "348b7c41-f824-46f3-9d32-62c7aef1f33c",
```

Approve the APIs:
```bash
az ad app permission admin-consent  --id <appID>
```

Perform the deployment via the Azure CLI or use the template via the deployment link:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fgithub.com%2Fhal9000-swarm%2Fswarm-control-center-deployment-template%2Fblob%2Fmaster%2FsccDeployment.json)

```bash
az deployment group create \
  --name "${instance}-swarm-control-center" \
  --resource-group "swarm-demo" \
  --template-file sccDeployment.json \
  --parameters deployments/${instance}Parameters.json
```

# Usage Data Discovery


Go to azure portal -> 
create resource -> 
template deployment -> 
create -> 
build your own template -> 
load file -> 
select data-discovery-deployment-template ->
save ->
insert values ->
review and create ->
create


# Usage SCC + Data Discovery
Follow the steps of 'Usage SCC' until 'Approve APIs' to create the Enterprise Application

Go to azure portal -> 
create resource -> 
template deployment -> 
create -> 
build your own template -> 
load file -> 
select scc-dd-deployment-template ->
save ->
insert values ->
review and create ->
create

The docker registry key and the bacpac storage key can be found in 1Password ('swarm docker registry' and 'data discovery storage account key')

The scc frontend name has to be corresponding to the reply URI of the AD Application. e.g. 'test-swarm-control-center' if the reply-URI is https://test-swarm-control-center.azurewebsites.net

Your SCC with data-discovery should be up and running after everything has deployed and all docker-containers are running. (CUBEJS_AD_APP_TENANT has to be set manually again.)

DPS-Enrollment-group still has to be created in order to add devices to the control center. CUBEJS_AD_APP_TENANT has to be set manually again.