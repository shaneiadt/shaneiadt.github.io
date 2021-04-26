---
title: Setup Kubernetes on Windows 10
date: 2021-04-26 17:00:00
tags: ['Kubernetes', 'Docker']
---

![](/assets/images/Docker-And-Kubernetes/docker.png)
# What is Kubernetes?

Kubernetes is a system used for running and managing different containers over multiple machines / virtual machines. A Kubernetes cluster is made up of a master node that controls additional nodes and their containers.

# Why use Kubernetes?

Whenever we need to manage many different containers with many different images.

# Development and Production

In a development environment an application called **minikube** is used to spin up these Kubernete clusters on our local machine. On the other hand when it comes to a production setup we can take advantage of some managed solutions such as **Amazons Elastic Kubernetes Service** or **Google Kubernetes Service**.

# The Setup

The following outlines setting up Kubernetes via the **Docker Desktop** application.

1. Open Docker Desktop and navigate to the settings panel by clicking on the **gear icon**

![](../assets/images/Docker-And-Kubernetes/go-to-settings.png)

2. Under General **Enable WSL 2**

![](../assets/images/Docker-And-Kubernetes/enable-wsl-2.png)

3. Navigate to the **Kubernetes** panel and click the checkbox **Enable Kubernetes** then click **Apply & Restart**.

![](../assets/images/Docker-And-Kubernetes/enable-kubernetes.png)

4. Once installed you will need to restart Docker Desktop. Now open a command terminal and run `kubectl version` to ensure everything is installed correctly.

![](../assets/images/Docker-And-Kubernetes/kubectl-version.png)

Now we have Kubernetes installed hooray :smile: