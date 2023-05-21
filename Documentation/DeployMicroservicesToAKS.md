# Deploy Microservices from Azure Container Registry to Azure Kubernetes Services

## Creating the AKS cluster

```powershell
az extension add --name aks-preview
az feature register --name "EnableWorkloadIdentityPreview" --namespace "Microsoft.ContainerService"

# Wait for this command to reach a "Registered" state
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableWorkloadIdentityPreview')].{Name:name,State:properties.state}"

az provider register --namespace "Microsoft.ContainerService"

$rgname="rg-playeconomy-dev-001"
$acrname="acrplayeconomydev001"
$aksname="aks-playeconomy-dev-001"

az aks create -n $aksname -g $rgname --node-vm-size Standard_B2s --node-count 2 --attach-acr $acrname --enable-oidc-issuer --enable-workload-identity --generate-ssh-keys

az aks get-credentials --name $aksname --resource-group $rgname
```
