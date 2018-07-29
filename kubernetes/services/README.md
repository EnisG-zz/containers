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

Add an application running on a different subnet as a service in k8s

Using DNS Name 
```
kubectl apply -f service-with-externalname.yaml
```
Using IP:Port
```
kubectl apply -f service-with-ip-endpoint.yaml
```
