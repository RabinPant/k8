### DAY 1 Learning:
```
what is k8
=> k8 is the container orchestration platform which helps to deploy the containers and the run the application

why the need of k8?
Container are ephermal in nature, i.e container are short lived and they provide only one host. if one resources requires lots of memory then the other might not get the resources to spin another container. single host is the problem and the last but the most important is the container can't spin new container if its killed by external force. These all prolem of container is fixed by the k8. 

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

spec: 
    selector:
        matchLabels:
            app: myapp
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
