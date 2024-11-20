---
title: "Kubernetes Learning Journal (Pod)"
date: 2024-11-20T23:51:03+08:00
draft: false
---

Previously, on the previous article I already explain about the core concept of k8s right? Now let's deep dive to more advance topic. 

# Pods
Broadly speaking, Pod are very similiar with Docker container, the command, the concept (isolation, cli command, and so on) and i won't explain further about that and if you interested with the command you should visit [Docs][https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands]. If you ever known about docker-compose, in pod it's named manifest.

> Manifest & docker-compose are declarative configuration, on the other hand `docker run ...` and `kubectl run ...` are considered imperative.


## Resource Management
Maybe you wonder why so many people adore this tools, well u will find the answer in this section. 

The basic cost of operating a machine, either virtual or physical,
is basically constant regardless of whether it is idle or fully loaded. Consequently, ensuring that these machines are maximally active increases the efficiency of every dollar spent on infrastructure.

To measure the utilization, we use utilization metric. *Utilization* is defined as the amount of a resource that actively being used divided by the amount of a resource that has been purchased. For example, your boss give you a VM with 10 cores and the application u run only consume 1 core which mean you have 10% utilization. With scheduling systems from k8s managing resource packing, u could drive the utilization to more than 50%, And to achieve this u need to tell the kubernetes on the pod manifest, using *request* and *limit*. So let's look deeper to it.

### Resource Requests: Minimum Required Resources
When a Pod requests the resources required to run its containers.
Kubernetes guarantees that these resources are available to the Pod. The most commonly requested resources are CPU and memory, but Kubernetes supports other resource types as well, such as GPUs.

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: kuard
spec:
	containers:
		- image: gcr.io/kuar-demo/kuard-amd64:blue
		  name: kuard
		  resources:
			  requests:
				cpu: "500m"
				memory: "128Mi"
		  ports:
			- containerPort: 8080
			  name: http
			  protocol: TCP
```

To Apply it
> kubectl apply -f pod_manifest.yaml

As you can see inside resources section, there's requests right? And that is how we get 50% utilization of cpu usage, if the VM are 1 core.

Requests are used when scheduling pods to node. The kubernetes scheduler will ensure the request won't exceed the capacity of the node. Therefore, it's guaranteed to the pod run at least with the requested specification. Importantly, *requests* here act as a minimum specification in order to run the pod properly.

Imagine a container/pod whose code attempts to use all the available CPU. Kubernetes schedules this pod to have all of the CPU, but what if there's 2 pod on same node with *requests* too. Let says both requesting 1/2 Core and the VM itself only has 2 Core, each pod will get 1 core.

### Capping Resources with Limit
In addition to setting the resources required by a Pod, which establishes the minimum resources available to it, you can also set a maximum on a it’s resource usage via resource limits. In our previous example we created a kuard Pod that requested a minimum of 0.5 of a core and 128 MB of memory. In the previous Pod manifest, we extend this configuration to add a limit of 1.0 CPU and 256 MB of memory.

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: kuard
spec:
	containers:
		- image: gcr.io/kuar-demo/kuard-amd64:blue
		  name: kuard
		  resources:
			  requests:
				cpu: "500m"
				memory: "128Mi"
			  limits:
				cpu: "1000m"
				memory: "256Mi"			
		  ports:
			- containerPort: 8080
			  name: http
			  protocol: TCP
```

To Apply it
> kubectl apply -f pod_manifest.yaml




