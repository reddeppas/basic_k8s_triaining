
## Kubernetes Basics

### What is a Container?
    A Container is a package of software that includes all dependencies:
      code,runtime,configuration and system libraries so that it can run on any host system.
### Building blocks of Containers
    Namespace,CGroup, and union file system

### Familiar Container Runtimes
    Docker , containerd, rkt, cri-o, lxc and runc

### excercise 1:

    Install Docker:
     https://docs.docker.com/get-docker/ 
    
    $ docker --version
      Docker version 20.10.6, build 370c289
    
    Install npm
      https://docs.npmjs.com/downloading-and-installing-node-js-and-npm
    
    $ node -v
      v14.14.0
    $ npm -v
      6.14.8
    Install express js
    $ npm install express --save

    Start node server
    $ node helloworld/server.js 
    Running on http://0.0.0.0:8080

    Open http://localhost:8080, and you'll see your hello world response
  
    Now , let us build the docker image:
    
    $docker build -t node-web-app .

    We can run the Docker container by:

    $ docker run --name hw_container -p 8080:8080 node-web-app
    
    Running on http://0.0.0.0:8080

    Let us remove the container

    $ docker rm -f hw_container

    
### What is Kubernetes? 
    Kubernetes also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

### Application Deployment
    Traditional Deployment:

      physical server dedicated to a single application and its libraries. Due to this resources are underutilzed and its expensive.
      
    Virtualized deployment:

      Virtualizations allows us to deploy multiple applications on single physical server by Virtualization. There will be multiple vms running along with dedicated operating system per vm and virtuazaltion hardware. We were able to utilize resources to an extent because of virtulzation.

    Container Deployment:
    
      Containers are similiar to vms, but they are isolated and share the same operating system among the applications. Therefore containers are lightweight.

      
     
### Create kubernetes cluster:
    
 We will use kind to create kubernetes cluster. To install kind follow the document https://kind.sigs.k8s.io/docs/user/quick-start/#installation

 Create a cluster
    $kind create cluster --name k8s_demo

  List clusters 
    $kind get clusters

  Deleting a cluster

    $kind delete cluster --name k8s_demo
    
 ### Helloworld in kubernetes
 Once the kind cluster up and running 
    $kubectl run hello-kube --image=node-web-app --port=80
 
 The run command runs the given container image inside a pod.
 
 Pods are like a box that encapsulates a container. To make sure the pod has been created and is running, execute the following command:
 
    $kubectl get pod
 
 Pods by default are inaccessible from outside the cluster. To make them accessible, you have to expose them using a service. So, once the pod is up and running, execute the following command to expose the pod:

    $kubectl expose pod hello-kube --type=LoadBalancer --port=80
    
 To make sure the load balancer service has been created successfully, execute the following command:
    
    $kubectl get service
    
    
### Kubernetes Architecture

A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. 
Every cluster has at least one worker node.

![image](https://user-images.githubusercontent.com/20655128/132990006-007c8f4c-e8a2-4c80-9d12-ba88f8f1791a.png)

Roles :
  Control plane : Makes most of the necessary decisions and acts as sort of the brains of the entire cluster. This can be a single server or a group of server in larger projects
  node: Responsible for running workloads. These servers are usually micro managed by the control plane and carries out various tasks following supplied instructions.
  
### control plane components

##### kube-apiserver: This acts as the entrance to the Kubernetes control plane, responsible for validating and processing requests delivered using client libraries like the                             kubectl program.
##### etcd: distributed key-value data store which acts as the single source of truth about your cluster.It holds configuration data and information about the state of the cluster
   
##### kube-scheduler: Assigning task to a certain node considering its available resources and the requirements of the task is known as scheduling. The kube-scheduler component                         does the task of scheduling in Kubernetes making sure none of the servers in the cluster is overloaded.
  
##### kube-controller manager: The controllers in Kubernetes are responsible for controlling the state of the cluster. When you let Kubernetes know what you want in your cluster,                                the controllers make sure that your request is fulfilled
    
##### cloud-controller manager: In a real world cloud environment, this component lets you wire-up your cluster with your cloud provider's (GKE/EKS) API. This way, the components                                 that interact with that cloud platform stays isolated from components that just interact with your cluster. 
     
### node components
  ##### kubelet: This service acts as the gateway between the control plain and each of the nodes in a cluster. Every instructions from the control plain towards the nodes, passes                  through this service. It also interacts with the etcd store to keep the state information updated.
    
  ##### kubeproxy: This small service runs on each node server and maintains network rules on them. Any network request that reaches a service inside your cluster, passes through                    this service.
    
  ##### container runtime: Kubernetes is a container orchestration tool hence it runs applications in containers. This means that every node needs to have a container runtime like                             Docker or rkt or cri-o.



  
 
