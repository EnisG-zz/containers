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

