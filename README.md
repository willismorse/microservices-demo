# This is a forked copy of WeaveWorks' microservices-demo

The original [microservices-demo Github repo](https://github.com/microservices-demo/microservices-demo) provides configurations to deploy a collection of microservices that together present a company catalog site for a fictional company called "The Sock Shop". 

The configurations in this repo simply pull pre-built container images in order to deploy. However, the source that produces these images are available as [sibling GitHub repos](https://github.com/microservices-demo). 

The original demo includes a monolithic web app built using static HTML and JQuery dynamic glue. This fork will be used to add deployment configurations for an additional front end repo that demonstrates a web site built from a collection of microfrontend services. 

This fork also adds a some minor changes that allow Sock Shop to run under a local Minikube installation (currently, this is just a modification to `deploy/kubernetes/manifests/front-end-svc.yaml` that opens an external endpoint in a way that is supported by Minikube).

## Tool Prerequisites
[Minikube](https://github.com/kubernetes/minikube) is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.

Here are the tooling prerequisites to get started:


### **Virtual machine**
Minikube requires a virtual machine in which to execute. Virtualbox seems to be the generally agreed-upon standard these days. There are alternatives, such as VMWare Fusion on Mac, but they don't seem to behave as well. Install Virtualbox via Homebrew:

```
brew cask install virtualbox
```

Alternatively, download the full binary installer with admin GUI from Oracle: 
https://www.virtualbox.org/wiki/Downloads

### **Minikube**
Bundles a kubernetes implementation with additional admin tools, including a dashboard. Install using Homebrew:
```
brew cask install minikube
```



### **kubectl** command
[`kubectl`](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-homebrew-on-macos) provides important commandline admin tools for kubernates:

```
brew install kubectl
```

### **kube-shell** command (optional)
[`kube-shell`](https://github.com/cloudnativelabs/kube-shell) provides a commandline Python tool that wraps `kubectl` and provides autocompletion and other conveniences. Currently it can only be installed using `pip`, so it's a little messy to set up, but Homebrew support is [a reported issue](https://github.com/cloudnativelabs/kube-shell/issues/45) to track.

### Helm
[Helm](https://github.com/kubernetes/helm) is a tool that provides higher level design abstractions on top of Kubernates. With Helm, you define your cluster's container and infrastructure architecture by writing simple templatized yaml config files (aka - "Charts"). 

Helm is a build tool that runs locally on your development machine or as a CI process. Helm uses your charts to generate all the requisite Kubernates deployment files. Once Helm has generated these files, you can choose to run these files immediately on your cluster, or package them up in a standardized tar format to serve as sharable deployment artifacts.

Install Helm in your liocal dev machine:

```
brew install kubernetes-helm
```

Helm includes an additional tool called Tiller, a server-side component that runs inside your cluster to aid in deployment. Once you have Helm installed on your local machine, you can use it to install Tiller into your cluster. (Note that the Tiller installation will discover your cluster by reading your kubectl config; in the case of local Minikube-based development this is most likely what you want):

```
helm init
```




## To start Minikube
Once you have all the prerequisite tools set up, you can fire up Minikube for the first time using the following command:

```
# The FIRST time you start minikube, you will bake the memory size into the virtualbox image forevermore. 
# Subsequent calls to start with a different memory parameter will ignore your request for more memory.
# Minikube's default memory is around 2GB, but you will need upwards of 8GB to reliably run the Sock Shop demo cluster.

minikube start --memory=8096

# If you forget to set enough memory during first start, you will have to delete the entire cluster and run start again (along with all subsequent kubernates cluster setup):

minikube delete
minikube start --memory=8096
```

### **Enable Ingress in Minikube**
Kubernetes Ingress is an optional feauture in Minkiube, so you must explicitly enable it:
```
minikube addons enable ingress
```

## To install Sock Shop in Minikube
Once you have Minikub installed and running, you can run the kubernates scripts to pull containers and set up the cluster:

```
cd <microservices-demo project folder>
kubectl create -f deploy/kubernetes/manifests/sock-shop-ns.yaml -f deploy/kubernetes/manifests
```

## To monitor the load process
Open the Minikube dashboard:

```
minikube dashboard
```

Choose `sock-shop` from the Namespaces dropdown and press the Overview link. You will see graphs and lists for all Sock Shop cluster resources.


## To view The Sock Shop monolithic frontend
Once all Deployements and Services are all running at 100% and green, browse to 

```
http://192.168.99.100:30656
```

<br><br><br><br>

# Original microservices-demo readme


# Sock Shop : A Microservice Demo Application

The application is the user-facing part of an online shop that sells socks. It is intended to aid the demonstration and testing of microservice and cloud native technologies.

It is built using [Spring Boot](http://projects.spring.io/spring-boot/), [Go kit](http://gokit.io) and [Node.js](https://nodejs.org/) and is packaged in Docker containers.

You can read more about the [application design](./internal-docs/design.md).

## Deployment Platforms

The [deploy folder](./deploy/) contains scripts and instructions to provision the application onto your favourite platform. 

Please let us know if there is a platform that you would like to see supported.

## Bugs, Feature Requests and Contributing

We'd love to see community contributions. We like to keep it simple and use Github issues to track bugs and feature requests and pull requests to manage contributions. See the [contribution information](.github/CONTRIBUTING.md) for more information.

## Screenshot

![Sock Shop frontend](https://github.com/microservices-demo/microservices-demo.github.io/raw/master/assets/sockshop-frontend.png)

## Visualizing the application

Use [Weave Scope](http://weave.works/products/weave-scope/) or [Weave Cloud](http://cloud.weave.works/) to visualize the application once it's running in the selected [target platform](./deploy/).

![Sock Shop in Weave Scope](https://github.com/microservices-demo/microservices-demo.github.io/raw/master/assets/sockshop-scope.png)

## 
