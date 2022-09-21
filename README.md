# Minikube configuration

```minikube service -n default gateway-service```
## Supprimer TOUT
```minikube delete --all```

## Deploy the cluster locally

1. Start minikube: ```minikube start```
2. Enable Ingress: ```minikube addons enable ingress```
3. Apply all configuration files: ```kubectl apply -R -f .```
4. Create tunnel to access Ingress service: ```minikube tunnel```

## KONG
1 . kubectl create -f https://bit.ly/k4k8s
## Require Helm 3
2 . helm install kong/kong --generate-name --set ingressController.installCRDs=false

You can also use ```minikube dashboard```  
>The stateful set takes a few minutes to initialise.  
>I think we can say there is an error if after 10mn some stateful set pods are still red.

> How to force minikube to pull a new version of an image:
>- Delete the deployment associated with the image to terminate all pods using the image.
>- Run ```minikube image unload <image name>``` to remove the cached image.
>- Re-apply the deployment.

## Kafka - Zookeeper usage in terminal

1. Foward port: ```kubectl port-forward <kafka-pod-id> 9092```
2. Add this line in /etc/hosts: ```127.0.0.1 kafka-broker```
2. Produce message: ```echo "hello goodfood!" | kafkacat -P -b localhost:9092 -t test```
3. Consume topic messages: ```kafkacat -C -b localhost:9092 -t test```


## Replicated database
There are 2 database instances: one primary and one replica.  
The replica continuously synchronise its data with the primary.  
For write requests, use the primary database (using a headless service).  
For read requests, use any database instance (primary or replica).

## Current state of the cluster

### Entry point
The entrypoint of the cluster is: localhost (the default port is 80).  

### Services
The services that are deployed are:
- Order (Replicated database: 1 primary, 1 replica, all requests on primary)
- Cart (Single instance database)
- Authorization Server (Replicated database: 1 primary, 1 replica, requests are split)
- Kafka + Zookeeper

## Sources
- https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/  
- https://mariadb.org/mariadb-k8s-how-to-replicate-mariadb-in-k8s/
- https://github.com/MariaDB/mariadb.org-tools/tree/master/anel/k8s/mariadb
- https://kubernetes.io/docs/tutorials/configuration/configure-redis-using-configmap/