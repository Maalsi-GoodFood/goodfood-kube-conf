# Minikube configuration

## Deploy the cluster locally

1. Start minikube on an admin terminal: ```minikube start```
2. Enable Ingress: ```minikube addons enable ingress```
3. Apply all configuration files: ```kubectl apply -R -f .```
4. Create tunnel to access Ingress service: ```minikube tunnel```

You can also use ```minikube dashboard```  
>The stateful set takes a few minutes to initialise.  
>I think we can say there is an error if after 10mn some stateful set pods are still red.

>The oder app pod will restart a few times because the database is not ready yet, it is not a problem.

## Replicated database
There are 3 database instances: one primary and two replicas.  
The replicas continuously synchronise their data with the primary.  
For write requests, use the primary database (using a headless service).  
For read requests, use any database instance (primary or replicas).


## Current state of the cluster

### Entry point
The entrypoint of the cluster is: localhost (the default port is 80).  
All requests are passed to the order service.

### Order
#### App
The order app is deployed on a single instance.

#### Database
The order app uses the primary database for all database queries.

## Sources
https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/  
https://mariadb.org/mariadb-k8s-how-to-replicate-mariadb-in-k8s/
https://github.com/MariaDB/mariadb.org-tools/tree/master/anel/k8s/mariadb