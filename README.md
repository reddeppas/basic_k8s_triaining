
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
