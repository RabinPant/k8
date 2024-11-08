### DAY 1 Learning:
```
what is k8
=> k8 is the container orchestration platform which helps to deploy the containers and the run the application

why the need of k8?
Container are ephermal in nature, i.e container are short lived and they provide only one host.
if one resources requires lots of memory then the other might not get the resources to spin another container.
single host is the problem and the last but the most important is the container can't spin new container
if its killed by external force. These all prolem of container is fixed by the k8. 

Single host, autohealing, loadbalancer, autoscaling, doesn't give any enterprise solution => problem with container


k8 => group of cluster => group of node => install in master node archticture so basically it can provide support to lots of container
autoscaling = replicaset
k8 also support HPA(horizonatal pod autoscale)
AutoHealing = when pods start to go down, apiServer will spin up new container.
k8 custom resource => ingress controller
=====================================================================================

k8 architecture:
k8 has control plane and Data plane:

In control plane(Master Node) : APIserver, scheduler, etcd, CCM
In data plane (Worker Node): kublet, proxy and conntainer runtime


Docker runtime = docker shimp 
k8 = supports any contianer runtime =? continerD, crio etc
kubelet = > if anything goes wrong with pod, creation of pod, makes pod run all the time
kubeproxy => Ipaddress, loadbalancing, use IpTables
Container Runtime=> for the application to run in the k8

MASTER NODE || CONTROL PLANE:

API SERVER = Heart of k8, gets instruction from external world
Scheduler = scheduling running of pod in node1 or node2 after getting instruction from the API server
etcd = key value pair/ have all the cluster related information
controller manager = to support autoscailing k8 has controllers/ Relicaset controller manage the autoscailing
CCM(Cloud control manager) = if you want to run k8 like EKS in the cloud then CCM comes handy.

```
Day 2
## How to manage Hundreds of kubernetes cluster
```
what kuberntes distribution have you used in the production?
EKS, openshift , Rancher, Tanzu
Common tools used to manage 100 of K8 cluster = KOPS

aws config

```
## Deploy Your first app

```
pod = lowest level of deployment => defination of how to run container

if you put two continer in same pod:
* shared Network
* Shared Storage
k8 CLI  = kubectl

Install minikube, and KIND


Difference container pod and deployment?
Pod can be single or multiple contaier and pod provides yaml specification for container deployment.
Docker => k8 because of auto healing and auto scaling
Pod can't do auto healing and auo scaling so to implement this we need
deployment.
deployment => create replicaet and this replicaset will rollout pod
repicas => no of pod that we want for larger traffic.


Deployment VS Replicaset
RS = Controller implmenting autohealing or autoscaling

kubectl get all -A = get all resources
kubectl apply -f pod.yml = create the pod


```
Day 3:
## k8 services

```
pod -> deployment-> service
why???
No service scenario

when there are 10 concurrent user

every time pod goes down. when it comes up then the IP address is change if there is not service

service makes use of kube-proxy which helps in load balacing.
172.15.3.4
172.16.3.5    -> Deploy -> svc          <- user will hit the service
172.16.3.6

svc -< load balancing
It helps in service discovery
ip address -> labels and selectors, different labels like payment label for pods

3) exposed to external world
    service of three types:
        -1 cluster IP = inside (Discovery)
        2 Nodeport = inside org ()
        3 loadbalancer = external world exposed



```
Day 4
### Some questions

```
Difference between Docker and Kubernetes?
Docker is a contanier platform and kubernetes is contianer orchastration platform. continers are ephermal in nature and deosn't provide the feature like auto healing, auto sacling, clustering enterprise level support.

 What are the main components of kubernetes architecture?
1) control plane(API server, scheduler, controller manager, C-CM, ETCD)
2) Data Plane(Kubelet, kube-proxy, container runtime)

What are the main differences between the Docker Swarm and kubernetes?
kubernetes is better suited for large organisations as it offers more scalability, networking capabilities like policises and huge third party ecosystem support.

Difference container and kubernetes pod?
a pod in k8 is a runtime specification of a container in docker. a pod provides more decalritive way of defining using YAML and you can run more than one contaienr in a pod.

What is a namespace in a k8?
In k8 namespace is a logical isolation of resources, network policies, rbac and everything. for example, there are two projects using same k8 cluster. one project can use ns2 without any overlap and authentication problems. RBAC(role based access control).

what is the role of kube proxy?
kube proxy works by maintaining a set of network rule on each node in the cluster, which are updated dynamically as services are added or removed. when a client sends a request to a service, the request is intercepted by kube-proxy on the node where it was received. kube-proxy then looks up the destination endpoint for the services and routes the request accordingly.

kube-proxy is an essential component of a kubernetes cluster, as it ensures that services can communicate with each other.

what are the different types of services within k8?
There are different types of services that a user can create
1) cluster IP Mode
2) Node port mode
3) load balancer mode

what is the difference between nodeport and loadbalancer?
when a service is created a nodeport type, the kube-proxy updates the Iptables with node ip address and port that is chosen in the service configuration to access the pods.

where as if you create a service as type loadbalancer, the cloud contorl manager creates a external load balancer IP using the underlying cloud provider logic in the C-CM. user can access services using the external IP.

what is the role of Kubelet?
Kubelet manages the containers that are scheduled to run on that node. It ensures that the container are running and healthy, and that the reources they need are available.

kublet communicates with the k8 API server to get the information about the container that should be running on the node, and then starts and stops the containers as needed to maintain the desired state. It also monitors the containers to ensure that they are running correctly and restarts them if necessary.


replication Controller | Replica set

These both are different technology where replication controller is the old one and the replica set is the new one.


```

### K8 Services Deep Dive | Traffic flow using kubeshark

```
play around Node port and Load balancer mode in the service yaml file
expose application using node port for internal organization and
outside by using the load balancer.

Service Discovery:
discovery of service using selector and label.


```

### Labels and selectors
```
The replicaset watches all the pods, in other word it monitors all the pods and to know which to look at there comes the concept of labels and selectors.

when we label pod while creating pod.defination.yml file then in replicaset we create a selector which tries to match the labels with that pod.

kubectl replace -f replicaset-definition.yml
kubectl scale --replicas=6 -f replicaset-definition.yml
kubectl scale --replicas=6 replicaset myapp-replicaset


```

### Deployment Rollout and Update
```
kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/my-app-deployment

Deployment Strategy:

Recreate : All the pods are killed and then all the pods are re-run again
There will be some interval when we kill and re-run the pods.

Rolling update: one pod is killed then another is up, individula pods are killed and brought up again, one at a time.

kubectl apply -f deployment-definition.yml
kubectl set image deployment/my-app-deployment nginx-container=nginx:1.9.1

upgrade the image:
*** kubectl edit deploy frontend
kubectl set image deploy frontend simple-webapp=kodekloud/webapp-color:v2
Here simple-webapp is the container name.

Rollback=> kubectl rollout undo deployment/my-app-deployment
kubectl rollout status deployment/my-app-deployment
kubectl rollout history deployment/my-app-deployment


*** Kubectl create -f deployment.yaml
*** kubectl rollout status deployment.apps/myapp-deploymen
*** kubectl history status deployment.apps/myapp-deployment
*** 


```

### Networking
```

```
## command for k8 replicaset

```
kubectl create -f replicaset-defination.yml
kubectl get replicaset
kubectl delete replicaset myapp-relicaset
kubectl replace -f replicaset-defination.yml
ku
kubectl scale --replicas=6 -f replicaset-defination.yml
```

### How to bind a pod with the replicaset

```
To bind a replicaset with a pod we have to create a separate replicaset.yml file and can provide both things under it

apiVersion: apps/v1
kind: Replicaset
metadata:
    name: myapp-replicaset
    labels: 
        app: myapp
        type: front-end

spec: 
    selector:
        matchLabels:
            type: front-end
    replicas: 3
    template:
        metadata:
            name: nginx-2
            labels: 
                app: myapp
        spec:
            containers:
                -name: nginx
                image:nginx

```
### create the replicaset using the yaml file

```
kubectl create -f replicaset.yaml

# check status: kubectl get replicaset

kubectl get replicationcontroller

 kubectl get pods
 kubectl delete pod myapp-...

kubectl edit replicaset myapp-replicaset
```

### Deployment

```
kubectl create -f deployment.yaml

kubectl rollout status deployment.apps/myapp-deployment

kubectl delete deployment myapp-deployment

Inspect the pods and webservices:
kubectl get pod
kubectl get deploy

Detail of the deployment or the Pod:
kubectl describe frontend

```

### Basic of networking in K8

```


```
```
Study Guide for Kubernetes Services, Ingress with TLS, and Ingress Controllers
Overview of Kubernetes Services
Kubernetes Services: An abstract way of exposing applications running as pods within a Kubernetes cluster. Services enable communication between different components of an application and allow external access to the application. 1
Ingress Controllers
Ingress Controllers: Manage external access to services in a Kubernetes cluster, typically HTTP. They provide routing rules to manage traffic and can handle SSL/TLS termination.
Key Functions of Ingress Controllers:
Routing: Directs incoming traffic to the appropriate service based on defined rules.
Security: Can implement SSL/TLS for secure communication.
TLS (Transport Layer Security) in Ingress
SSL Passthrough: This method allows encrypted client-to-server requests to pass through the load balancer without decryption. The load balancer acts merely as a pass-through, which can impact latency. 2

SSL Offloading: This technique involves decrypting SSL/TLS traffic at the load balancer level, reducing the load on backend services and improving performance. It addresses latency issues caused by SSL passthrough. 3

Example of Secure Ingress
Client and Server Certificates: To establish secure connections, client and server certificates can be created. This ensures that both parties can authenticate each other, enhancing security. 4
Important Concepts to Remember
Ingress Resource: Defines how external HTTP/S traffic should be routed to services.
TLS Termination: The process of decrypting SSL/TLS traffic at the ingress controller level.
Questions to Consider
What are the differences between SSL passthrough and SSL offloading?
How do ingress controllers manage traffic in a Kubernetes cluster?
What steps are involved in setting up secure ingress with TLS?
This study guide provides a structured overview of Kubernetes services, ingress controllers, and TLS implementation, helping you grasp the essential concepts and practices in managing secure traffic in Kubernetes.


```
