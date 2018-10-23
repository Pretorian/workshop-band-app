# Exercise 5 - Accessing a Kubernetes cluster with IBM Cloud Kubernetes Service

You must already have a [cluster created](https://console.bluemix.net/docs/containers/container_index.html#container_index). Here are the steps to access your cluster.

>Note: If your terminal is still running the LoopBack app from the previous step, you can open a new terminal or exit the LoopBack process (Ctrl/Cmd + C).

## Install IBM Cloud Kubernetes Service command line utilities

1. Install the IBM Cloud [command line interface](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started).

2.  Log in to the IBM Cloud CLI with your IBM ID.

```shell
ibmcloud login
```

If you have a federated account, include the `[--sso]` flag. If you have an api key, use the `--apikey` flag.

```shell
ibmcloud login [--sso]
```

3.  Install the IBM Cloud Kubernetes Service plug-in.

```shell
ibmcloud plugin install container-service -r Bluemix
```

4. To verify that the plug-in is installed properly, run `ibmcloud plugin list`. The Container Service plug-in is displayed in the results as `container-service`.

5.  Initialize the Container Service plug-in and point the endpoint to `us-south` region.

Example:

```shell
ibmcloud cs region-set
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 6
```

6. Install the Kubernetes CLI. Go to the [Kubernetes page](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-via-curl) to install the CLI and follow the steps.

## Access your cluster
Learn how to set the context to work with your cluster by using the `kubectl` CLI, access the Kubernetes dashboard, and gather basic information about your cluster.

1.  Set the context for your cluster in your CLI. Every time you log in to the IBM Cloud Kubernetes Service CLI to work with the cluster, you must run these commands to set the path to the cluster's configuration file as a session variable. The Kubernetes CLI uses this variable to find a local configuration file and certificates that are necessary to connect with the cluster in IBM Cloud.

>Note: If you feel comfortable modifying your `.bashrc` file (running a script every time you launch your terminal), you could save the `KUBECONFIG` export step explained below. This allows you to avoid having to run this command every time you launch a new terminal.

a. List the available clusters.

```shell
ibmcloud cs clusters
```

b. Download the configuration file and certificates for your cluster using the `cluster-config` command.

```shell
ibmcloud cs cluster-config <your_cluster_name>
```

c. Copy and paste the output command from the previous step. This will set the `KUBECONFIG` environment variable and configure your CLI to run `kubectl` commands against your cluster.

2.  Get basic information about your cluster and its worker nodes. This information can help you manage your cluster and troubleshoot issues.

a.  View details of your cluster.

```shell
ibmcloud cs cluster-get <your_cluster_name>
```

b.  Verify the worker nodes in the cluster.

```shell
ibmcloud cs workers <your_cluster_name>
ibmcloud cs worker-get <worker_ID>
```

3.  Validate access to your cluster.

a.  View nodes in the cluster.

```shell
kubectl get node
```

b.  View services, deployments, and pods.

```shell
kubectl get svc,deploy,po --all-namespaces
```

**Next Step:** [Deploy to Kubernetes](06-deploy-kubernetes.md)
