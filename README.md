# Multi container application

Make sure that minikube, kubectl, VirtualBox and skaffold are installed on your computer

## Development flow

Run app in Kubernetes cluster with minikube:

```bash
    minikube start
    minikube addons enable ingress
    kubectl create secret generic mongo-username --from-literal MONGO_USERNAME=shol
    kubectl create secret generic mongo-password --from-literal MONGO_PASSWORD=12345678
    kubectl create secret generic access-token-secret --from-literal ACCESS_TOKEN_SECRET=yourSecretKey
    kubectl create secret generic refresh-token-secret --from-literal REFRESH_TOKEN_SECRET=yourSecretKey
    kubectl create secret generic email-password --from-literal EMAIL_PASSWORD=yourEmailPassword
    kubectl apply -f k8s/database-persistent-volume.yml
    kubectl apply -f k8s/files-persistent-volume.yml
    skaffold dev --port-forward
    minikube ip
    echo "$(minikube ip) music-school.me" | sudo tee -a /etc/hosts
```

## Port forward

```
kubectl port-forward <mongo_pod_id> 27017:27017
```

## Restore access to an existing pv:

```bash
kubectl patch pv database-persistent-volume -p '{"spec":{"claimRef": null}}' && kubectl patch pv files-persistent-volume -p '{"spec":{"claimRef": null}}'
```

## Installing npm package into kubetnetes pod:

(During development process)

- Stop Skaffold;
- Navigate into client or server folder on host system and install the package locally;
- Start Skaffold.

Local folder "node_modules" is ignored by git and docker.

## MongoDB dump

```bash
kubectl get pods                // get <mongo_pod_id>
kubectl exec -it <mongo_pod_id> -- sh
mongodump --out music-school_dump --db music-school
exit                            // exit from mongo shell
kubectl cp <mongo_pod_id>:music-school_dump ~/Documents/music-school_dump
```

## Copy uploaded files from persistent server storage

```bash
kubectl cp <server_pod_id>:/app/uploads ~/Documents/minikubeUploads
```

## For production buid run:

```bash
skaffold run -p prod
```

### Request in development

**frontend** minikube_ip
**backend** minikube_ip/api/
**api-documentation** localhost:5000/api-docs
