## Service Load Balancer Demo
Create a service with clusterIP

```
kubectl apply -f service-loadbalancer-demo.yaml

export CLUSTER_IP=$(kubectl get services/webapp1-clusterip-svc -o go-template='{{(index .spec.clusterIP)}}')

echo CLUSTER_IP=$CLUSTER_IP
```

Send multiple requests to service IP, and see response are coming from different pod everytime

```
curl $CLUSTER_IP:80
```

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
