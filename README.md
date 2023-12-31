### Kubernetes course for beginners 

https://www.youtube.com/watch?v=d6WC5n9G_sM 

PLAN
- Terminollogy and key features of Kubernetes 
- Create Kubrnetes cluster locally 
- Create and scale deployments 
- Build custom Docker image and create deployment using published image
- Creae services and deployments using YAML files
- Connect different deployments together
- Change container runtime from Docker to SRI-O 


Kubennetes could be run without Docker 

Kubernetes is container orchestration system 

Kubernetesallow to run software on diffetent servers physical and virtual, and all is done automatically wothout your intervension, You just tell Kubernetes how many containers you would like to create based on secific image 

Kubernetes or K8s:
- Automatic deployment of the containerized applications across different servers 
- Distribution of the load across multiple servers 
- Auto-scaling of the deployed applications 
- Monitoring and health check of the containers 
- Replacement of the failed containers 

K8s supports following container runtimes:
+ Docker
+ CRI-O 
+ Containerd 


POD is the smallest unit in k8s world 
Containers are created inside of the POD 

Inside of the POD cpuld be:
- one or several containers
- shared volumes 
- Shared IP address 

ONE CONTAINER PER POD IS THE MOST COMMON USE CASE

ONE POD - ONE SERVER 

### K8S Cluster consists of Nodes   
Node is a server 
Inside each node there are pods 

### MASTER NODE MANAGES WORKER NODES 
Master node distributes loads across working nodes 

K8s Cluaster: Master node -> several worker nodes 

Master node is a system node that is responsible for work of K8s cluster in general

K8s services: 

Master node includes:
- kubelet
- kubo-proxy 
- Container Runtime 


Worker node includes:
- API SERVER
- Scheduler 
- Kube Controller Manager 
- etcd   - stores all log related to operating systems
- kubelet
- kubo-proxy 
- Container Runtime - runs containers inside of each node 

Kubelet is a service that communicates with each API service on the Master node 


### Api server is done by cubectl   cube control
cubetcl is separate command line tool which allows us to connect to specific k8s cluster and manage it remotely 
- cubctl can be run on local pc 

Master node communicates with kubectl via HTTPS 

PODs are created automatically by K8s 

Inside PODs there are containers - usually single container per POD 

### Possible to creatw cluster on local computer. For that reason needs minikube 

minikube  

in order to run minikube neet to utiize virtual machine or container manager 

### SUPPORTED
- VirtualBox
- VMWare 
- Docker 
- Hyper-V 
- Parallels 


In Docker all PODs will be created inside container 
How to install minikube on Ubuntu 22.04:
https://www.learnitguide.net/2023/04/how-to-install-minikube-on-ubuntu-2204.html 



### VSCode kubernetes extension

https://kubernetes.io 

https://kubernetes.io/docs/tasks/tools/ 

Install cubectl on linux 
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/ 

nstall kubectl binary with curl on Linux
```bash 
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
``` 
Download the kubectl checksum file: 
```bash
$ curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```
Validate the kubectl binary against the checksum file: 
```bash
$ echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
```
If valid, the output is:
```nash 
kubectl: OK
``` 

Install kubectl 
```bash
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Check the version of kubectl:
```bash
$ kubectl version --output=json
```
or 
```bash
$ kubectl version --output=yaml
```

### Install minikube locally 
kind lets you run Kubernetes on your local computer. This tool requires that you have either Docker or Podman installed. 

Install kind on Linux:

# For AMD64 / x86_64

```bash
$ [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
$ chmod +x ./kind
$ sudo mv ./kind /usr/local/bin/kind
```
Check version:
```bash
$ kind --version
```

### Creatng a cluster 

```bash 
$ kind create cluster
``` 

For help
```bash 
$ kind create cluster --help
``` 

## MiniKube installation
https://kubernetes.io/ru/docs/tasks/tools/install-minikube/ 

```bash
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
```

```bash 
$ sudo mkdir -p /usr/local/bin/
$ sudo install minikube /usr/local/bin/
``` 
Check version
```bash
$ minikube version
```

Minikube help
```bash
$ minikube help
```


Install virtualbox on ubuntu 22 
https://itslinuxfoss.com/virtualbox-ubuntu22-04/ 
```bash
$ sudo apt update && sudo apt upgrade 
$ sudo apt install virtualbox
```

Minikube start
```bash
$ minikube start --driver=docker
$ minikube start --driver=virtualbox
```

```bash
$ minikube status
```


Docker is running inside the minikbe node 

Find out IP address
```bash
$ minikube ip
```
Connect via SSH
```bash 
$ ssh docker@<IP>
```
TYpe: yes 

Login: docker
Password: tcuser


or just type
```bash
$ minikube ssh
```

Inside a minikube console 
```bash 
docker ps
exit
```

Cubectl is a tool to manage k8s clusters 

```bash
$ minikube kubectl --help 
$ minikube kubectl cluster-info
```
```bash
$ kubectl cluster-info
$ kubectl get pods 
$ kubectl get namespaces
$ kubectl get nodes
```

### Namesaces are used in k8s to group different resources and different objects

```bash
$ kubectl get pods --namespace=kube-system
```

### To create a POD
name of the POD does not have to match with image name
```bash
$ kubectl run nginx --image=nginx
```
in the end =nginx - nginx is a docker inmage which will be pull automatically and container will be created based on this image 

To descrie a POD
```bash
$ kubectl describe pod <podname>
$ kubectl describe pod nginx
``` 

In orddr tp connect to PODs need to create services 

```bash
$ minikube ssh
# inside minikube
$ docker ps | grep nginx
``` 

All containers inside same POD share namespaces

To connect to the container inside minikube ssh
```bash
$ docker exec -it ContainerId sh
# for example
$ docker exec -it  7624319c14e1 sh
``` 

hostname 
IP address of container
```bash
$ hostname
$ hostname -i
$ curl 10.244.0.4

```

```bash
$ kubectl get pods -o wide
``` 

To delete a pod
```bash
$ kubectl delete pod podName
$ kubectl delete pod nginx
```
### Create deployment 
```bash
$ kubectl create deployment nginx-deployment --image=nginx

$ kubectl get deployments

$ kubectl get pods

$ kbectl describe deployment nginx-deployment
```
Pod is managed by deployment 

Selector needs to connect pods with deployments 

NewReplicaSet 

Replica set manages all pods related to deployment 
You can create lot of pods in the same deployment 
and all of them are included in the replica set 

```bash 
$ kubectl describe pod nginx-deployment-66fb7f764c-4zzsq 
``` 

To scale deployment
```bash 
$ kubectl scale deployment nginx-deployment --replicas=5
```
now we scaled to 5 replicas and 5 different pods are running in our k8s cluster 
```bash 
$ kubectl scale deployment nginx-deployment --replicas=3
``` 

To connect to node:
```bash
$ minikube ssh
$ curl <IP>
``` 


### Load balancer 
Podscould be distributed across different nodes 

Common solution is to have a load balance IP address 
it will be single IP address for specific cluster under specific deployment 

### Create cluster IP for deployment


```bash 
$ kubectl get deployments 
``` 

there are 3 pods and inside of each pod there is Nginx container
and in each container is a web server running on port 80

- to expose internal port from the deployment port 

```bash
$ kubectl expose deployment nginx-deployment --port=8080 --target-port=80
```

GET SERVICES

```bash
$ kubectl get services 
$ kubectl get svc
``` 

Virtual IP address that was made by k8s in order to connect to any of pods 
This IP allows you to connect to any deployment 
1h 15m 

Cluster IP is only available inside the cluster 

Cluster IP address is NOT AVAILABLE outside the k8s cluster 

```bash
$ minikube ssh

# is provided by the one of the pods of a deployment
curl 10.99.5.79:8080
```

Cluster IP is just a single IP for all pods in deployment 


```bash
$ kubectl describe service nginx-deployment
```

1h 20m 
Delete deployment and service
```bash
$ kubectl delete deployment nginx-deployment 
$ kubectl delete service nginx-deployment
``` 


### WEB APP 
PLAN: 
- create NOdejs Express web app 
https://github.com/bstashchuk/k8s 

Install Docker and Kubernetes extension for VSCode 

Install Docker desktop 
https://www.youtube.com/watch?v=Y4ut8kKpPUs 


https://www.docker.com/products/docker-desktop/ 



Build docker image vith Dockerfile with tagname to be able to upload it on docker hub

```bash
$ docker build . -t dmitrysafarov/k8s-web-hello 
```

```bash 
$ docker images | grep k8s-web
```

Push customely build image to hub.docker.com

```bash 
$ docker login

$ docker push dmitrysafarov/k8s-web-hello
``` 

Make k8s deployment based on dockerhub image

```bash
$ kubectl create deployment k8s-web-hello --image=dmitrysafarov/k8s-web-hello 
```

1h 42 mins 

Create service using cluster IP anconnet to the webserver running Nodejs/Express

```bash
$ kubectl expose deployment k8s-web-hello --port=3000 

$ kubectl get svc

$ minikube ssh 
$ curl <CLUSTER_IP>:3000

```  

```bash
$ kubectl get pods
$ kubectl get deploy
``` 

Scale dployment
```bash
$ kubectl scale deployment k8s-web-hello --replicas=4 

$ kubectl get pods
$ kubectl get pods -o wide
```
4 pods are running and each pod has own unique IP address 

then in minukube ssh we would get responses from various pods 
NOW LOAD IS BALANCED ACROSS DIFFERENT PODS  
this is how distributuon of a load works in action 

```bash
$ kubectl delete svc k8s-web-hello

$ kubectl get svc
```

Create service again with type NotePort 
```bash
$ kubectl expose deployment k8s-web-hello --type=NodePort --port=3000
```
Randomly created port
Now it is possible to connect to deployment using node IP address
and when we see the IP address of minikube and also see the random port of our service
```bash 
$ kubectl get svc 
$ minikube ip
```
we can open web browser and put it like http://192.168.59.100:30323/ and it should work

we can also coonect via minkube service
```bash
$ minikube service k8s-web-hello
``` 
to see only url:
```bash
$ minikube service k8s-web-hello --url
```   
delete service
```bash
$ kubectl delete svc k8s-web-hello 
$ kubectl get svc
```
### Create Load balance service 

```bash
$ kubectl expose deployment k8s-web-hello --type=LoadBalancer --port=3000 

$ kubectl get svc 

$ minikube service k8s-web-hello

$ kubectl get deploy
$ kubectl describe deployment k8s-web-hello
``` 


### Rolling update - Update version of the image in deployment 
new PODs will be created with new image while previous pods will be still running
So, pods WILL BE REPLACED ONE BY ONE 
And finally all pod will run new updated image


build and push a modified image of app with new tagname
```bash
$ docker build . -t dmitrysafarov/k8s-web-hello:2.0.0

$ docker push dmitrysafarov/k8s-web-hello:2.0.0
``` 

2h 

Set image for deployment
```bash
$ kubectl set image deployment k8s-web-hello k8s-web-hello=dmitrysafarov/k8s-web-hello:2.0.0 

$ kubectl rollout status deploy k8s-web-hello
```

```bash
$ minikube service k8s-web-hello 
``` 
to check status updated
```bash
$ kubectl rollout status deploy k8s-web-hello 
``` 



To delete one pod manually
```bash 
$ kubectl get pods
$ kubectl delete pod k8s-web-hello-7958777c6c-6snjh
```
but new pod was automatically created 
bexause we told k8s that we desired quantity of the pod to be 4

### How to launch k8s dashboard 

```bash 
$ minikube dashboard
```

2h 10m

### Apply configuration files 

To delete all resources and services in default namespace
```bash
$ kubectl delete all --all
```

we also delete kubernetes service but it automatically recreated 

### DEPLOYMENT YAML CONFIG
- create file deployment.yaml in root
begin to type: Depl.. and select Kubernetes deployment 

Documentation: https://kubernetes.io/docs/reference/

APPLY DEPLOYMENT
```bash 
$ kubectl apply -f deployment.yaml  
```


```bash
$ kubectl get pods
$ kubectl get deploy
```

2 h 23m 

### Create service config YAML file 
create file service,yaml 

type in Serv and rhen check Kubernetes service

APPLY SERVICE
```bash 
$ kubectl apply -f service.yaml  
```

```bash
$ minikube service k8s-web-hello
```

### To delete Service and Deloyment
```bash
$ kubectl delete -f deployment.yaml -f service.yaml
``` 


We will connect to API via loadBalancer IP address 
From one deployment we will connect to other deployment and will get response from one of the pod which will be running nginx application 
and we will get result to the clien with the response from Nginx pod 

We create a service called Nginx for nginx deployment 

build and push a docker file:
```bash
$ docker build . -t dmitrysafarov/k8s-web-nginx

$ docker push dmitrysafarov/k8s-web-nginx
``` 

### How to combine deployment.yaml and service.yaml configuraion in one file

create file k8s-b-nginx.yaml 

create file nginx.yaml 

## APPLY
```bash
$ kubectl apply -f k8s-web-nginx.yaml -f nginx.yaml
```

Service name is static, cluster ip is dynamic 

```bash
$ minikube service k8s-web-nginx
``` 
ifyou add /nginx to thr URL in web browser you will get to another service 

```bash
$ kubectl get pods
$ kubectl exec k8s-web-nginx-7d4b4cc9b7-6p8qp -- nslookup nginx
```

to dekete
```bash 
$ kubectl delete -f nginx.yaml -f k8s-web-nginx.yaml 
``` 


### Change container runtime from Docker 

```bash
$ minikube status
$ minikube stop
$ minikube delete
``` 

Create new claster with minikube with CRI-O or containderd
```bash
$ minikube start --driver=virtualbox --container-runtime=cri-o  

$ minikube start --driver=virtualbox --container-runtime=containerd
``` 

```bash
$ minikube ip

$ mnikube ssh

$ sudo crictl ps
```

```bash
$ kubectl apply -f k8s-web-nginx.yaml -f nginx.yaml

$ minikube service k8s-web-nginx
```
