# Google Cloud Fundamentals: Getting Started with GKE 

## Objectives

In this lab, you will learn how to perform the following tasks:

- Provision a Kubernetes cluster using Kubernetes Engine.

- Deploy and manage Docker containers using kubectl.

## Steps

### 1. Confirm that needed APIs are enabled

- Run the following command to list the APIs and services available to you in your current project:

```shell
gcloud services list --available
```
- Confirm form the list that the following APIs are available
    1. Kubernetes Engine API
    2. Container Registry API
- If either API is missing e.g Kubernetes Engine API, Run the following command to enable the Kubernetes Engine API API service in your current project:

```shell
gcloud services enable kubernetesengine.googleapis.com
```

### 2. Start a Kubernetes Engine cluster

- For convenience, set your zone in the shell enviroment

```shell
export MY_ZONE=us-central1-a
```

- Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

```shell
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```

```
It takes several minutes to create a cluster as
Kubernetes Engine provisions virtual machines for you.
```

- After the cluster is created, check your installed version of Kubernetes using the kubectl version command:

```shell
kubectl version
```

- The gcloud container clusters create command automatically authenticated kubectl for you.

- To view your running nodes, run the command to view your VM instances.

```shell
gcloud compute instances list --filter="zone:us-central1-a"
```

Your Kubernetes cluster is now ready for use.


### 3. Run and deploy a container

- launch a single instance of the nginx container.

```shell
kubectl create deploy nginx --image=nginx:1.17.10
```

- View the pod running the nginx container:

```shell
kubectl get pods
```

- Expose the nginx container to the Internet:

```shell
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

- View the new service:

```shell
kubectl get services
```

- Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.

- Scale up the number of pods running on your service:

```shell
kubectl scale deployment nginx --replicas 3
```

- Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.

- Confirm that Kubernetes has updated the number of pods:

```shell
kubectl get pods
```

- Confirm that your external IP address has not changed:

```shell
kubectl get services
```

- Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.
