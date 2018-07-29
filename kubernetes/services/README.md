## Service Internal Load Balancer Demo
Create a service with clusterIP

```
kubectl apply -f service-loadbalancer-demo.yaml

# kubectl run -it --rm aks-ssh --image=debian
kubectl exec aks-ssh-7b5b5856cd-js7m6 -it /bin/bash

#Send multiple requests to service IP, and see response are coming from different pod everytime

curl http://webapp1-clusterip-svc

```

Update the service type to LoadBalancer and access from outside of the cluster


## External Service 

Add an application running on a different subnet as a service in k8s. (Assuming win1803 vm has public dnsname: my-win1803.westeurope.cloudapp.azure.com)

```
# on win1803 run
docker run -d -p 80:80 --name mywebapp microsoft/dotnet-samples:aspnetapp
```

Create a service using DNS Name 
```
kubectl apply -f service-with-externalname.yaml
```
Create a service using IP:Port
```
kubectl apply -f service-with-ip-endpoint.yaml
```

Access the external application from pod: 
```
kubectl run -it --rm aks-ssh --image=debian
apt-get update && apt-get install curl 

# Access via Vnet
curl http://win1803

# Access via external DNS name
curl http://win1803-svc

# Access via IP address
curl http://win1803-svc-ip
```


## Configure a service with Static IP 

https://docs.microsoft.com/en-us/azure/aks/static-ip

```
az resource show --resource-group aks-demo --name aks-vnet --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

az network public-ip create --resource-group MC_aks-demo_aks-vnet_westeurope --name myAKSPublicIP --allocation-method static

az network public-ip list --resource-group MC_aks-demo_aks-vnet_westeurope --query [*].ipAddress --output tsv

kubectl apply -f azure-vote-staticip.yaml 
```
Deploying another service with same loadBalancerIP will be stuck in pending state: 

```
kubectl apply -f kuard-with-same-staticip.yaml
```

For multiple service that needs to be accessible from outside of the cluster, it is better to use ingress resource.