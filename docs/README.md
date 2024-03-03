# What is qbo?

QBO is an AsyncAPI designed to streamline the deployment of [Kind](https://kind.sigs.k8s.io/) images using Docker container as Kubernetes nodes. It empowers users to effortlessly deploy, manage, and scale containerized applications using Kubernetes, eliminating the need for intricate infrastructure management. With Kubernetes Engine (QKE), users can unlock the full potential of [metal performance](https://en.wikipedia.org/wiki/Bare-metal_server), particularly beneficial for compute-intensive workloads demanding high performance and minimal latency, such as databases and AI/ML models.

QBO offers **QBO Kubernetes Engine Community Edition (QKE CE)** at no cost, compatible with Linux or Windows (WSL2), enabling local execution. Additionally, QBO provides a fully managed cloud solution through **QBO Kubernetes Engine Cloud (QKE Cloud)**.

# Technology

> Designed for AI and ML

Bare metal with the flexibility of the cloud, powered by qbo Kubernetes, excels in handling compute-intensive workloads demanding high performance and low latency, such as databases, AI/ML models, and various real-time applications. On the other hand, virtual machines (VMs) are well-suited for workloads that don't rely heavily on compute power and low latency, such as web servers, websites, and development environments. If your users' experience and your business success hinge on achieving high performance and low latency, leveraging bare metal for your Kubernetes clusters supported by qbo technology should be a strategic consideration.


> Pure containers

QBO operates as an AsyncAPI that oversees Docker-in-Docker (DinD) Kubernetes clusters. In this unique setup, the traditional notion of a Kubernetes node, typically associated with a virtual or physical machine, is redefined as a Docker container within the QBO framework. Containerd runs within Docker in the QBO environment, facilitating the deployment of an entire Kubernetes infrastructure as a self-contained process directly on the hardware.

> Metal Performance

This deployment method offers an instant boost in performance, leveraging direct access to all hardware resources, including CPU, memory, and disk. Tasks like cluster creation, deletion, starting, stopping, or scaling demonstrate remarkable speed compared to virtual machines. QBO sustains the efficiency of metal performance while retaining the benefits of resource isolation through pure container technology.

> Native Kernel Functions

Administering a single kernel for each host, encompassing the Kubernetes nodes, offers a distinct advantage in terms of security, network, and storage operations. All tasks can be seamlessly executed through a unified interface—the Linux Kernel. QBO harnesses these capabilities by utilizing native kernel features such as IPVS, netfilter, iproute2, and eBPF. Achieving observability and enhancing security becomes feasible by instrumenting a single kernel through eBPF.

> Unified AsyncAPI 

QBO operates as a proxy AsyncAPI, overseeing not just Kubernetes clusters but also various cloud components. It boasts a high-performance AsyncAPI capable of real-time reporting on the status of Kubernetes nodes (represented as Docker containers), pods, processes, and threads within the host. The data structure is formatted in JSON, and interactions are executed through commands.

> Real Time Communication System

Given the dynamic nature of operations and the multitude of data sources involved, websockets play a vital role in capturing real-time system states. QBO introduces the concept of 'mirrors' as focal points for websocket messages, enabling subscribers within the same 'mirror' to receive updates from relevant systems. Regardless of data origin — be it Kubernetes, Docker, Linux OS, threads, processes, registries, or authentication providers — QBO consolidates pertinent data for mirror subscribers, ensuring an accurate real-time system representation. For example, users monitoring Kubernetes pod operations through a 'mirror' can observe instantaneous updates. Similarly, actions such as pthread or process executions are immediately visible to relevant mirror subscribers. Thus, QBO facilitates real-time communication via a proxy AsyncAPI, catering to all cloud components.

# Kubernetes Engine

> Watch the video

[![Watch the video](https://i.ytimg.com/vi/s2pItFe8IwU/maxresdefault.jpg)](https://www.youtube.com/embed/s2pItFe8IwU?rel=0")


## Conformance

QBO Kubernetes aligns with the Cloud Native Computing Foundation (CNCF) standards, ensuring adherence to best practices in cloud-native computing. This conformance establishes a solid foundation for scalability, interoperability, and performance, making qbo Kubernetes a reliable choice for AI workloads.


[![CNCF Certified](img/certified-kubernetes-color.svg)](https://landscape.cncf.io/card-mode?category=certified-kubernetes-distribution,certified-kubernetes-hosted,certified-kubernetes-installer&grouping=category&selected=qbo)

Compatible with `Kind` images
[https://hub.docker.com/r/kindest/node/tags](https://hub.docker.com/r/kindest/node/tags)


## Features

|Feature            | CLOUD                            | CE  | Notes |
|-------------------|-------------------------------------|----------|--|
|Multi cluster management|X| X |
|Cluster scaling|X|X
|In-memory cache|X|
|SSL|X|
|Google Oauth2|X|
|Cloud Load Balancer|X|
|IP Network management|X|
|Service Accounts|X|
|Kubeconfig management|X|X
|Kubenetes API Integration|X|
|CNI kindnet|X|X|
|Persistent Storage|X|X|
|Gitlab Registry || X
|Docker Hub Registry|X|X
|Kind Kubernetes image support|X|X|
|Custom image support|X|X|
|Websockets API|X|X|
|CLI|X|X|
|Web interface|X|X|
|Web terminal|X|X|

<!-- ■ Real Time logs<br>
■ Neural Graphs<br> -->


## Commands
|Command            | Argument                            | Options  | Paraemeter | Admin | Example                                                         |    Description               | CLOUD | CE |
|-------------------|-------------------------------------|----------|------------|-------|-----------------------------------------------------------------|------------------------------|-|-|
|  qbo add cluster  |    char[64]                         | -i     |  char[64]    |  N    | qbo add cluster `alex` -i `hub.docker.com/kindest/node:v1.27.2` | Add cluster                  |X|X|
|                   |                                     | -n     |  unsigned    |  N    |                                                                 | Number of nodes              |X|X|
|                   |                                     | -d     |  char[128]   |  N    |                                                                 | Domain name                  |X|X|
| qbo delete cluster|    char[64] |          |                                    |  N    | qbo delete cluster `alex`                                       | Delete cluster               |X|X|
|                   |                                     | -A     |              |  N    | qbo delete cluster -A                                           | Delete all clusters          |X|X|
| qbo stop cluster  |    char[64] |          |                                    |  N    | qbo stop cluster `alex`                                         | Stop cluster                 |X|X|      
|                   |                                     | -A     |              |  N    | qbo stop cluser -A                                              | Stop all clusters            |X|X|
| qbo start cluster |    char[64] |          |                                    |  N    | qbo start cluster `alex`                                        | Start cluster                |X|X|
|                   |                                     | -A     |              |  N    | qbo start cluster -A                                            | Start all clusters           |X|X|
| qbo get nodes     |    char[64] |          |                                    |  N    | qbo get nodes `alex`                                            | Get nodes                    |X|X|
|                   |                                     | -A     |              |  N    | qbo get nodes -A                                                | Get all nodes                |X|X|
|                   |                                     | -w     |              |  N    | qbo get nodes -w `43706dd0`                                     | Watch nodes                  |X| |
| qbo get pods      |    char[64] |          |                                    |  N    | qbo get pods `alex`                                             | Get pods                     |X|X|
|                   |                                     | -A     |              |  N    | qbo get pods -A                                                 | Get all pods                 |X|X|
|                   |                                     | -w     |              |  N    | qbo get pods -w `43706dd0`                                      | Watch pods                   |X| |
| qbo get services  |    char[64] |          |                                    |  N    | qbo get svc `alex`                                              | Get cluster services         |X| |
|                   |                                     | -A     |              |  N    | qbo get svc -A                                                  | Get all services             |X| |
|                   |                                     | -w     |              |  N    | qbo get svc -w `43706dd0`                                       | Watch services               |X| |
| qbo get ipvs      |    char[64] |          |                                    |  N    | qbo get ipvs `alex`                                             | Get cluster load balancers   |X| |
|                   |                                     | -A     |              |  N    | qbo get ipvs -A                                                 | Get all load balancers       |X| |
| qbo get images    |    char[64] |          |                                    |  N    | qbo get images                                                  | Get node images              |X|X|
|                   |                                     | -A     |              |  N    | qbo get images -A                                               | Get all images               |X|X|
| qbo get users     |    char[64] |          |                                    |  N    | qbo get user `alex`                                             | Get user                     |X| |
|                   |                                     | -A     |              |  N    | qbo get users -A                                                | Get all users                |X| |
| qbo get cluster   |    char[64] |          |                                    |  N    | qbo get cluster `alex`                                          | Get cluster                  |X| |
|                   |                                     | -A     |              |  N    | qbo get cluster -A                                              | Get all clusters             |X|X|
|                   |                                     | -k     |              |  N    | qbo get cluster -k `alex`                                       | Get cluster kubeconfig       |X| |
| qbo get network   |    char[64] |          |                                    |  N    | qbo get net `alex`                                              | Get cluster network          |X|X|
|                   |                                     | -A     |              |  N    | qbo get net -A                                                  | Get all cluster networks     |X|X|
|                   |                                     | -H     |              |  Y    | qbo get net -H                                                  | Get host network             |X| |
| qbo add node      |    char[64] |          |                                    |  N    | qbo add node `alex`                                             | Add node to cluster          |X|X|
|                   |                                     | -n     | unsigned     |  N    | qbo add node `alex` -n `3`                                      | Add n nodes to cluster       |X|X|
| qbo delete node   |    char[64] |          |                                    |  N    | qbo del node `node-2b251a2c`                                    | Delete node                  |X|X|
| qbo start node    |    char[64] |          |                                    |  N    | qbo start node `node-2b251a2c`                                  | Start node                   |X|X|
|                   |                                     | -A     |              |  N    | qbo start node -A                                               | Start all nodes              |X|X|
| qbo stop node     |    char[64] |          |                                    |  N    | qbo stop node `node-2b251a2c.localhost`                         | Stop node                    |X|X|
| qbo add user      |    char[64] |          |                                    |  Y    | qbo add user `alex`                                             | Add user                     |X| | 
|                   |                                     | --admin|              |  Y    | qbo add user --admin `alex`                                     | Add admin user               |X| |
| qbo delete user   |    char[64] |          |                                    |  Y    | qbo del user `alex`                                             | Delete user                  |X| |
| qbo add network   |    char[64] |          |                                    |  Y    | qbo add net `136.25.15.102 136.25.15.103`                       | Add network                  |X| |
| qbo delete network|    char[64] |          |                                    |  Y    | qbo del net `136.25.15.102 136.25.15.103`                       | Delete network               |X| |
| qbo version       |    char[64] |          |                                    |  N    | qbo version                                                     | Get qbo version              |X| |



