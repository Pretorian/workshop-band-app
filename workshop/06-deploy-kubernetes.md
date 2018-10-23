# Exercise 6 - Deploy your LoopBack application to Kubernetes

In this step, you'll create a Docker container from your LoopBack application, build and push the container to DockerHub, create a Kubernetes deployment from that image and access your application.

## Docker-ize your application

The first step to deploying your application to Kubernetes is to container-ize it. Although there are multiple technologies for running your applications in containers, the most popular one is Docker. In addition, Docker provides DockerHub for free - a public registry for hosting your Docker-based applications.

To get started, first [create a DockerHub account](https://hub.docker.com/) and note your username. You'll also need to [install Docker CE](https://docs.docker.com/install/). Once you've completed this, proceed to the next step.

### Create a Dockerfile

Create a file named `Dockerfile` in the Loopback application directory:
```
FROM node:slim
ADD . /app
WORKDIR /app
RUN npm install
CMD npm start
```

This file tells Docker how to build your application container. It starts with a base image based on Node.js (node:slim). It then copies over the application files, installs the NPM dependencies and describes the command to start the app.

### Build your Docker Container

Run the following commands to build your Docker container and push it to DockerHub.

```
docker login
# Follow the prompts for logging in

docker build -t [your_docker_username]/band-app:v1 .
docker push [your_docker_username]/band-app:v1
```

### Run your LoopBack application locally

Run the following command to launch your Docker-ized LoopBack application on your local machine:

```
docker run -it -p 3000:3000 [your_docker_username]/band-app:v1
```

You can now launch a browser and access `localhost:3000/explorer` to see your running LoopBack app. This time, it's running as a Docker container. Once you've verified that your container is working, let's push it to your Kubernetes cluster.

### Create a Kubernetes Deployment Manifest

Create the following file (`k8s-deploy.yml`) in the LoopBack directory:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: bandapp-deployment
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: bandapp
    spec:
      containers:
      - name: bandapp
        image: <your_docker_username>/band-app:v1
        ports:
          - containerPort: 3000
---
kind: Service
apiVersion: v1
metadata:
  name: bandapp-service
spec:
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
      name: http
  type: NodePort
  selector:
    app: bandapp
```

Update `<your_docker_username>` with your Docker username.

This file describes two major Kubernetes resources - a deployment and service. A deployment tells Kubernetes to pull the container specified next to `image:` from DockerHub, and then starts it and verifies that the state of the application matches the parameters described. For example, since we chose 3 replicas Kubernetes will always ensure that there are 3 copies of our LoopBack application running. The second part of the file describes the service - a service exposes the Kubernetes application for external access. Namely, we are exposing the internal port 3000 on the external node port 32000. This is important for the next step where we learn how to access the application.

### Deploy your application

Run the following command to deploy the application to Kubernetes:

```
kubectl apply -f k8s-deploy.yml
```

> Note: In the previous exercise, you exported the `KUBECONFIG` env var. If you opened a new terminal, this variable may be lost, which prevents you from being able to access your cluster (or deploy to it). If this command failed, try verifying that the `KUBECONFIG` is set. Run `echo $KUBECONFIG`. If it's empty, re-run the steps from the previous exercise - namely the `ibmcloud cs cluster-config <cluster_name>` command.

### Access your LoopBack application in Kubernetes

Run `ibmcloud cs workers mycluster` and locate the `Public IP`. This IP is used to access the LoopBack application. Open a browser and navigate to `<Public_IP>:32000` to access your application. Note that we append the NodePort that we described in the deployment manifest above.

That's it! You've Docker-ized your application and deployed it to a running Kubernetes cluster in IBM Cloud. Note that these steps are almost identical to Kubernetes deployments running anywhere, whether it is on a different cloud or on local hardware. The one unique IBM-specific step is downloading the cluster-configuration for Kubernetes. Every cloud provider has their own process for downloading this config.

## Done!

Congratulations! You've completed the workshop. Let's overview what we've done:
- Created a LoopBack application to handle events for a band, all supported through CRUD APIs
- Explored the Open API Spec-based explorer for the LoopBack-generated API endpoints
- Created a Kubernetes cluster on IBM Cloud
- Deployed the LoopBack application to Kubernetes

### Optional steps

In the next two steps, we'll connect this LoopBack application to a real database on IBM Cloud and securely manage credentials using Kubernetes secrets. We'll also go through a standard iterative process of making changes and pushing them to Kubernetes.

**Next Step:** [Connect a Datasource](07-datasource.md)
