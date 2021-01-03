# Multi container application
Make sure that minikube, kubectl, VirtualBox  and skaffold are installed on your computer

  ## Development flow
  Run app in Kubernetes cluster with minikube:
  ```bash
    minikube start
    minikube addons enable ingress
    kubectl create secret generic mongo-username --from-literal MONGO_USERNAME=shol
    kubectl create secret generic mongo-password --from-literal MONGO_PASSWORD=12345678
    skaffold dev
    minikube ip
  ```

  ## For production buid run:
  ```bash
  skaffold run -p prod
  ```

  ### Request in development
  **frontend** minikube_ip
  **backend** minikube_ip/api/
