_# Minikube configuration

## Deploy the cluster locally

1. Start minikube: ```minikube start```
2. Enable Ingress: ```minikube addons enable ingress```
3. Apply all configuration files: ```kubectl apply -R -f .```
4. Create tunnel to access Ingress service: ```minikube tunnel```

You can also use ```minikube dashboard```  
>The stateful set takes a few minutes to initialise.  
>I think we can say there is an error if after 10mn some stateful set pods are still red.

>The oder app pod will restart a few times because the database is not ready yet, it is not a problem.

## Kafka - Zookeeper usage in terminal

1. Foward port: ```kubectl port-forward <kafka-pod-id> 9092```
2. Produce message: ```echo "hello goodfood!" | kafkacat -P -b localhost:9092 -t test```
3. Consume topic messages: ```kafkacat -C -b localhost:9092 -t test```


## Replicated database
There are 3 database instances: one primary and two replicas.  
The replicas continuously synchronise their data with the primary.  
For write requests, use the primary database (using a headless service).  
For read requests, use any database instance (primary or replicas).


## Current state of the cluster

### Entry point
The entrypoint of the cluster is: localhost (the default port is 80).  
Requests with prefix /api/auth are passed to the authorization server.
All remaining requests are passed to the order service.

### Order
#### App
The order app is deployed on a single instance.

#### Database
The order app uses the primary database for all database queries.


### Authorization server
#### App
The authorization server is deployed on a single instance.

#### Database
The authorization server uses the primary for write queries (signup) and any instance for read queries (sign in, token validation).

## Sources
https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/  
https://mariadb.org/mariadb-k8s-how-to-replicate-mariadb-in-k8s/
https://github.com/MariaDB/mariadb.org-tools/tree/master/anel/k8s/mariadb