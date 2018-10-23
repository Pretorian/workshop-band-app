# Working with Credentials on Kubernetes with LoopBack

As outlined in the previous step: putting our username and password in our code to be committed to a public repository, makes our application vulnerable. Let's look at how we can alleviate this issue.

## Our approach

It is good practice to have your environments as consistent as possible. The reason being: when you promote your application from your local development environment to a staging environment and eventually to a production environment, if all the environments along that path are very similar and consistent, we shouldn't have any surprises or unexpected behavior introduced through our deployments. Therefore, we will use the same Cloudant DB instance for both local and production environments.

*Note: As we get further along in our development, we would create a database just for production data to keep development changes from introducing instability; but for now, we will just use the same data-source.*

### Overview

- Add the local credentials file to our ignore lists (do not commit to public repo)
- Configure our application to use the production credentials in prod and the local credentials during development

Before we go any further, let's duplicate our datasources configuration file, `server/datasources.json` and name it `server/datasources.local.json`. Make sure they are in the same directory. Then go back into the `server/datasources.json` and change the private credentials (cloudant.url) to `REPLACE_PRIVATE_CREDENTIALS` for future reference - this will serve as a template file for future users of this app.

Now, let's add the `server/datasources.local.json` file to the "git ignore" list to prevent committing the secure credentials to GitHub. LoopBack automatically creates a `.gitignore` file for you - update it to add the following at the bottom.

```
...
server/datasources.local.json
```

Also create a `.dockerignore` file with the same entry so that your Docker images don't store the credentials.

### Create Production Credentials

Duplicate the `datasources.local.json` file and rename it to `datasources.production.json`. In this file, replace the private credentials with ${CLOUDANT_URL}. This will be dynamically determined by LoopBack if the `CLOUDANT_URL` environment variable is set.

```
{
  "db": {
    "name": "db",
    "connector": "memory"
  },
  "cloudant": {
    "url": "${CLOUDANT_URL}",
    "database": "band-app",
    "username": "",
    "password": "",
    "name": "cloudant",
    "modelIndex": "",
    "connector": "cloudant"
  }
}
```

 You should have three datasource JSON files in the `server` directory now:
* `datasources.json` - a default template for future users, no private credentials
* `datasources.local.json` - creds for local development, holds private credentials that will not be Docker-ized or committed to GitHub
* `datasources.production.json` - creds that are only used when in production, with a reference to private credentials that are resolved dynamically. This file is committed to Docker images and GitHub.

Note that the `datasources.production.json` file is used only in production, which is determined by the `NODE_ENV=production` environment variable. If the environment variable is not set, the local config overrides the default config template file. You can learn more about [LoopBack config management](https://loopback.io/doc/en/lb3/Environment-specific-configuration.html#data-source-configuration) on the docs.

By the way, we do not need to put `datasources.production.json` in the `.gitignore` or `.dockerignore` files - there are no secret credentials in here.

### Push our changes to DockerHub

Now that we've made changes to our app, we need to push the new version to DockerHub. Note that we use `v2` this time:
```
docker build -t [your_docker_username]/band-app:v2 .
docker push [your_docker_username]/band-app:v2
```

## Create a Kubernetes secret

Kubernetes provides a mechanism for managing secure data such as database credentials with secrets. It's very easy to create a Kubernetes secret - let's create a secret with the credentials for our database now.

Ensure that `$KUBECONFIG` env var is still set before running the following command:

```
# Replace the URL with the one you have in your `datasources.local.json` file
# Note the use of single quotes
kubectl create secret generic cloudant-secret --from-literal=cloudanturl='<your_cloudant_credentials_url>'
```

You can run `kubectl describe secret cloudant-secret` to ensure it's set.

Next, let's set two environment variables on our Kubernetes deployment file, `k8s-deploy.yml`. We'll set `NODE_ENV` to `production` and `CLOUDANT_URL` to the value in the secret. We also need to update this `Deployment` to use the new band-app image on DockerHub. We'll update the reference to `v2`. Update your Kubernetes manifest yaml to the following:

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
          image: [your_docker_username]/band-app:v2
          ports:
            - containerPort: 3000
          env:
            - name: NODE_ENV
              value: production
            - name: CLOUDANT_URL
              valueFrom:
                secretKeyRef:
                  name: cloudant-secret
                  key: cloudanturl
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

If you copied the above directly, remember to update the image reference with your Docker username.

Deploy the updated app with `kubectl apply -f k8s-deploy.yml`. Access your app using the same URL `PUBLIC_IP:32000`. The Kubernetes-hosted app is now storing data in your Cloudant service, with the credentials being managed securely. In addition, your dev and prod versions share the same code but run with separate configuration, following a best-practice DevOps approach. 

## Done!

Congratulations! You've completed the workshop and optional steps. Let's overview what we've done:
- Created a LoopBack application to handle events
- Explored the Open API Spec-based explorer for the generated API endpoints
- Created a Kubernetes cluster on IBM Cloud
- Deployed the LoopBack application to Kubernetes
- Created the Cloudant service on IBM Cloud and connected the LoopBack application to it
- Took advantage LoopBack configuration to create effective prod and dev environments
- Used Kubernetes secrets to securely manage database credentials in the cloud