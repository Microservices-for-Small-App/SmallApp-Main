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

## Create the Kubernetes namespace

```powershell
$namespace="identityservice"
kubectl create namespace $namespace
```

## Create the Kubernetes secrets

```powershell
$adminPass="Sample@123$"
$cosmosDbConnString="[Azure Cosmos DB CONN STRING HERE]"
$serviceBusConnString="[CONN STRING HERE]"

kubectl create secret generic identity-secrets --from-literal=cosmosdb-connectionstring=$cosmosDbConnString --from-literal=servicebus-connectionstring=$serviceBusConnString --from-literal=admin-password=$adminPass -n $namespace

kubectl get secrets -n $namespace
```

## Create the Kubernetes pod

```powershell
kubectl apply -f .\K8s\identity.yaml -n $namespace

kubectl get pods -n $namespace
kubectl logs PodName -n $namespace
kubectl describe pod PodName -n $namespace

kubectl get services -n $namespace
```

## Few commands to manage the AKS cluster

```powershell
az aks get-credentials --name $aksname --resource-group $rgname

kubectl version

kubectl cluster-info

kubectl get secret  -o wide

az aks browse --name $aksname --resource-group $rgname

az aks scale --name $aksname --resource-group $rgname --node-count 3

az aks nodepool add --resource-group $rgname --cluster-name $aksname --name nodepool2 --node-count 1 --node-vm-size Standard_B2s --labels service=backend

az aks nodepool delete --resource-group $rgname --cluster-name $aksname --name nodepool2

az aks nodepool list --resource-group $rgname --cluster-name $aksname -o table

az aks nodepool scale --resource-group $rgname --cluster-name $aksname --name nodepool1 --node-count 3

az aks nodepool update --resource-group $rgname --cluster-name $aksname --name nodepool1 --labels service=backend

az aks nodepool upgrade --resource-group $rgname --cluster-name $aksname --name nodepool1 --kubernetes-version 1.19.11

az aks nodepool show --resource-group $rgname --cluster-name $aksname --name nodepool1

az aks nodepool get-upgrades --resource-group $rgname --cluster-name $aksname --nodepool-name nodepool1

az aks nodepool upgrade --resource-group $rgname --cluster-name $aksname --nodepool-name nodepool1 --kubernetes-version 1.20.7

az aks nodepool upgrade --resource-group $rgname --cluster-name $aksname --nodepool-name nodepool1 --kubernetes-version 1.21.1

az aks nodepool upgrade --resource-group $rgname --cluster-name $aksname --nodepool-name nodepool1 --kubernetes-version 1.21.2
```