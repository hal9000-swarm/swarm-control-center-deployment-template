# About
This is an Azure Resource Manager Template to deploy the Swarm Control Center to your Azure enviornment. See https://docs.swarm-analytics.com/swarm-control-center/getting-started/installation for an installation handbook.

# Usage SCC
Generate a App Registration with the predefined values (this is required until registration and assignment of roles is fully supported in Azure).

```bash
instance=<instancename>
postfix=<postfix>
```

```bash
az ad app create  --display-name "${instance}-swarm-control-center" --app-roles @script/appRoles.json \
--required-resource-accesses @script/resourceAccesses.json \
--oauth2-allow-implicit-flow true \
--reply-urls "https://${instance}-swarm-control-center-${postfix}.azurewebsites.net" 

```

This results in a relevant entry:
```bash
"appId": "348b7c41-f824-46f3-9d32-62c7aef1f33c",
```

Approve the APIs:
```bash
az ad app permission admin-consent  --id <appID>
```

Perform the deployment  the deployment link:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fgithub.com%2Fhal9000-swarm%2Fswarm-control-center-deployment-template%2Fblob%2Fmaster%2FsccDeployment.json)

