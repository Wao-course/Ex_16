# Training exercise 1 - Fundamentals

## Concepts 'n' parts

What is a *controlplane* and what does it do in a kubernetes context?
Describe and explain what each of these parts in do:

**`Control Plane in Kubernetes:`** The control plane in Kubernetes is responsible for managing the cluster's state, making global decisions about the cluster (for example, scheduling), and detecting and responding to cluster events. It does not run user containers or applications. Instead, it manages the various components that make up the Kubernetes cluster.

**`API Server:`** Acts as the front end for the Kubernetes control plane. It exposes the Kubernetes API, which allows users to interact with Kubernetes. It validates and configures data for the api objects which include pods, services, replication controllers, and others.

**`Scheduler:`** Assigns newly created pods to nodes. It considers factors such as individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, and inter-workload interference.

**`Controller Manager:`** Runs controller processes, which are responsible for regulating the state of the cluster. Examples include the replication controller, endpoints controller, namespace controller, and service accounts controller.

**`ETCD:`** Consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.

**`Kubelet:`** An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.

**`Container Runtime:`** The software that is responsible for running containers. Common runtimes include Docker, containerd, and CRI-O.

**`Kube-proxy:`** Maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

**`Load Balancer:`** Distributes network or application traffic across multiple servers. In Kubernetes, it can refer to a cloud provider's load balancer service or an Ingress controller.

What does the concept of a node encompass? 
Each of these aforementionened parts live on a node. Be specific, which and why?

>The concept of a node in Kubernetes encompasses a single worker machine in the Kubernetes cluster. Each node contains the necessary services to run pods, managed by the control plane components. Specifically, the mentioned parts ``(API Server, Scheduler, Controller Manager, ETCD, Kubelet, Container Runtime, and Kube-proxy)`` are components that reside on each node to enable the node's functionality within the Kubernetes cluster.

## Imperitive vs. Declarative


What does each of these mean?

>**`Imperative:`** Describes how to achieve a certain state. Commands are executed to make changes happen. For example, imperative commands directly manipulate the current state to achieve the desired state.
**`Declarative:`** Describes the desired state without specifying the steps to get there. Instead of giving commands on how to achieve the desired state, you declare what state you want the system to be in, and the system takes care of the rest.

Which of these two approaches encompass how k8s provisioning should be understood?
Can you actually do both?

>You can use both approaches in Kubernetes. However, the declarative approach aligns more closely with the core principles and workings of Kubernetes, making it the preferred method for managing clusters and applications.

## k8s vs k3s

From a highlevel perspective, what is the difference?

>**`Kubernetes`** (k8s) is a comprehensive container orchestration platform designed to automate deploying, scaling, and managing containerized applications.

>**`K3s`** is a lightweight distribution of Kubernetes designed for edge computing, IoT, CI/CD, and other scenarios where a lightweight Kubernetes distribution is preferable. It includes only the necessary components, making it easier to deploy and maintain in resource-constrained environments.

# Training exercise 2 - Cluster Installation

## Installation


First of all we need the _k3d_ binary installed. This utility creates a virtual
multinode cluster on our desktop. Enabling us to play with an entire cluster
__as if_!

Follow the installation guide here - applies to Linux & Macs: https://k3d.io/v5.4.9/
For Windows users follow: https://www.digestibledevops.com/devops/2021/03/24/k3d-on-windows.html


Very simple you run the following command that installs a 4 node cluster. 
- 1 Master
- 3 Worker nodes
You can choose differently and might be convinient since 3 nodes take up resources...

> k3d cluster create my-first-cluster --agents 3 --k3s-arg "--disable=traefik@server:0" --port 8080:80@loadbalancer --port 8443:443@loadbalancer

Do note that we disable the socalled _ingress_ service _traefik_ meaning direct
and proper handling of http/https is not at our disposable just yet.
Furthermore ports 8080 and 8443 are port forwarded to the cluster at the
internal port _80_ and _443_ respectively. Currently this is just a heads-up,
not something you need to think about, but you must know.

## Interacting with the cluster

### QOL

See the _kubectl Cheat Sheet_ -
https://kubernetes.io/docs/reference/kubectl/cheatsheet/ For bash and zsh user
you can enjoy autocompletion in the commandline, so quite a must. Follow the
guide and do... such that it works every time you start a terminal.

### The kubectl cli command

Trying some very basic stuff, just to get acquainted.
- Get a list of all nodes
- Get a list of all namespaces
- Get cluster information of the currenct context cluster (you are already at it
  ;), the point being that you can change it will at a later date.)

# Training exercise 3 - Your first app in cluster

At this point you have not been introduced to how images are run as containers,
never the less, we will try something simple. The task being run a _Hello
World_ html server. The k8s object used is called a _Deployment_, which is all you
need to know at this point.

## Installing/starting _Hello World_

The image you are to use is _nginxdemos/hello_ at docker hub. Url: https://hub.docker.com/r/nginxdemos/hello/

To do this from the CLI you need to use 
> kubectl create deployment ...

the detailed options are not shown, but for you to figure out. Do note that its
a _Deployment_, which is created!  Things you need to determine and thus how to
specify on the commandline:
- App name, what you wanna call it
- Port exposed, how to figure this out from the image and state at the commandline
- The image used for the container

Try the help from the commandline! The _cheatsheet_ mentioned earlier is also a
good place to look...

## Verification

Since the used approach is called a _Deployment_, do list the _Deployments_
using the kubectl CLI. How is this done?

Reading the list of deployments, how do you know which one is yours?!

Upon getting a specific deployment (commandline notation!?) you can inquire
about its configuration by adding `-o yaml`. Read the output in terminal and
find the following places:
- Deployment name
- Image used
- Number of replicas - What do you reckon it should be as this point?!

Finally _port-forward_ a local port to the deployment. The cli command is 
> kubectl port-forward ...
figure out the missing options...

## Removal

Delete the deployment using the CLI and verify that its actually deleted.


# Training exercise 4 - Getting started with OpenLens 

Having completed this exercise you will have GUI to access and inspect your cluster. 

## Installation 

Following the simple guidelines found here
https://gitlab.au.dk/swwao/course/-/wikis/tools - This includes installing the
plugin.

## Inspecting the cluster

Have a look around and familize yourselves with the application. Do note that
your _Deployments_ can be found in the section _Workloads_.

At this point the cluster only has 3 deployments, none of which are yours and
thus none you have any knowledge about, so in effect quite empty.

Lets install the _Deployment_ used in the prior exercise using the commandline.

### Port forwarding

Wait until the deployment from the last exercise has been started. Where did you
find it?!  Find its pods, click on one that's up and running. Scroll down and
find the part about _forward_ - Clicking at this point you get the same
functionality as on the commandline adding port forwarding. Do port forward and
verify that you have access to the container.

### Logs

Figuring out what's happeing in a pod is obviously very important. This can be
done on the commandline as well as using this GUI.

Try both :-)


### Entering the pods

Finally, some times entering a running pod to inspect it is just as
important. Again, try doing this both from the commandline as well as the GUI.



## Cleanup

Find the section handling port forwardings and remove the newly created and
finally removed the deployment using the GUI.
