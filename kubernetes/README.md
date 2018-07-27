## Create ClusterRoleBinding for RBAC-Enabled Clusters: 

https://docs.microsoft.com/en-us/azure/aks/kubernetes-dashboard

```
$ kubectl apply -f dashboard-admin.yaml
```

## Configure a service with Static IP 

https://docs.microsoft.com/en-us/azure/aks/static-ip

```
az resource show --resource-group aks-demo --name aks-vnet --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

az network public-ip create --resource-group MC_aks-demo_aks-vnet_westeurope --name myAKSPublicIP --allocation-method static

az network public-ip list --resource-group MC_aks-demo_aks-vnet_westeurope --query [?].ipAddress --output tsv

kubectl apply -f azure-vote-staticip.yaml 
```
Deploying another service with same loadBalancerIP will be stuck in pending state: 

```
kubectl apply -f kuard-with-same-staticip.yaml
```

# Storage 

## Deploy an application which uses persistent storage
### Using Azure Files

Create Azure Files storage
```
az storage account create --resource-group MC_aks-demo_aks-vnet_westeurope --name aksdemostorageaccount --sku Standard_LRS
```

Workaround due to this error: User "system:serviceaccount:kube-system:persistent-volume-binder" cannot create secrets in the namespace "default"

https://github.com/Azure/AKS/issues/525
```
kubectl apply -f azure-file-workaround-grant-permission.yaml
```

Create storage class 
```
kubectl apply -f azure-file-storage-class.yaml
```
Create volume claim 
```
kubectl apply -f azure-file-volume-claim.yaml
```
Create pod
```
kubectl apply -f azure-file-pod.yaml
```

### Using Azure Disk

You can also make use of pre-configured managed-premium storage class
```
kubectl apply -f azure-disk-premium-volume-claim.yaml
kubectl apply -f azure-disk-pod.yaml
```

# ACR
## Create a registry 

az acr create --resource-group aks-demo --name myacr10 --sku Basic
az acr login --name myacr10 
