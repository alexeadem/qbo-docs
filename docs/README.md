# Overview
# What is&nbsp;[qbo](glossary?id=-qbo-pronounced-letter-q-bow-as-in-bow-tie)?

QBO is an AsyncAPI designed to streamline the deployment of [Kind](https://kind.sigs.k8s.io/) images using Docker containers as Kubernetes nodes. It empowers users to effortlessly deploy, manage, and scale containerized applications using Kubernetes, eliminating the need for intricate infrastructure management. With Kubernetes Engine (QKE), users can unlock the full potential of [metal performance](https://en.wikipedia.org/wiki/Bare-metal_server), particularly beneficial for compute-intensive workloads demanding high performance and minimal latency, such as databases and AI/ML models.

QBO offers **QBO Kubernetes Engine Community Edition (QKE CE)** at no cost, compatible with Linux or Windows (WSL2), enabling local execution. Additionally, QBO provides a fully managed cloud solution through **QBO Kubernetes Engine Cloud (QKE Cloud)**.

# Technology

> Designed for AI and ML

Delivering bare metal with the flexibility of the cloud, qbo Kubernetes excels in handling compute-intensive workloads demanding high performance and low latency, such as databases, AI/ML models, and various real-time applications. If your user experience and business success hinge on achieving high performance and low latency, leveraging bare metal for your Kubernetes clusters supported by qbo technology should be a strategic consideration.


> Pure containers

QBO operates as an AsyncAPI that oversees Docker-in-Docker (DinD) Kubernetes clusters. In this unique setup, the traditional notion of a Kubernetes node, typically associated with a virtual or physical machine, is redefined as a Docker container within the QBO framework. Containerd runs within Docker in the QBO environment, facilitating the deployment of an entire Kubernetes infrastructure as a self-contained process directly on the hardware.

> Metal Performance

This deployment method offers an instant boost in performance, leveraging direct access to all hardware resources, including CPU, memory, and disk. Tasks like cluster creation, deletion, starting, stopping, or scaling demonstrate remarkable speed compared to virtual machines. QBO sustains the efficiency of metal performance while retaining the benefits of resource isolation through pure container technology.

> Native Kernel Functions

Administering a single kernel for each host, encompassing the Kubernetes nodes, offers a distinct advantage in terms of security, network, and storage operations. All tasks can be seamlessly executed through a unified interface: the Linux Kernel. QBO harnesses these capabilities by utilizing native kernel features such as IPVS, netfilter, iproute2, and eBPF. Achieving observability and enhancing security becomes feasible by instrumenting a single kernel through eBPF.

> Unified AsyncAPI 

QBO operates as a proxy AsyncAPI, overseeing not just Kubernetes clusters but also various cloud components. It boasts a high-performance AsyncAPI capable of real-time reporting on the status of Kubernetes nodes (represented as Docker containers), pods, processes, and threads within the host. The data structure is formatted in JSON, and interactions are executed through commands.

> Real Time Communication System

Given the dynamic nature of operations and the multitude of data sources involved, websockets play a vital role in capturing real-time system states. QBO introduces the concept of 'mirrors' as focal points for websocket messages, enabling subscribers within the same 'mirror' to receive updates from relevant systems. Regardless of data origin — be it Kubernetes, Docker, Linux OS, threads, processes, registries, or authentication providers — QBO consolidates pertinent data for mirror subscribers, ensuring an accurate real-time system representation. For example, users monitoring Kubernetes pod operations through a 'mirror' can observe instantaneous updates. Similarly, actions such as pthread or process executions are immediately visible to relevant mirror subscribers. Thus, QBO facilitates real-time communication via a proxy AsyncAPI, catering to all cloud components.

# Features

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
|Kubernetes Engine (QKE)|X|X|
|Container Engine (QCE)|X||