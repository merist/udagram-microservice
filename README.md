# Udagram Microservice

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

## Requirements

You need to have already installed:

[Docker](https://docs.docker.com/docker-for-windows/install/)

[Docker desktop](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

[Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

[Kitematic](https://docs.docker.com/toolbox/) (optional - graphical user interface (GUI) for running Docker containers).
You will also need an account in docker hub to publish your images.

## Setup Docker

First check that docker is installed running the following command:

`docker --version`

Create all the docker images for every service:

`docker build -t meristatha/udacity-restapi-user .`

`docker build -t meristatha/udacity-restapi-feed .`

`docker build -t meristatha/udacity-frontend .`

`docker build -t meristatha/reverseproxy .`

To check that the images are already created:

`docker images`

Before pushing your images to your docker hub you need to login:

`docker login`

and then set your credentials.

To publish your images in your docker hub repository run:

`docker push meristatha/udacity-restapi-feed`

`docker push meristatha/udacity-restapi-user`

`docker push meristatha/udacity-frontend`

`docker push meristatha/reverseproxy`

Check your repository that the images were published successfully.
To list all the running containers run the followring command:

`docker container ls` (or `docker ps`)

Then build all the images:

`docker-compose -f docker-compose-build.yaml build --parallel`

To run all your docker containers:

`docker-compose up`

To stop and remove all the containers run:

`docker-compose stop`

## Enable kubernetes on Docker desktop

Go to docker settings in Docker desktop, select Kubernetes section and click **Enable Kubernetes**

## Create Kubernetes configurations

Create the secret base64 credentials:

`kubectl create secret generic db-user-pass --from-file=./aws_access_key_id.txt --from-file=./aws_secret_access_key.txt`

Load secret files:

`kubectl apply -f aws-secret.yaml`

`kubectl apply -f env-configmap.yaml`

`kubectl apply -f env-secret.yaml`

Apply all the service files:

`kubectl apply -f backend-feed-service.yaml`

`kubectl apply -f backend-user-service.yaml`

`kubectl apply -f frontend-service.yaml`

`kubectl apply -f reverseproxy-service.yaml`

Apply all the deployment files:

`kubectl apply -f backend-feed-deployment.yaml`

`kubectl apply -f backend-user-deployment.yaml`

`kubectl apply -f frontend-deployment.yaml`

`kubectl apply -f reverseproxy-deployment.yaml`

`kubectl apply -f pod-example/pod.yaml`

Check the pods status:

`kubectl get pod` (for more detailed status information run `kubectl get pod -o wide`)

Check the running services:

`kubectl get service`

Port forwarding commands:

`kubectl port-forward service/frontend 8100:8100`

`kubectl port-forward service/reverseproxy 8080:8080`
