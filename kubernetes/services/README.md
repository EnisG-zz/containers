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