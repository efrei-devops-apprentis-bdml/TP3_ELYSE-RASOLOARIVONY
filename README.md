## Auteur : RASOLOARIVONY ELYSE - M1 BDIA - EFREI

#### Objectifs :
* Deployer sur Azure Container Instance (ACI) using Github Actions
* Mettre à disposition son image (format API) sur Azure Container Registry (ACR) using
Github Actions
* Mettre à disposition son code dans un repository Github

#### Bonus :
* Add hadolint to Github workflow before build+push and failed on errors
* Configurer une probe liveness HTTP
* Aucune données sensibles stockées dans l'image ou le code source (i.e: openweather API
key, Docker hub credentials)

#### Notation :
- Code disponible sur Github
- Github action qui build et push l'image à chaque nouveau commit sur ACR
- Container deployé sur Azure Container Instance
- API qui renvoie la météo en utilisant la commande suivante :
```bash
curl "http://devops-<identifiant-efrei>.francecentral.azurecontainer.io/?
lat=5.902785&lon=102.754175"
```
puis dans un autre terminal :
```bash
curl "http://localhost:8081/?lat=5.902785&lon=102.754175"
```

#### Contraintes et indications :
* *abrév* ACI = Azure Container Instances et ACR = Azure Container Registry
* location : **france central**
* dns-name-label : **devops-< identifiant-efrei >**
* utiliser l'organisation Github et ses secrets : [github Efrei](https://github.com/efrei-devops-apprentis-bdml)
* Important links : 
	- [Deploy Container instance / Configure a GitHub action to create a container instance](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-github-action#save-credentials-to-github-repo)
	- [Create an Azure Container Registry using the Azure portal](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal)
	- [Create an Azure Container Registry using Azure CLI](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli)
	- [ACI deploy](https://github.com/azure/aci-deploy)
NB : If you want to use *Azure Cloud Shell* or *local installation of the Azure CLI* to complete the Azure CLI steps. [Install Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) (to create ACR or doing sthg else). ake note of the resource group used for the deployment, which is used for the GitHub workflow.

* Azure Container Registry : efreidevops.azurecr.io
* ACI name : identifiant EFREI (example: 11002167 )
* ACR repository name : identifiant EFREI (example: efreidevops.azurecr.io/11002167:v1 )


#### Livrable : 
* Livrables
URL repository Github (avec Dockerfile, l'API et la configuration Github Action)
* Rapport qui présente, étape par étape, les choix techniques et les difficultées rencontrées
si vous n'avez pas pu aller jusqu'au bout. Commenter également l'intérêt de l'utilisation
d'une Github Action pour deployer plutôt que via l'interface utilisateur ou la CLI.


#### STEPS :
* 1. Install Azure CLI
* 2. run > az login 
```bash 
rasel@rasel:~$ az login
A web browser has been opened at https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize. Please continue the login in the web browser. If no web browser is available or if the web browser fails to open, use device code flow with `az login --use-device-code`.
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "413600cf-bd4e-4c7c-8a61-69e73cddf731",
    "id": "5e344786-8ad6-45d1-91f9-3bc8b31730d0",
    "isDefault": false,
    "managedByTenants": [],
    "name": "Azure for Students",
    "state": "Disabled",
    "tenantId": "413600cf-bd4e-4c7c-8a61-69e73cddf731",
    "user": {
      "name": "elyse.rasoloarivony@efrei.net",
      "type": "user"
    }
  },
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "413600cf-bd4e-4c7c-8a61-69e73cddf731",
    "id": "14f3b93b-01dc-4e9e-8688-5b2dd60107ce",
    "isDefault": false,
    "managedByTenants": [],
    "name": "Azure for Students",
    "state": "Disabled",
    "tenantId": "413600cf-bd4e-4c7c-8a61-69e73cddf731",
    "user": {
      "name": "elyse.rasoloarivony@efrei.net",
      "type": "user"
    }
  },
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "413600cf-bd4e-4c7c-8a61-69e73cddf731",
    "id": "765266c6-9a23-4638-af32-dd1e32613047",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Efrei - Apprentis BDML",
    "state": "Enabled",
    "tenantId": "413600cf-bd4e-4c7c-8a61-69e73cddf731",
    "user": {
      "name": "elyse.rasoloarivony@efrei.net",
      "type": "user"
    }
  }
]


```