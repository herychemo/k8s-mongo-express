# k8s-mongo-express
Sample K8s configuration files for a simple mongo express setup, for reference purposes only.

## Execution order

1. Mongo ConfigMap.
1. Mongo Secret.
1. Mongo Express Secret.
1. Mongo deployment and service.
1. Mongo Express deployment.
1. (Optional) Mongo Express external service.

```shell
# Mongo ConfigMap
kubectl apply -f mongo-configmap.yaml

# Mongo Secret
kubectl apply -f mongo-secret.yaml

# Mongo express Secret
kubectl apply -f mongo-express-secret.yaml

# Mongo Deployment and Service
kubectl apply -f mongo.yaml

# Mongo express Deployment
kubectl apply -f mongo-express.yaml

# Mongo express External Service
kubectl apply -f mongo-express-service.yaml
```

## Delete all

```shell
kubectl delete -f mongo-express-service.yaml >/dev/null 2>&1 | true
kubectl delete -f mongo-express.yaml >/dev/null 2>&1 | true
kubectl delete -f mongo.yaml >/dev/null 2>&1 | true
kubectl delete -f mongo-express-secret.yaml >/dev/null 2>&1 | true
kubectl delete -f mongo-secret.yaml >/dev/null 2>&1 | true
kubectl delete -f mongo-configmap.yaml >/dev/null 2>&1 | true
```

## Additional details

For secrets, get base64 encoded strings using:
```shell
echo -n "my string" | base64
```
