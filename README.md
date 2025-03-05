# k8s-mongo-express
Sample K8s configuration files for a simple mongo express setup, for reference purposes only.

## Execution order

1. Create namespace and switch.
1. Storage class and Persistent Volume.
1. Persistent Volume Claim.
1. Mongo ConfigMap.
1. Mongo Secret.
1. Mongo Express Secret.
1. Mongo deployment and service.
1. Mongo Express deployment and service.
1. Mongo Express ingress.

```shell
# Create namespace
kubectl create -f app-namespace.yaml

# Switch to namespace
kubens k8s-mongo-express

# Storage class and Persistent Volume.
kubectl apply -f docker-sc.yaml
kubectl apply -f mongo-pv.yaml

# Persistent Volume Claim.
kubectl apply -f mongo-pvc.yaml

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

# Mongo express Ingress
kubectl apply -f mongo-express-ingress.yaml
```

## Delete all

```shell
kubens k8s-mongo-express
kubectl delete all --all
kubectl delete -f mongo-pvc.yaml
kubectl delete -f mongo-pv.yaml
kubectl delete -f docker-sc.yaml
kubens default
kubectl delete -f app-namespace.yaml
```

## Additional details

### For secrets

For secrets, get base64 encoded strings using:
```shell
echo -n "my string" | base64
```

### Ingress controller setup

Ingress configured using nginx-ingress:
https://github.com/kubernetes/ingress-nginx

### Setup host for ingress

Identify the address for ingress:
```shell
> kubectl get ingress

NAME                               CLASS    HOSTS                           ADDRESS     PORTS   AGE
gr-samples-mongo-express-ingress   <none>   mongo-express.grayraccoon.com   localhost   80      30m
```

And configure hosts or dns based on generated address. i.e.
```shell
> tail /etc/hosts

127.0.0.1	localhost	mongo-express.grayraccoon.com
```

### SSL Generation

The following commands can be used to generate the key and the cert.

```shell
# Key
openssl genrsa -out ca.key 2048

# Cert
openssl req -x509 -new -nodes -key ca.key -sha256 \
    -subj "/CN=mongo-express.grayraccoon.com" -days 1024 -out ca.crt -extensions san \
    -config <(
echo '[req]';
echo 'distinguished_name=req';
echo '[san]';
echo 'subjectAltName=DNS:mongo-express.grayraccoon.com')

cat tls/ca.crt | base64
cat tls/ca.key | base64
```

Then, create the secret using generated files' contents.
