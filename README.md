# Minikube configuration

## Deploy the cluster locally

1. Start minikube on an admin terminal: ```minikube start```
2. Enable Ingress: ```minikube addons enable ingress```
3. Apply all configuration files: ```kubectl apply -f .```
4. Create tunnel to access Ingress service: ```minikube tunnel```

You can send request to the cluster at this address: localhost (the default port is 80).  
Currently, all requests are passed to order.