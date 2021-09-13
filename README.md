
## Kubernetes Basics

### What is a Container?
A Container is a package of software that includes all dependencies: code,runtime,configuration and system libraries so that it can run on any host system.

### Building blocks of Containers
Namespace,CGroup,and union file system

### Familiar Container Runtimes
Docker,containerd, rkt, cri-o, lxc and runc



##### Install Docker: 
https://docs.docker.com/get-docker/ 
    
     docker --version
    
    
##### Install npm
https://docs.npmjs.com/downloading-and-installing-node-js-and-npm
    
     node -v
      
     npm -v
      
      
##### Install express js

     npm install express --save

##### Start node server
 
     node helloworld/server.js 
    

Open http://localhost:8080, and you'll see your hello world response

Now , let us build the docker image:
    
     docker build -t reddeppas/node-web-app:latest .
     
Push image to docker hub or private registry:

     docker login
     
     docker push reddeppas/node-web-app:latest
     
We can run the Docker container by:

      docker run --name helloworld_container -p 8080:8080 node-web-app
    
Let us remove the container

     docker rm -f helloworld_container
    

    
### What is Kubernetes? 


Kubernetes also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

### Application Deployment

##### Traditional Deployment:

physical server dedicated to a single application and its libraries. Due to this resources are underutilzed and its expensive.
      
###### Virtualized deployment:

Virtualizations allows us to deploy multiple applications on single physical server by Virtualization. There will be multiple vms running along with dedicated operating system per vm and virtuazaltion hardware. We were able to utilize resources to an extent because of virtulzation.

##### Container Deployment:
    
Containers are similiar to vms, but they are isolated and share the same operating system among the applications. Therefore containers are lightweight.

      
     
    
### Kubernetes Architecture

A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. 
Every cluster has at least one worker node.

![image](https://user-images.githubusercontent.com/20655128/132990006-007c8f4c-e8a2-4c80-9d12-ba88f8f1791a.png)

##### Roles :

###### Control plane : 
Makes most of the necessary decisions and acts as sort of the brains of the entire cluster. This can be a single server or a group of server in larger projects

###### node: 
Responsible for running workloads. These servers are usually micro managed by the control plane and carries out various tasks following supplied instructions.
  
  
### control plane components

##### kube-apiserver: 
This acts as the entrance to the Kubernetes control plane, responsible for validating and processing requests delivered using client libraries like the                             kubectl program.
##### etcd: 
distributed key-value data store which acts as the single source of truth about your cluster.It holds configuration data and information about the state of the cluster
   
##### kube-scheduler: 
Assigning task to a certain node considering its available resources and the requirements of the task is known as scheduling. The kube-scheduler component                         does the task of scheduling in Kubernetes making sure none of the servers in the cluster is overloaded.
  
##### kube-controller manager: 
The controllers in Kubernetes are responsible for controlling the state of the cluster. When you let Kubernetes know what you want in your cluster,                                the controllers make sure that your request is fulfilled
    
##### cloud-controller manager: 
In a real world cloud environment, this component lets you wire-up your cluster with your cloud provider's (GKE/EKS) API. This way, the components                                 that interact with that cloud platform stays isolated from components that just interact with your cluster. 
     
     
### node components
  ##### kubelet: 
  This service acts as the gateway between the control plain and each of the nodes in a cluster. Every instructions from the control plain towards the nodes, passes                  through this service. It also interacts with the etcd store to keep the state information updated.
    
  ##### kubeproxy: 
  This small service runs on each node server and maintains network rules on them. Any network request that reaches a service inside your cluster, passes through                    this service.
    
  ##### container runtime: 
  Kubernetes is a container orchestration tool hence it runs applications in containers. This means that every node needs to have a container runtime like                             Docker or rkt or cri-o.

###### API Request Flow 
 
 
 ![image](https://user-images.githubusercontent.com/20655128/133031485-ede88815-f176-42d9-a023-aeb12300f7b0.png)
 

### Kubernetes Objects:
  Pods: A pod usually encapsulates one or more containers that are closely related sharing a life cycle and consumable resources.
  Services: a Service is an abstraction which defines a logical set of Pods and a policy by which to access them
  
  ### Create kubernetes cluster:
 
kubectl, one of the most important tools to interact with your Kubernetes cluster. Follow this guide to install it:         https://kubernetes.io/docs/tasks/tools/install-kubectl/.
    
 We will use kind to create kubernetes cluster. To install kind follow the document https://kind.sigs.k8s.io/docs/user/quick-start/#installation

 Create a cluster
 
        kind create cluster --name k8sdemo

  List clusters 
  
       kind get clusters
  
  Set Kube context 
  
     kubectl config use-context k8sdemo
     
  Deleting a cluster

      kind delete cluster --name k8sdemo
    
    


 ### Helloworld in kubernetes
 Once the kind cluster up and running 
    
    
        kubectl run hello-world --image=reddeppas/node-web-app:latest --port=80
 
 The run command runs the given container image inside a pod.
 
 Pods are like a box that encapsulates a container. To make sure the pod has been created and is running, execute the following command:
 
     kubectl get pod
 
 Pods by default are inaccessible from outside the cluster. To make them accessible, you have to expose them using a service. So, once the pod is up and running, execute the following command to expose the pod:

      kubectl expose pod hello-world --type=LoadBalancer --port=80
    
 To make sure the load balancer service has been created successfully, execute the following command:
    
     kubectl get service
    
  Deleting resources in Kubernetes
  
     kubectl delete <resourec type> <resource name>

  To delete a pod named hello-world the command will be as follows:

      kubectl delete pod hello-world
  
  To delete service
  
      kubectl delete service hello-world
    
### Declartive approach vs Impertive approach
    
 In Impertive approach you had to execute every command one after the another manually. Taking an imperative approach defies the entire point of Kubernetes.
    
 In declarative approach you let know the kubernetes desire state for your servers to be in and Kubernetes figures out a way to implement that.
    

### Writing configurations
 Every valid Kubernetes configuration file has four required fields. They are as follows:

##### apiVersion: 
Which version of the Kubernetes API you're using to create this object. This value may change depending on the kind of object you are creating. For creating a Pod the required version is v1.

##### kind: 
What kind of object you want to create. Objects in Kubernetes can be of many kinds. As you go through the article, you'll learn about a lot of them, but for now, just understand that you're creating a Pod object.

##### metadata: 
Data that helps uniquely identify the object. Under this field you can have information like name, labels, annotation etc. The metadata.name string will show up on the terminal and will be used in kubectl commands. The key value pair under the metadata.labels field doesn't have to be components: web. You can give it any label like app: hello-world. This value will be used as the selector when creating the LoadBalancer service very soon.

##### spec: 
contains the state you desire for the object. The spec.containers sub-field contains information about the containers that will run inside this Pod. The spec.containers.name value is what the container runtime inside the node will assign to the newly created container. The spec.containers.image is the container image to be used for creating this container. And the spec.containers.ports field holds configuration regarding various ports configuration. containerPort: 80 indicates that you want to expose port 80 from the container.

    
    kubectl apply -f <configuration file>
    
    kubectl apply -f k8s/pod.yaml
    
    kubectl apply -f k8s/service.yaml
    
    kubectl get pod
    
    kubectl get service
 
    

##### Config Maps

A ConfigMap is  used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

Use a ConfigMap for setting configuration data separately from application code. The data stored in a ConfigMap cannot exceed 1 MiB


         kubectl apply -f k8s/configmap.yaml
         
         
            env:
             Define the environment variable in deployment.yaml
            - name: app_build
              valueFrom:
                configMapKeyRef:
                  name: hello-world-configmap
                  key: app_build
            - name: app_name
              valueFrom:
                configMapKeyRef:
                  name: hello-world-configmap
                  key: app_name
            

##### Secrets:

A Secret contains a small amount of sensitive data such as a password, a token, or a key.
 
                kubectl apply -f k8s/secret.yaml
                
                containers:
                - name: hello-world
                  image: reddeppas/node-web-app:1.0
                  ports:
                  - containerPort: 80
                  volumeMounts:
                  - name: helloworld
                    mountPath: "/etc/helloworld"
                    readOnly: true
                volumes:
                - name: helloworld
                    secret:
                      secretName: hello-world-secret
                 



###### Types of secrets

Opaque:	arbitrary user-defined data
 
kubernetes.io/service-account-token:	service account token

kubernetes.io/dockercfg:	serialized ~/.dockercfg file

kubernetes.io/dockerconfigjson	serialized ~/.docker/config.json file

kubernetes.io/basic-auth:	credentials for basic authentication

kubernetes.io/ssh-auth:	credentials for SSH authentication

kubernetes.io/tls:	data for a TLS client or server

bootstrap.kubernetes.io/token:	bootstrap token data


##### ReplicationController: 
as the name suggests allows you to easily create multiple replicas very easily. Once the desired number of replicas are created, the controller will make sure that the state stays that way.

##### ReplicaSet:
ReplicaSet can provide you with a wider range of selection option, both ReplicationController and ReplicaSet are more or less the same thing.

##### Deployment:
A Deployment is like an extension to the already nice ReplicaSet API. Deployment not only allows you to create replicas in no time, but also allows you to release updates or go back to a previous function with just one or two kubectl commands.

    REPLICATIONCONTROLLER                          |	        REPLICASET	                         |                    DEPLOYMENT
     Allows the creation of multiple pods easily	   |     Allows the creation of multiple pods easily | 	      Allows the creation of multiple pods easily
    The original method of replication in Kubernetes.  |	    Has more flexible selectors	             |       Extends ReplicaSets with easy update                                                                                                                                roll-out and roll-back



##### Persistent Volumes and Persistent Volume Claims
A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes.
PVs are resources in the cluster. PVCs are requests for those resources and also act as claim checks to the resource.



##### Ingress Controllers
 ClusterIP to expose an application within the cluster 
 LoadBalancer to expose an application outside the cluster.
 Nodeport: NodePort opens a specific port on all the nodes in your cluster, and handles any traffic that comes through that open port.


Ingress is actually not a type of service. Instead, it sits in front of multiple services and acts as a router of sorts.

An IngressController is required to work with Ingress resources in your cluster.

![image](https://user-images.githubusercontent.com/20655128/133029698-17824958-c1b2-4653-9275-c9b2a98e05b9.png)
