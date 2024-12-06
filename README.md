# Cosmocloud Helm Chart

## Overview

This Helm chart is designed to deploy a simple web application stack on Kubernetes,
and it was part of my assignment from **cosmocloud** which consists of:

- **Backend**: A backend application running on a custom image (`shreybatra/sample-backend`).
- **Frontend**: A frontend application running on another custom image (`shreybatra/sample-frontend`).
- **Redis**: A Redis database to manage data persistence.

The main purpose of this project is to deploy and manage these services using basics of Kubernetes and Helm, ensuring they work together smoothly.

## Prerequisites

Before we begin,we must check the prerequistes or ensure we have the following tools installed:

### Kubernetes CLI (kubectl)

This is the command-line tool for interacting with Kubernetes clusters. You can install it by following the official guide present on their website just google *kubectl* and first link appear is the official website of **kubernetes.io**.

To Verify it is working just type :

*kubectl version --client*


### Helm

Helm is the Kubernetes package manager. You can install it by following the official instructions provided on the official website **https://helm.sh/**.

Verify the installation with:

*helm version*


### Kubernetes Cluster
There are two ways by which we can create cluster : *minikube* or *kind*.
It will create a local cluster.
You need a Kubernetes cluster running. 
 
let's see how we can create local cluster:

`minikube`:

*minikube start*


`kind`:

*kind create cluster*
 

we can also use `kind` to create custom name for cluster with using:
     *kind create cluster --name <my.cluster>*

Check that your cluster is up and running with:

*kubectl get nodes*



**Those who are using windows to push the repository to github can skip this part.**


**This Part is for linux users who want to commit repository from linux os itself**


### Git

You need Git to clone or push your repository to GitHub:

*sudo apt install git*

## Installation

To install the application stack, run the following Helm command:

*helm install testapp ./cosmocloud-deploy --atomic --timeout 30s*

Here some of you will get errors because of the timeout settings.
It will show that the application cannot be deployed in 30s so, we 
have to change the time accordingly.In my case I use 120s to test 
and deploy the app.

This command will install the application using Helm. It includes all 
the backend, frontend, and Redis services defined in the Helm chart.

## Helm Chart Structure

Here’s the file structure of the Helm chart:


```
cosmocloud-deploy/
  Chart.yaml           # Helm chart metadata
  values.yaml          # Default values for the application
  templates/
    deployment-backend.yaml   # Kubernetes Deployment for the backend
    deployment-frontend.yaml  # Kubernetes Deployment for the frontend
    deployment-redis.yaml     # Kubernetes Deployment for Redis
    service-backend.yaml      # Kubernetes Service for the backend
    service-frontend.yaml     # Kubernetes Service for the frontend
    service-redis.yaml        # Kubernetes Service for Redis
```

it should be in the same structure fromat, if you want to change them then change accordingly and the yaml files also and executable path too.

### Templates

The `templates/` folder contains YAML files that define the Kubernetes resources:

- **deployment-backend.yaml**: Defines the backend deployment and its environment variables (e.g., connecting to Redis).
- **deployment-frontend.yaml**: Defines the frontend deployment, setting the backend URL as an environment variable.
- **deployment-redis.yaml**: Defines the Redis database deployment.
- **service-backend.yaml**: Exposes the backend as a Kubernetes service (ClusterIP).
- **service-frontend.yaml**: Exposes the frontend as a NodePort service, making it accessible externally.
- **service-redis.yaml**: Exposes Redis as a ClusterIP service, accessible only inside the cluster.

## Testing the Deployment

Once you've installed the application using the Helm command above, you can verify the services are running correctly.

To check the status of the deployments and services, run:
```
kubectl get pods,services
```

If everything is working correctly, the backend, frontend, and Redis pods should be up and running.

You can test the frontend by accessing it in your browser using the NodePort assigned to the frontend service (e.g., `http://<NodeIP>:31000`).

## Uninstalling the Application

To uninstall the application stack, simply run the following Helm command:

 *helm uninstall testapp*

This will remove the deployed resources from your Kubernetes cluster.

## Conclusion

With this Helm chart, you can easily deploy a basic application stack consisting of a frontend, backend, and Redis database on Kubernetes. The chart is designed to be flexible, allowing you to adjust configurations as needed for your environment.

Feel free to explore, modify, and extend this chart to fit your needs. If you have any questions or suggestions, feel free to open an issue in the GitHub repository.
