## Create Pod

```
kubectl apply -f nginx-pod.yaml
kubectl get pods 
```

## Liveness Probe 

```
kubectl apply -f liveness-probe.yaml
kubectl get pods 
```

## Create ReplicaSet

```
kubectl apply -f nginx-rc.yaml
kubectl scale --replicas=5 rs/nginx-replica
```

## Perform Rolling Update

```
kubectl create -f nginx-deployment.yaml --record
kubectl rollout status deployment nginx-deployment
kubectl rollout history deployment nginx-deployment

kubectl apply -f nginx-deployment-updated.yaml --record
kubectl rollout status deployment nginx-deployment
kubectl rollout undo deployment nginx-deployment --to-revision=1
kubectl rollout status deployment nginx-deployment
```

