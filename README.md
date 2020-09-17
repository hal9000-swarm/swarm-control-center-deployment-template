# About
This is a Azure Resource Manager Template to deploy SCC Frontend and SCC API Gateway. See:
* https://github.com/hal9000-swarm/swarm-control-center-api-gateway
* https://github.com/hal9000-swarm/swarm-control-center-frontend

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhal9000-swarm%2Fscc-deployment%2Fmaster%2FsccDeployment.json%3Ftoken%3DAOONTU4HZBZPLJT2FETVUIC6XVTNI)

# Usage
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


Navigate to the new app registration and create a service principal / Enterprise Application for it, assign roles and owners


* See https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deployment-script-template?tabs=CLI for further extensions.
* See https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deployment-tutorial-local-template for a general description. 

Copy and adapt the parameter file for a specific customer.

Example via the Azure CLI (https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)
```bash
az deployment group create \
  --name "${instance}-swarm-control-center" \
  --resource-group "swarm-demo" \
  --template-file sccDeployment.json \
  --parameters deployments/${instance}Parameters.json
```
