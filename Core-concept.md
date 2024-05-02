# Kubernetes (K8s)

[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/569/badge)](https://bestpractices.coreinfrastructure.org/projects/569) [![Go Report Card](https://goreportcard.com/badge/github.com/kubernetes/kubernetes)](https://goreportcard.com/report/github.com/kubernetes/kubernetes) ![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/kubernetes/kubernetes?sort=semver)

<img src="https://github.com/kubernetes/kubernetes/raw/master/logo/logo.png" width="100">

----

Kubernetes, also known as K8s, is an open source system for managing [containerized applications]
across multiple hosts. It provides basic mechanisms for the deployment, maintenance,
and scaling of applications.

Kubernetes builds upon a decade and a half of experience at Google running
production workloads at scale using a system called [Borg],
combined with best-of-breed ideas and practices from the community.

Kubernetes is hosted by the Cloud Native Computing Foundation ([CNCF]).
If your company wants to help shape the evolution of
technologies that are container-packaged, dynamically scheduled,
and microservices-oriented, consider joining the CNCF.
For details about who's involved and how Kubernetes plays a role,
read the CNCF [announcement].

----

## Kubernetes Cluster Architecture


A Kubernetes cluster is the core unit for deploying and managing containerized applications. 
It groups a set of machines (nodes) and software components (control plane) that work together to automate container deployment, scaling, and networking. Here's a breakdown of the key elements:

### 1. Nodes (Worker Machines):

The workhorses of the cluster, nodes are machines (physical or virtual) that run containerized applications.
Each node has a container runtime engine (like Docker) to manage containers and a kubelet agent for communication with the control plane.
Pods, the smallest deployable units in Kubernetes, are scheduled and executed on these nodes.
### 2. Control Plane:

The brain of the cluster, the control plane manages the worker nodes and pods.
It consists of several components, each handling specific tasks:
*API Server:* Acts as the central point for all communication with the cluster.
*Scheduler:* Assigns pods to appropriate nodes based on defined criteria.
*Controller Manager:* Manages the lifecycle of pods, deployments, and other Kubernetes objects.
*etcd:* A highly available key-value store that stores cluster state.

## Communication Flow:

1. You interact with the API server to deploy containerized applications (pods) or manage cluster resources.
2. The API server processes your requests and communicates with other control plane components.
3. The scheduler selects a suitable node for each pod based on resource availability and other constraints.
4. The API server instructs the kubelet agent on the chosen node to pull container images and start the pod.
5. The kubelet manages the pod's lifecycle, ensuring containers within the pod run as intended.
6. The control plane components continuously monitor the cluster's health and take corrective actions if needed (e.g., restarting failed pods).

!["Kubernets Architecture"](https://miro.medium.com/v2/resize:fit:1024/1*NbfqFvRvZnofYn2-T4SAuw.png "Kubernets Architecture")
## Basics objects of Kuberntes:

!["Kubernets Objects"](https://support.huaweicloud.com/intl/en-us/basics-cce/en-us_image_0258869759.png "Kubernets Architecture")
### Pod
A pod is the smallest and simplest unit that you create or deploy in Kubernetes. A pod encapsulates one or more containers, storage resources, a unique network IP address, and options that govern how the containers should run.

### Deployment
A Deployment can be viewed as an application encapsulating pods. It can contain one or more pods. Each pod has the same role, and the system automatically distributes requests to the pods of a Deployment.

### StatefulSet
A StatefulSet is used to manage stateful applications. Like Deployments, StatefulSets manage a group of pods based on an identical container spec. Where they differ is that StatefulSets maintain a fixed ID for each of their pods. These pods are created based on the same declaration but cannot replace each other. Each pod has a permanent ID regardless of how it is scheduled.

### Job
A job is used to control batch tasks. Jobs are different from long-term servo tasks (such as Deployments). The former can be started and terminated at specific time, while the latter runs unceasingly unless it is terminated. Pods managed by a job will be automatically removed after successfully completing tasks based on user configurations.

### Cron job
A cron job is a time-based job. Similar to the crontab of the Linux system, it runs a specified job in a specified time range.

### DaemonSet
A DaemonSet runs a pod on each node in a cluster and ensures that there is only one pod. This works well for certain system-level applications, such as log collection and resource monitoring, since they must run on each node and need only a few pods. A good example is kube-proxy.

### Service
A Service is used for pod access. With a fixed IP address, a Service forwards access traffic to pods and performs load balancing for these pods.

### Ingress
Services forward requests based on Layer 4 TCP and UDP protocols. Ingresses can forward requests based on Layer 7 HTTPS and HTTPS protocols and make forwarding more targeted by domain names and paths.

### ConfigMap
A ConfigMap stores configuration information in key-value pairs required by applications. With a ConfigMap, you can easily decouple configurations and use different configurations in different environments.

### Secret
A secret lets you store and manage sensitive information, such as password, authentication information, certificates, and private keys. Storing confidential information in a secret is safer and more flexible than putting it verbatim in a pod definition or in a container image.

### PersistentVolume (PV)
A PV describes a persistent data storage volume. It defines a directory for persistent storage on a host machine, for example, a mount directory of a network file system (NFS).

### PersistentVolumeClaim (PVC)
Kubernetes provides PVCs to apply for persistent storage. With PVCs, you only need to specify the type and capacity of storage without concerning about how to create and release underlying storage resources.