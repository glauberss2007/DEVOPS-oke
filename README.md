# devops-OKE

![image](https://user-images.githubusercontent.com/22028539/132143462-71e3d8b6-b3e3-4746-b618-45fa4a9f8530.png)

## Introduction

A Kubernetes cluster is a group of nodes. The nodes are the machines running applications. Each node can be a physical machine or a virtual machine. The node’s capacity (its number of CPUs and amount of memory) is defined when the node is created. A cluster comprises:

- one or more master nodes (for high availability, typically there will be a number of master nodes)
- one or more worker nodes (sometimes known as minions)

A Kubernetes cluster can be organized into namespaces to divide the cluster’s resources between multiple users. Initially, a cluster has the following namespaces:

- default, for resources with no other namespace
- kube-system, for resources created by the Kubernetes system
- kube-node-lease, for one lease object per node to help determine node availability
- kube-public, usually used for resources that have to be accessible across the cluster

### Objectives

- Create a Kubernetes cluster
- Deploy a sample app

### Prerequisites

- Policy: allower service OKE to manage all resourcer in tenancy
- Policy: Administrator group or some individual policies

## Create Kubernetes Cluster

1. Sign in to Oracle Cloud Infrastructure Console using your cloud tenant name, user name, and password.
2. From the OCI services menu, Click Kubernetes Clusters under Developer Services. There is no need to create any policies for OKE as all the policies are pre-configured.
3. Under List Scope, select your compartment.
4. Click Create Cluster. Choose Quick Create and click Launch Workflow.
5. Fill out the dialog box:
- Name: Provide a name (oke-cluster in this example)
- Compartment: Choose your compartment
- Choose Visibility Type: Public
- Shape: Choose a VM shape
- Number of Nodes: 1
6. Click Next and click Create Cluster.
We now have an OKE cluster with 1 node and virtual cloud network with all the necessary resources and configuration needed.

## Check OCI CLI in Cloud Shell
OCI Command Line comes preinstalled in Oracle Cloud Shell.
1. Check the installed version of OCI CLI.
Start up the Oracle Cloud Shell if it’s not already running. Enter command:
````
oci -v
````

to check the OCI CLI version, which should be 2.5.x or higher.

## Initialize your Environment
1. Switch to the OCI Console window and navigate to your cluster. In cluster detail window, scroll down and click Quick Start under Resources.

Follow the steps under the Quick Start section.

2. The Quick Start directions will direct you to copy and execute the commands in your local termina

## Deploy Nginx App on Cluster Using kubectl
1. Create nginx deployment with three replicas. Enter command:
````
kubectl run nginx  --image=nginx --port=80 --replicas=3
````

2. Get Kubernetes deployment. Enter command:
````
kubectl get deployments
````

3. Get Pods. Enter command:
````
kubectl get pods -o wide
````

4. Create a service to expose the application. The cluster is integrated with the OCI Cloud Controller Manager (CCM). As a result, creating a service of type --type=LoadBalancer will expose the pods to the Internet using an OCI Load Balancer. In the terminal, enter command:
````
kubectl expose deployment nginx --port=80 --type=LoadBalancer
````

5. Switch to the OCI Console window. From the OCI services menu, click Load Balancers under Networking. A new OCI LB should be getting provisioned (this is due to the command above).

6. Once the load balancer is active, click the load balancer name, and from the Load Balancer Information page, note down its IP address.

7. Open a new browser tab and enter URL http://<Load-Balancer-Public-IP> (http://129.213.76.26 in this example). The Nginx welcome screen should be displayed.

## Delete the Resources

Note- You can ignore the Delete the Resources section if you’re using Oracle’s free tenancy, otherwise deleting resources in your own tenancy is optional.

### Delete OKE Cluster

1. To navigate back to your OCI Console window, click Container Clusters (OKE) under Developer Services.

2. Navigate to your cluster. Click Delete Cluster and then click Delete in the confirmation window.

### Delete VCN

1. From the OCI services menu, click Virtual Cloud Networks under Networking. A list of all VCNs will appear.

2. Locate your VCN, click the Action icon, and then click Terminate. Click Delete All in the confirmation window. Click Close once the VCN is deleted.

### Delete API Key

1. To navigate to user settings, click the Profile icon in the top right corner of the window. Then, select User Settings.

2. Scroll down to select API Keys under the Resources section.

3. Click on the Action icon and click Delete to delete the API key.

### Add your OKE in RANCHER

TODO based on 

https://rancher.com/docs/rancher/v2.x/en/cluster-provisioning/imported-clusters/

https://rancher.com/docs/rancher/v2.x/en/cluster-provisioning/registered-clusters/

## References:

https://docs.oracle.com/en/learn/container_engine_kubernetes/#acknowledgements

https://cloud-blogs.com/index.php/oracle-cloud/oracle-cloud-iaas/comprehensive-blog-on-oracle-kubernetes-engine-getting-started/01-getting-started-with-oracle-kubernetes-engine/


