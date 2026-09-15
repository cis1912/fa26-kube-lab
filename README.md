# Kube Demo

Time to toy around with Kubernetes!

Credit: This is based off of the demo created by the Fall 2024 CIS 1912 Staff

## Installation

[Install Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)

[Install Kubectl](https://kubernetes.io/docs/tasks/tools/)

## Cluster Bootstrap

First, let's create our `kind` cluster and configure `kubectl` to use it:

```
$ kind create cluster --name cis1912
$ kubectl config use-context kind-cis1912
```

## Lab

In this lab, we'll be using `kubectl` to create some resources and interact with them. We'll start with 2048. First, let's manually create a pod for the 2048 image:

```
$ kubectl run lab-2048 --image=ghcr.io/cis1912/2048 --port=80
```

Now use `kubectl get pods` to see our list of running pods. If the prior command worked, you should see the 2048 pod!

Now, use `kubectl describe pod lab-2048` to get more detailed information about our pod. Note the events on the pod - they show you information about what happened while the pod was spinning up.

### Kubernetes Actions

As you may have noticed, our Kubectl commands all follow the pattern `kubectl <verb> <resource_type> <resource_id>`. This syntax is not just a property of Kubectl; all Kubernetes actions have this structure! As we mentioned in lecture, this structure is enforced because Kubernetes is an API on which we need to enforce certain permissions. Enforcing that all actions take this form allows us to easily define which verb/resource/id combinations are legal and which aren't.

This form also makes it easy to extend Kubernetes, but that's a lecture topic for a few weeks from now.

### Connecting

While normally we port forward directly to a service, we can also forward to a pod, Port forward and go to `localhost:8080` to play 2048:

```
$ kubectl port-forward pod/lab-2048 8080:80
```

### Using a service

As we mentioned, applications are typically exposed through a service in Kubernetes. Now, figure out the appropriate `kubectl` command to expose 2048 and then port forward to the service. [The docs](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) might be helpful.

Note that you don't need to use `kubectl create service`. Kubectl provides you an easier command to use to expose the pod.

If you've exposed the application correctly, you should be able to port forward to the service and see your application:

```
$ kubectl port-forward svc/lab-2048 8080:80
```

You can use `kubectl delete` to delete individual resources in your cluster or `kind delete cluster --name cis1912` to delete the whole cluster.
