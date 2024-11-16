---
title: "Kubernetes Learning Journal"
date: 2024-11-17T00:22:51+08:00
draft: False
---
# Introduction
Came from survival instinct about job availability out there. I decided to learn new & required skills for the job markets out there. Kubernetes are required to create a resilience and scallable. If you look the projection based on The demand is reflected in the job market. According to Kube Careers' Q1 2024 report, 43% of Kubernetes-related job postings were for Software Engineers, with DevOps and Site Reliability Engineers also in high demand. The average salary for these roles ranged from $135,692 to $188,823, indicating strong compensation for professionals with Kubernetes skills. For me learn is to envisioning myself to get better compensation, like i said the world dynamic made me activated my survival instinct to keep relevant on this professional environment and learning is the key to keep relevant, event for learning something that not even linear with your career. Trust me learning is compounding process.

## What is kubernetes
Imagine a playground with lots of different toys (like swings, slides, and sandboxes). There’s someone in charge who makes sure each toy is in the right place, that kids don’t overcrowd one toy, and that broken toys get fixed. Kubernetes is like that supervisor, but instead of toys, it manages apps and makes sure they’re running well. If an app stops working, it fixes it; if too many people are using one app, it spreads people out to keep things fair.
### Why it used
Kubernetes exists because managing containerized applications in a distributed, scalable, and resilient way is inherently complex. It was created to address problems such as:

1. Managing the lifecycle of containers (starting, stopping, restarting).
2. Scaling applications dynamically based on demand.
3. Handling failures automatically without human intervention.
4. Abstracting infrastructure complexities to simplify application deployment.
5. Supporting the cloud-native, microservices-based application paradigm.

Kubernetes solves these problems by providing an orchestration layer that automates these processes, making it a foundational tool for modern application deployment and management. One of the notable reason why kubernetes exist are, previously managing container on different machine or nowadays we called it *distributed systems* are extremely hard, and kubernetes come in and *taraa* it solve it.

## Command

### Prerequisite
In here i use [minikube][https://minikube.sigs.k8s.io/docs/] to run kubernetes locally. Please visit the website and install it.

Good to have is create an alias for `minikube kubectl`
```bash
alias kubectl='minikube kubectl'
```

### Namespace
Kubernetes uses namespaces to organize objects in the cluster. You can
think of each namespace as a folder that holds a set of objects. to list all Pods in your cluster—you can pass the `--all-namespaces` flag.

### Context
You can view context as the configuration, so by default namespace use the default namespace, and if you want to change that u need to adjsut the conf file. This gets recorded in a kubectl configuration file, usually located at `$HOME/.kube/config`. This configuration file also stores how to both find and authenticate to your cluster. Now let's delve to the interesting part.

### Kubernetes Object
Each Kubernetes object exists at a unique HTTP path; for example,
https://your-k8s.com/api/v1/namespaces/default/pods/my-pod leads to the representation of a Pod in the default namespace named my-pod. The most basic thing u could do is `kubectl get <resource-name>` where it's also fetch using previous format. By default, kubectl uses a human-readable printer for viewing the responses from the API server, but this human-readable printer removes many of the details of the objects to fit each object on one terminal line. One way to get slightly more information is to add the -o wide flag, which gives more details, on a longer line. If you want to view the complete object, you can also view the objects as raw JSON or YAML using the -o json or -o yaml flags, respectively. Interestingly i also can retrieve multiple objects on different types by using comma i.e
```bash
kubectl get pods,services
```
and if you little bit curios about the objects, u can use
```bash
kubectl describe <resource-name> <obj-name>
```
This will provide a rich multiline human-readable description of the object as well as any other relevant, related objects and events in the Kubernetes cluster. 

If you would like to see a list of supported fields for each supported type of
Kubernetes object you can use the explain command.
```bash
kubectl explain pods
```
