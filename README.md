# k8s-mongo-express
Sample K8s configuration files for a simple mongo express setup, for reference purposes only.

## Execution order

1. Mongo Secret.
1. Mongo ConfigMap.
1. Mongo deployment and service.
1. Mongo Express deployment.

```shell
kubectl apply -f mongo-configmap.yaml

kubectl apply -f mongo-secret.yaml

kubectl apply -f mongo-express-secret.yaml

kubectl apply -f mongo.yaml

kubectl apply -f mongo-express.yaml

```

## Additional details

For secrets, get base64 encoded strings using:
```shell
echo -n "my string" | basse64
```
