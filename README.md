# Multi container application

Make sure that minikube, kubectl, VirtualBox and skaffold are installed on your computer

## Development flow

Run app in Kubernetes cluster with minikube:

```bash
    minikube start
    minikube addons enable ingress
    kubectl create secret generic mongo-username --from-literal MONGO_USERNAME=shol
    kubectl create secret generic mongo-password --from-literal MONGO_PASSWORD=12345678
    kubectl create secret generic jwt-private-key --from-literal JWT_PRIVATE_KEY=yourSecretKey
    kubectl apply -f k8s/database-persistent-volume.yml
    skaffold dev --port-forward
    minikube ip
```

## Port forward

```
kubectl port-forward <mongo_pod_id> 27017:27017
```

## Restore access to an existing pv:

```bash
kubectl patch pv database-persistent-volume -p '{"spec":{"claimRef": null}}'
```

## For production buid run:

```bash
skaffold run -p prod
```

### Request in development

**frontend** minikube_ip
**backend** minikube_ip/api/
