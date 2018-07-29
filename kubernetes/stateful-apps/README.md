## Create StatefulSet mongo 

Create pods
```
kubectl apply -f mongo-simple.yaml
```

Create service for dns records
```
kubectl apply -f mongo-service.yaml
```

Manually initialize mongodb 
```
kubectl exec mongo-0 -it mongo
rs.initiate({_id:"rs0", members: [{"_id":1, "host":"mongo-0.mongo:27017"}]})
rs.add("mongo-1.mongo:27017");
rs.add("mongo-2.mongo:27017");
exit;
```