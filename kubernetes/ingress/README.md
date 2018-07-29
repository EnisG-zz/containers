## AKS Application Routing 

```
az aks show --resource-group aks-demo --name aks-vnet --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table

# Result: f9c24a603dba491aa920.westeurope.aksapp.io

kubectl apply -f http-application-routing.yaml
```


## Manually adding a DNS record 

Install nginx ingress controller 
```
helm install stable/nginx-ingress --namespace kube-system

#check load balancer status: 
kubectl --namespace kube-system get services -o wide -w yummy-swan-nginx-ingress-controller 
```

Get Public IP resource of nginx controller
```
$IP="40.91.212.234"
$PUBLICIPID=az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv
```

Define a DNS name to a Public IP 
```
$DNSNAME="my-aks-ingress" 
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```


Create ingress route
```
kubectl apply -f manual-http-routing.yaml
curl http://my-aks-ingress.westeurope.cloudapp.azure.com/party-clippy
curl http://my-aks-ingress.westeurope.cloudapp.azure.com/webapp-ingress
```
