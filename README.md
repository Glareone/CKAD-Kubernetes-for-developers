# Kubernetes for developers, tech leads and solution architects
This repository contains materials prepared by myself and collected over the internet regarding Kubernetes and how to properly use it in case you are a developer, a tech lead or a solution architect 

# Recommended video course and Examples:
[Pluralsight: Kubernetes for developers](https://app.pluralsight.com/library/courses/kubernetes-developers-core-concepts/table-of-contents)    
Video Course with a good intro about Docker and Docker runtime: [Oreilly CKAD: Certified Kubernetes Application Developer](https://learning.oreilly.com/videos/certified-kubernetes-application/9780136677628)  
Lots of Service \ NetworkPolicy \ Deployment [EXAMPLES](https://github.com/Glareone/Certified-Kubernetes-Application-Developer) 

# Docker Intro and useful materials

<details>
<summary>Docker. busybox. Docker commands</summary>

busybox - is a minimal linux container to emulate workload. if you type the command without `-it` it immediately stops because container doesnt know what to do, there is no application inside.

> docker run -it busybox

to inspect what's happening within the container:  
> docker inspect <ID> | less
</details>
  
# Kubernetes. General Information. About Kubernetes itself
#### When Kubernetes is worth?
* New devs onboarding.
* You can eliminate Application conflicts. (You can run multiple versions of one application simultaneously)

<details>
<summary>Kubernetes application instance creation diagram</summary>
  
  ![MicrosoftTeams-image](https://user-images.githubusercontent.com/4239376/188572077-42c51924-f2de-4173-8837-b26bb5d9d2a3.png)
</details>

### Kubernetes building Blocks.

<details>
<summary>Cluster, Internal Structure, Building Blocks, State Management</summary>

  ![1 Cluster](https://user-images.githubusercontent.com/4239376/149999683-875c45bd-503e-4f96-bbca-4490e94fdbe8.png)  
  ![2 state management](https://user-images.githubusercontent.com/4239376/150000162-71be084d-1a6b-409e-9239-63827c6f6e96.png)  
  ![3 pod](https://user-images.githubusercontent.com/4239376/150000193-9174b15d-6fb2-42e0-a107-5114cbbf970a.png)  
  ![4 K8s Building blocks](https://user-images.githubusercontent.com/4239376/150000219-c4d8705a-f7d3-4eb4-8189-50aa15ca9e1c.png)  
  ![5 Node - virtual machines + agents](https://user-images.githubusercontent.com/4239376/150000241-ba7e45f7-fb21-4b87-9724-936ea352a57b.png) 
  ![6 K8s interfaces](https://user-images.githubusercontent.com/4239376/150000271-eea554dc-1d57-4fc4-8452-d62860c34b2e.png)
  ![7 Node agents](https://user-images.githubusercontent.com/4239376/150000291-26f1c468-a373-48fb-958d-ae84612224b2.png)
  ![8 Kubernetes in docker](https://user-images.githubusercontent.com/4239376/150000309-b0ebc220-f4f5-461b-bfda-4d0ddab7241b.png)
</details>

### Containers
<details>
<summary>Init Containers</summary>
  
* Kind of containers which should be started before launching the main application container  
![image](https://user-images.githubusercontent.com/4239376/206247417-b660a69e-6b15-4640-98cf-0eb8c0e990bb.png)
  
* Init Container Example:  
![image](https://user-images.githubusercontent.com/4239376/206248788-fbb4d08b-96b3-49d6-a1a0-7890c0fbd6da.png)  
Pay attention on sleep mode. After 20 sec it's done. If you havent declared end status it will run forever and your main application won't start.


</details>
  
### Pods
<details>
<summary>Kubernetes Pod. Brief Explanation</summary>

![image](https://user-images.githubusercontent.com/4239376/204153391-2192ab2f-9f84-4bef-8e78-f084d1432ba0.png)
</details>

[What happened when you create a POD. Comprehensive article on medium](https://medium.com/@karthikeyan_krishnaswamy/overview-of-kubernetes-34d8e0e59b26)

<details>
<summary>Pod Lifecycle diagram and list of steps</summary>
    
![image](https://user-images.githubusercontent.com/4239376/189322111-652e11f7-4c51-4b63-b2b9-82b43f67554d.png)

1. kubectl writes to the API Server.
2. API Server validates the request and persists it to etcd.
3. etcd notifies back the API Server.
4. API Server invokes the Scheduler.
5. Scheduler decides where to run the pod on and return that to the API Server.
6. API Server persists it to etcd.
7. etcd notifies back the API Server.
8. API Server invokes the Kubelet in the corresponding node.
9. Kubelet talks to the Docker daemon using the API over the Docker socket to create the container.
10. Kubelet updates the pod status to the API Server.
11. API Server persists the new state in etcd.
</details>
  
<details>
<summary>Multi-container pod. Good scenarios to run multiple containers in one pod.Sidecar.</summary>
  
![image](https://user-images.githubusercontent.com/4239376/204357555-59fe745c-e0ba-4300-a3dd-ae9aeab8c8d7.png)
![image](https://user-images.githubusercontent.com/4239376/204357834-afde819f-c018-4833-8367-d6bb767564c6.png)

Sidecar example:
* One container generates logs (busy box), another one exposes the logs(sidecar). 
* Instead of busy box it could be your database. The sidecar in this scenario may limit the amount of logs you want to expose.

![image](https://user-images.githubusercontent.com/4239376/204358240-fb8162f6-aa40-4629-b386-1466e135ff4a.png)
</details>
  
<details>
<summary>Pod resources. Cpu & memory request and limit. How Kubernetes manages them</summary>
* kubernetes rely on the following mechanism
Kubectl -> docker run (or any other engine you use for containers) -> linux CGroups.   
  * Linux CGroups support resource limitation
  
![image](https://user-images.githubusercontent.com/4239376/206243911-8957e9f2-4cc2-49db-8e80-f7c603a9ac1e.png)
  
</details>

### Deployments. ReplicaSet. Labels. Annotations.

<details>
<summary>Deployments specifications: replicas, strategy, labels</summary>

![image](https://user-images.githubusercontent.com/4239376/207669037-cd5dd926-17f1-4fb8-854a-28a3eb7dc7e8.png)
![image](https://user-images.githubusercontent.com/4239376/207669416-41817e3f-9d6f-439b-83c0-afe007df0814.png)

</details>
  
<details>
<summary>ReplicaSet</summary>
  
![image](https://user-images.githubusercontent.com/4239376/207676941-0f892b1e-538d-4e4a-ad33-920904904a23.png)
</details>
 
<details>
<summary>Labels. Can be used in a query. How to Query</summary>

## Labels
* Interesting Fact: Kubectl deployment monitors through the replicaset to ensure that k8s has sufficient amount of pods is available which have assigned label.
* It means, it uses selectors to track pods. If you delete label from pod but in deployment you declared it - deployment will create another pod in a couple of seconds.
  
  ![image](https://user-images.githubusercontent.com/4239376/207681444-7ddfe7d5-237f-424c-8abe-0688cf4dac22.png)
  
Labels could be assigned automatically. For example in case of using K8s Dashboard to create resource:  
 ![image](https://user-images.githubusercontent.com/4239376/207685024-61f51361-cb4d-4c56-98af-19265769265d.png)

## How to use query:
* Show all labels for all resources `kubectl get all --show-labels`:  
![image](https://user-images.githubusercontent.com/4239376/207687317-67bc5e7f-d968-49e7-b169-8f0333dbbae9.png)

* Use selector `kubectl get all --selector app=nginx --all-namespaces`:
![image](https://user-images.githubusercontent.com/4239376/207687662-1a788c48-4471-4f52-b282-8297ed9d3323.png)
</details>
  
<details>
<summary>Annotations. Can NOT be used in a query</summary>
   
  ![image](https://user-images.githubusercontent.com/4239376/207682314-8db126ec-afe7-40fa-8a14-c4763d96acf5.png)
</details>

### Deployment History. Rollout updates. Update Strategies

<details>
<summary>Deployment History</summary>
  
![image](https://user-images.githubusercontent.com/4239376/207688288-309f93a7-5e00-4ad5-8b8c-dc4114de7698.png)
  
* When new major changes appear Deployment creates new ReplicaSet. Old ReplicaSet still persists, but the number of replicas set to 0.
</details>

<details>
<summary>Rollout updates. Update Strategies. Rollback \ undo changes</summary>
![image](https://user-images.githubusercontent.com/4239376/211910788-0645b26f-5cdd-475a-9cd4-2bec6dda2956.png)


`kubectl rollout history deployment <NAME-OF-DEPLOYMENT> --revision=<NUMBER-OF-REVISION>` - to see what changed in this exact revision step (1 -> 2)  
![image](https://user-images.githubusercontent.com/4239376/211913521-135d2bba-db13-4e16-b882-f85566d97e5c.png) 
  
`kubectl rollout undo deployment <NAME-OF-DEPLOYMENT> --to-revision=<NUMBER-OF-DESIRED\PREVIOUS-REVISION>` - to revert changes to selected previous revision  

### Deployment strategies supported by Kubernetes by default: Recreate and Rolling update
![image](https://user-images.githubusercontent.com/4239376/211911595-585bcb1f-8a4e-4710-81c3-d2c3cdb78aa5.png)

* Recreate - delete all pods and recreate. Leads to temporarly unavailability. 
  - Useful when you cant run several versions of application.
* Rolling update - updaet one pod at time. Guarantees availability. Preferred approach

#### Rollout update parameters: maxUnavailable, maxSurge
![image](https://user-images.githubusercontent.com/4239376/211911941-e5b67a5b-c528-40c2-b62d-779bbc24193d.png)

### Rollout update parameters: example
![image](https://user-images.githubusercontent.com/4239376/211912322-419de20b-ddba-424d-b08e-dbdd7a72b9c6.png)

* Rollout updates manipulate ReplicaSets. Each rollout Update creates new ReplicaSet, populate it and clean up old ReplicaSet  
* Managing rollout updates you can easily revert changes
 
`kubectl rollout undo deployment <NAME-OF-DEPLOYMENT> --to-revision=<NUMBER-OF-DESIRED\PREVIOUS-REVISION>` - to revert changes to selected previous revision    
  
### Deployment strategies in general: Blue-green, Canary, A\B, Multi-service
![image](https://user-images.githubusercontent.com/4239376/197362803-243e0580-737f-4042-8cf0-1ed7ab0173c8.png)
 
</details>
  
### Namespaces
  
<details>
<summary>Kubernetes Namespace. General info</summary>
  
![image](https://user-images.githubusercontent.com/4239376/204359995-49432951-70df-4b7e-b1f2-0701847fff6d.png)
</details>
  
### Kubernetes Jobs

<details>
<summary>Kubernetes Jobs. CronJob. Jobs vs CronJobs.</summary>

![image](https://user-images.githubusercontent.com/4239376/204906371-038c1c9e-52ee-4bfa-9e98-2571ce4d5eb7.png)
  
* Simple example of a job  
![image](https://user-images.githubusercontent.com/4239376/204906540-6e4b43c4-5b48-4079-b662-e7bb9c058211.png)
![image](https://user-images.githubusercontent.com/4239376/204914770-d0ccc7ac-1263-4e39-b7b3-778d5957d06f.png)

## Jobs vs CronJobs
The key difference is that you want to run CronJobs on a regular basis, multiple times, using schedule.
![image](https://user-images.githubusercontent.com/4239376/206235934-b7a5192b-ffb0-4645-9509-a45267d1c3c8.png)

* CronJob creates Job for each run, but has only one CronJob
![image](https://user-images.githubusercontent.com/4239376/206240304-b2a4bac5-d0ff-4720-ac01-7b0a7e8637de.png)
 
</details>

# Running Kubernetes Locally

<details>
<summary>Options to run Kubernetes locally:</summary>

1) minikube (little version of K8s, but with full list of abilities from the full version) - but should have only one master node
2) docker desktop
3) kubernetes in docker (kind) - install kubernetes right in docker desktop application. and you can use all commands from kubectl
4) kubeadm - full version of k8s running locally
  
</details>

## Kubernetes User. RBAC. Authorization Authentication. Security Context.
<details>
<summary>Kubernetes User and user configuration. kubectl config view</summary>
   Kubernetes user is just a connection to some certificates

![image](https://user-images.githubusercontent.com/4239376/204151637-885120e5-4cb5-4e07-87e0-c13720917e3e.png)

  It means Kubectl doesnt need you to log in, just need the certificates to be set in an appropriate way.
  These certificaets lie among other things in hidden .kube config directory
</details>  
 
<details>
<summary>Authorization. kubectl auth can-i</summary>

![image](https://user-images.githubusercontent.com/4239376/204152410-fa776576-ddd9-4550-a54a-de38a59b813d.png)
</details>
  
<details>
<summary>Security Context</summary>
  
![image](https://user-images.githubusercontent.com/4239376/204904649-9702e8cd-1dc7-402b-9a81-59bc09f5e1de.png)
![image](https://user-images.githubusercontent.com/4239376/204904870-5de6e9db-c691-446a-bc0a-1f50cbdcb405.png)
</details>
  
# Kubectl. Kubectl commands under the hood: curl.
  
<details>
<summary>kubectl is just a curl</summary> 

  ![image](https://user-images.githubusercontent.com/4239376/204151154-1ef581e5-fd5a-475d-890a-06d8aef509b0.png)
</details>

<details>
<summary>Managing pods. Kubectl run - runs deployment, not pod itself</summary> 

  ![image](https://user-images.githubusercontent.com/4239376/204153571-489a4efc-ef8f-4cd7-872b-5818643c3dfe.png)
</details>
  
<details>
<summary>Commands with parameters:</summary>
  
`kubectl version`  
`kubectl cluster-info`  
`kubectl gel all` - retrieve all inf about pods, deployments, etc.  
  - you also can use `-o wide` parameter to see extra information
  - `--show-labels` labels attached to pods will be shown. They will help you identify pods

`kubectl run [cont-name] --image=[image-name]`  
`kubectl port-forward [pod] [ports]` - configure your proxy to expose your POD.  
`kubectl expose` - expose your ports  
`kubectl create [resource]` - create resource in k8s based on yml file  
`kubectl apply [res]` - create or MODIFY EXISTING  
  
1. `kubectl run` vs `kubectl create` - in general run is imperative command, kubectl create is declarative way. `kubectl run` is deprecated.
2. `kubectl run` - created deployment, not directly a pod. after running `kubectl run` respective deployment will not be saved.
</details>

<details>
<summary>Kubectl describe vs kubectl logs vs kubectl exec</summary>

* kubectl describe goes to etcd database and returns configurations
* kubectl logs goes on pods level in order to receive logs coming from containers  
  
`kubectl logs [POD_NAME_IN_DEFAULT_NAMESPACE]` or `kubectl logs [YOUR_POD] -n [YOUR_NAMESPACE]`
  
* kubectl is for executing commands on container level. If you have multiple containers under pod - you also need to specify the container's name

![image](https://user-images.githubusercontent.com/4239376/204894819-8732557b-da5f-4c43-b93a-f762869d5567.png)
  
* kubectl exec might also be useful in inspect the container from inside the pod
  ![image](https://user-images.githubusercontent.com/4239376/204895479-628fb92f-df91-4ae9-8007-5946181f1359.png)  
`kubectl exec -it [POD_NAME] -n [NAMESPACE]  -- sh` - as an example
  
PS to exit from interactive terminal you cant use `exit` command, use `ctrl-p ctrl-q`. in Azure CLI you can exit using `exit` command.
  
</details>
  
# Service. Kubernetes networking. Exposing pods outside. Port forwarding

<details>
<summary>Services. General Info. Selector to Deployments. Kube-proxy agent</summary>

## Service. General. Connection to Deployments and Pods inside. 
* Service is a Kubernetes object, which is an abstraction that defines a logical set of Pods and policy by which to access them.
* Service, simply saying - is a load balancer which provides ip-address (in one way or another) and exposes your Pods.
![image](https://user-images.githubusercontent.com/4239376/211929322-de50bc2d-66a3-4521-9d2a-52858bf5fa84.png)

* The set of Pods is determined for Service by selector. Controller scans for Pods that match the selector and include these in the Service. 
* To use selector you need to declare a **Label** in your Deployment.

## Access to Multiple Deployments:
* Service could provide access to multiple deployments. Kubernetes automatically makes Load Balancing between Deployments.

## How Service works under the hood:
* The only thing that Services do is watch for a Deployment that has a specific label set based on the Selector that is specified in the Service.
* Using `kubectl expose`, it really looks as if you are exposing a specific Deployment, but it's not. This command is only for your convenient and doesnt show several Deployments if you have them behind Service

## Kube-proxy agent and ClusterIP port
* kube-proxy agent - is an agent running on the nodes which watches the Kubernetes API for new services and their endpoints. After creation, it opens random ports and listens for traffic to the clusterIP port, and next redirects traffic to the randomly generated service endpoints

## Kube-proxy agent, Service and Pods:
![image](https://user-images.githubusercontent.com/4239376/212548102-fff5439d-c303-41bc-a010-745ef77b7e4f.png)

* P1, P2, P3 - Pods under 2 different deployments.
* Kube-Proxy is an agent which plays Load Balancer role for 3 Pods
* Service is registered in etcd and gives access to external users to these Pods
  
![image](https://user-images.githubusercontent.com/4239376/212550170-0ceeaebd-c869-484f-8b4c-76045ee39c4c.png)
![image](https://user-images.githubusercontent.com/4239376/212551735-5b1c4911-1968-4dc3-973f-560ecfdacdbf.png)

</details>

<details>
<summary>Service types: ClusterIP, NodePort, LoadBalancer, External Name, Service without Selector (useful for Databases)</summary>

* `ClusterIP`: default type, provides internal access only;
* `NodePort`: NodePort, which allocates a specific node port which needs to be opened on the firewall. using these node ports, external users, as long as they can reach out to the nodes' IP addresses, are capable of reaching out to the Service;
* `LoadBalancer`: is a load balancer, but currently only implemented in public cloud. So if you're on Kubernetes in Azure or AWS, you will find a load balancer;
* `ExternalName`: is a relatively new object that works on DNS names and redirection is happening at the DNS level;
* `Service without selector`: which is used for direct connections based on `IP + port` combinations without an endpoint. This is useful for connections to a database or between namespaces.
</details>
 
<details>
<summary>Services and DNS. DNS service</summary>

  * Exposed Services automatically register with the Kubernetes internal DNS.
  * DNS Service is registered in Kubernetes by default. And this DNS service is updated every time a new service is added.
  
## Troubleshooting
If you want to understand why your Deployment isn't reachable - you need to check the label and selector you use, because label is a connection between Service and Deployment. If service dont see Deployment then DNS also cant point the proper IP-address.
</details>
 
<details>
<summary>Networking. How Kubernetes Networking works under the hood in minikube (but quite similar to public cloud K8s)</summary>
  
![image](https://user-images.githubusercontent.com/4239376/212553209-ba5f8d53-3e95-4da7-95fd-5b0381c188f3.png)

  * kube-apiserver - process which runs minikube. You can find it in your list of processes.
  * base cidr address pool for minikube - 10.98.0.0/12. It means all addresses until 10.111.255.255 will be in the same network.
  * mywebserver and nginx - Services; They are under one network, not different;
  * endpoints - how you get to the pods. Load-Balancing role is here.
  * 172.17.0.17 and 172.17.0.18 - Pod addresses
  * NodePort - port on host level, in our case - on minukube. NodePort is spreaded all over your Nodes. `32000` brings us to `nginx` Service, `31074` brings us to `mywebserver` Service.
  * ClusterIP - referring the Ip addresses within Services. It's because not Ip-addresses are accessible from the outside. Because there is no routing between 192.168.99.100 and `kube-apiserver`
  * LB on top - is LoadBalancing in Public Clouds (Azure, AWS) which navigates you to the proper `Service` on different `Nodes`.
  ![image](https://user-images.githubusercontent.com/4239376/212552710-9fae95d1-708a-477f-a199-b35127b5f24e.png)

</details>
  
# Ingress. Controller
 
<details>
<summary>NetworkPolicy. Ingress & Egress. Directions.  Connections. Managing Networking. Firewall between pods and namespaces. Web->Database pods Access, Example</summary>

## General info
  * A NetworkPolicy is like a firewall. 
  * By default, all pods can reach one another.
  * Network isolation can be configured to block traffic to pods by running pods in dedicated namespaces.
  * Between namespaces by default there is no traffic, unless routing has been configured.
  
## NetworkPolicy
  * `NetworkPolicy` can be used to block Egress as well as Ingress traffic, And it works like a firewall.
  * The use of Network Policy depends on the support from the network provider. Not all are offering support; not all are offering support and in that case your policy wont have any effect!
  * Connections in `NetworkPolicy` are stateful - allowing one direction is enough, another direction will be allowed automatically
  * `Labels` are used to define what policied applied to which resources
  
## NotworkPilicy directions: Egress and Ingress
  * If direction is declared in the manifest, but with no extra specifications - it means no extra limitations, both directions will be allowed by default.
  * if direction is listed and contains a specification - that specification will be used.
  
Pods with applied NetworkPolicy you can find here: [PODS WITH NETWORKPOLICY.](https://github.com/Glareone/Certified-Kubernetes-Application-Developer/blob/master/pods-with-nw-policy.yaml)
</details>
  
<details>
<summary>Ingress. When users connect indirectly. DNS. Basic slides</summary>
  
  * Users can connect services either directly or indirectly. If they wanna do that indirectly there is another component known as Ingress.
  * Ingress is about DNS name which is connected to a Service.
  
![image](https://user-images.githubusercontent.com/4239376/211929866-e96c184e-58ca-4df2-9a28-aaf71a54dcdc.png)
  
  * Another slide with Ingress and Service

![image](https://user-images.githubusercontent.com/4239376/213295177-7a0c2522-0b5b-45fd-a3bf-dc7cfb3ccd53.png)

</details>

<details>
<summary>General information. Ingress vs LoadBalancer. Ingress vs Service. Ingress in Public Cloud K8s</summary>

# General info
  * Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster.
  * Traffic routing is controlled by rules that are defined on the Ingress resource
  * Ingress can be configured to do multiple things. It gives services externally reachable URLs
  * It allows you to load balance traffic. 
  * It allows you to load balancing traffic between several Services
  * It can terminate SSL as well as TLS
  * Offers name based virtual hosting
  
# Ingress vs LoadBalancer
  * Ingresses are native objects inside the cluster that can route to multiple services, while load balancers are external to the cluster and only route to a single service.
  * On many cloud providers ingress-nginx will also create the corresponding Load Balancer resource. All you have to do is get the external IP and add a DNS A record inside your DNS provider that point myservicea.foo.org and myserviceb.foo.org to the nginx external IP. Get the external IP by running:
  
# Ingress vs Service
  * So where services take care of the basic exposure, Ingress really is what brings your application to the internet
  
# Creating Ingress. Ingress Controller
  * Ingress controller is required before creating your Ingresses. 
  * The Ingress controller is not available by default in Minikube. 
  * If you are using a Kubernetes in cloud, then Ingress controller will be available and it's just a drop down list that you will find in a cloud.
  
</details>
  
<details>
<summary>Controller. Types of Controllers. Ingress Controller</summary>
  
  * The Kubernetes controller manager is a daemon.
  * Kubernetes controller - it is a loop that watches the state of your cluster and makes changes as needed, always working to maintain your desired state
  * Controllers can track many objects including:
    - What workloads are running and where
    - Resources available to those workloads
    - Policies around how the workloads behave (restart, upgrades, fault-tolerance)
  
# Type of Controllers
  - ReplicaSet
  - StatefulSet
  - Job
  - CronJob
  - DaemonSet
  - Ingress Controller
  
# Ingress Controller. Types
  - nginx (ingress-nginx): https://github.com/kubernetes/ingress-nginx
  - haproxy https://haproxy-ingress.github.io/docs/getting-started/
  - traefik https://doc.traefik.io/traefik/providers/kubernetes-ingress/
  - kong https://docs.konghq.com/kubernetes-ingress-controller/latest/
  - contour (comparison): https://joshrosso.com/docs/2019/2019-04-12-contour-ingress-with-envoy/
  
</details>
  

<details>
<summary>How to work with Ingress. Configuring Ingress.</summary>
  
  # Check Tips and Tricks section and you will find comprehensive example for Ingress: https://github.com/Glareone/Kubernetes-for-developers/edit/main/README.md#exam-tip--tricks
</details>

<details>
<summary>Ingress rules. Inbound Traffic. Regular expression. Simple Fanout. Name based virtual hosting</summary>

## Rules consist of several things:  
* Optional host: if host is not specified - then rule applies to all Inbound traffic;
* List of paths like `/testpath`. Each path has it's own backend. You can use Regular Expression here.
* Backend: consist of ServiceName and ServicePort. It matches K8s API Service Objects. You also may configure default Backend for incoming traffic if the current path doesnt match to anyone.

## Backend types in opposite to Ingress Backend:
* Simple Ingress  
![image](https://user-images.githubusercontent.com/4239376/214406315-0915c404-4fce-48e5-ae55-aea798898a4c.png)  
* Simple Fanout: A fanout configuration routes traffic from a single IP address to more than one Service, based on the HTTP URI being requested.  
![image](https://user-images.githubusercontent.com/4239376/214402840-8496d3a9-0f07-4702-aa5b-94112d31cf48.png)  
![image](https://user-images.githubusercontent.com/4239376/214406611-da32fb07-d0fe-4141-b29a-3a310c70f7b1.png)  


* Name based virtual hosting: traffic is coming to the specific route (subdomain, as example)  
![image](https://user-images.githubusercontent.com/4239376/214403275-cfdf87d4-f371-4c88-a66a-329597949513.png)  
![image](https://user-images.githubusercontent.com/4239376/214407005-6bcf8319-2261-42dc-b0e9-aac791a888f3.png)  
The third path in example - is generic path. You send traffic which does not match to any other path there.

* TLS Ingress: Ensure that TLS termination is happened at the Load Balancer level. You also can secure an Ingress by specifying a Secret that contains a TLS private key and certificate. 
</details>
  
# Kubernetes Persistent Storages. PV Persistent Volumes. PVC Persistent Volume Claims. Azure Shared Disks

<details>
<summary>Volume Types. Pod Volumes. ConfigMap. Secrets. azureDisk, hostPath, awsElastickBlockStore, gcePersistentDisk. NFS. ISCSI</summary>
  
## Basics
The PV is persistent volume and this persistent volume goes to external storage and this external storage as we will see shortly can be anything. And the nice thing is you can have multiple PVs available pointing to different external storage solutions. So they don't even have to be the same external storage, it can be anything. Now, the PVs are independent objects in the Kubernetes environment. And in order to work with the PV, it is the PVC, the persistent volume claim. In a persistent volume claim, the pod can use a persistent volume claim. The persistent volume claim is a request for storage and this requests for storage is only asking for a specific size and a specific type. So that can be like I need two terabytes of ReadWritemany.
  
![image](https://user-images.githubusercontent.com/4239376/214425826-3a98849c-88e9-4f34-87bb-986ae6f0074e.png)

## PV Example:
  ![image](https://user-images.githubusercontent.com/4239376/214426068-3f4c5e60-3d49-4822-b72b-91904b1dda6f.png)  
* hostPath: PV uses hostPath a storage solution. In opposite to emptyDir, but hostPath is persistent, emptyDir is temporary.
* Created PersistentVolume with hostPath means this storage will still be there when Pods which use it are gone.
  
## Volume Types:
  * emptyDir: creates a temporary directory on the host
  * hostPath: persistently connects to host environment
  * azureDisk: Azure Cloud Storage
  * awsElasticBlockStore: aws cloud storage
  * gcePersistentDisk: GCP cloud storage
  * iscsi: ISCSI SAN Storage (disk)
  * ngs: Network File System storage
  
![image](https://user-images.githubusercontent.com/4239376/214413163-31cb9d97-5fa7-4e46-b09e-39d36b736992.png)

  * Files stored in a container will only live as long as the container itself
  * Pod Volumes - can be used to allocate storage that outlives the container and stays available during pod lifetime
  * PV allows Pod to connect to external storage and can exist outside of Pods.
  * to use PV you need to use PVC. They request access to specific PV based on your request and make binding between POD and PV.
  * ConfigMap - is a special case of storage. Best way to provide dynamic data within a Pod;
  * Secret - do the same as ConfigMap, but they encoding the data they contain. Encoding is not the same as encrypting, but it just scrambles the data so that on first sight it is not readable, but for anyone who knows the basics for utility, it's very easy to dig out the data in a secret
  
</details>
  
<details>
<summary>How to decide what Volume to use. Pod Volume Example. PV Example</summary>
  
![image](https://user-images.githubusercontent.com/4239376/214417312-e8bd470e-7b36-472a-8639-db839dc65af3.png)
  
## Pod Volume mount example:
  ![image](https://user-images.githubusercontent.com/4239376/214417935-1d0ca47e-3a32-4279-8fd1-8687c817f279.png)

## PV Example:
  ![image](https://user-images.githubusercontent.com/4239376/214426068-3f4c5e60-3d49-4822-b72b-91904b1dda6f.png)  
* hostPath: PV uses hostPath a storage solution. In opposite to emptyDir, but hostPath is persistent, emptyDir is temporary.
* Created PersistentVolume with hostPath means this storage will still be there when Pods which use it are gone.
</details>

<details>
<summary>PV and PVC relation. Binding</summary>

  ![image](https://user-images.githubusercontent.com/4239376/215288222-e055af91-1b94-4836-8c1b-f84125d7bfc1.png)
  - PV `type` - nobody cares about declaring pv type, it could be anything. The only important thing is how you configure PVC
  - PV and PVC both have same `accessMode`. And this is really what connects PVC to PV and lead to state of `BOUND`.
  - You may connect PV and PVC which have only identical `accessMode`, it's a glue between PV and PVC
  
  ![image](https://user-images.githubusercontent.com/4239376/215288332-08e1da3c-e4fe-4014-bf1a-1f9cd201f6b6.png)  
  - using `claimName` name you creates a binding between POD and PVC.
  
* And that's how Kubernetes provides decoupling
  ![image](https://user-images.githubusercontent.com/4239376/215288437-9ca2460e-600e-4911-9154-93525dd9b068.png)
</details>
  
<details>
<summary>PVC. Persistent Volume Claims. Connect Pod to PVC.</summary>

* `kubectl get pvc` - to get all pvcs:  
![image](https://user-images.githubusercontent.com/4239376/215287667-7cf1e595-0f86-45d9-b669-656ea228b3b6.png)
  
* `kubectl get pv` - after applying `kubectl create -f pvc.yaml` from previous example we see that pv and pvc binding created   
* PVC requested 1 GiB. PV has 2GiB in total:  
  ![image](https://user-images.githubusercontent.com/4239376/215287756-49f32d8d-b794-4d42-9c9c-7b27b795fd5c.png)

* we can connect Pod to PVC:  
  ![image](https://user-images.githubusercontent.com/4239376/215287841-5a3428f7-4b23-44c5-888d-6e46d8120336.png)
  - `pv-storage` - the name of PV. In volume mount we declare only PV volume name. In Pod spec we declare mount and claim for this mount (PVC claim)  
  - `pv-claim` - is the name of attached PVC we created above
</details>
  
<details>
<summary>PV-PVC Demo using NFS PV</summary>

  * PV config  
  ![image](https://user-images.githubusercontent.com/4239376/215288767-328bddc2-ac37-493b-b50e-9efb8284486c.png)  

  * to configure NFS storage you need `nfs-utils` in linux and some linux-related + firewall configuration to give readWrite access to nfs server and storage  
  * PVC config  
  ![image](https://user-images.githubusercontent.com/4239376/215288944-469a546b-0d31-4e49-91be-e50bcfbec475.png)   

  * Pod config with 2 containers each of them connects to PV using PVC   
  ![image](https://user-images.githubusercontent.com/4239376/215289151-bd98c953-5120-40be-bb0e-2b08352da2cd.png)  

  
</details>

  
  
<details>
<summary>Kubernetes Persistent Storages. Volumes. Azure Shared Disks</summary>

![image](https://user-images.githubusercontent.com/4239376/197339361-f2862df2-ac3b-461d-aa31-80cb1077c911.png)  
Article: https://learn.microsoft.com/en-us/azure/aks/concepts-storage

* Nowadays, you may start sharing Azure Shared between nodes and pods (but only for Premium disks):
https://stackoverflow.com/questions/67078009/is-it-possible-to-mount-a-shared-azure-disk-in-azure-kubernetes-to-multiple-pods  
![image](https://user-images.githubusercontent.com/4239376/197342695-cb7217d0-20c0-4021-8f1c-fea066b0ef0b.png)

Traditional volumes are created as Kubernetes resources backed by Azure Storage. You can manually create data volumes to be assigned to pods directly, or have Kubernetes automatically create them. Data volumes can use: Azure Disks, Azure Files, Azure NetApp Files, or Azure Blobs.

</details>

# Exam. Tip & Tricks.
  
Lots of Service \ NetworkPolicy \ Deployment examples you could find here: [HUGE PACK OF REAL EXAMPLES](https://github.com/Glareone/Certified-Kubernetes-Application-Developer) 

<details>
<summary>How to get initial deployment file</summary>

You may use `kubectl run` and then export deployment to yaml file, change it and use `kube apply`
 ![image](https://user-images.githubusercontent.com/4239376/204347366-d6385bc2-0d9e-4b0d-8d75-818a2d010536.png)

* You may use `kubectl create deployment --help` - this command shows several good examples how to create deployment. together with `--dry-run=client` it might be a good fit for declarative deployment creation
  
### Working example:
  
`kubectl create deployment httpd-test-deployment-2 --image httpd -n new-httpd-test-namespace --replicas=2 --dry-run=client -o yaml > httpd-test-deployment-2.yaml`  
`kubectl create deployment nginx-rollout-deployment --image=nginx:1.19 --dry-run=client -o yaml > nginx-rollout-deployment.yaml`  
will give you the following:  
![image](https://user-images.githubusercontent.com/4239376/207677288-95ee2405-dcaf-4521-b7db-a886b4341f71.png)

</details>
  
<details>
<summary>Apply deployments</summary>
  
`kubectl create -f <YOUR_YAML_FILE>` or `kubectl apply -f <YOUR_YAML_FILE>`  
</details>
  
<details>
<summary>Create namespace using declarative way</summary>
  
`kubectl create ns production -o yaml` - will create a namespace and show yaml structure it uses. 
  we may copy the output and using vim put it into yaml file, then use it for Namespace creation.
  
`kubectl delete namespaces production`  - to delete already created namespace
  
`kubectl create ns production -o yaml > ns-file.yaml` is also applicable
  
  * to avoid any creation we may use `dry-run` like in the following example `kubectl run --generator=run-pod/v1 nginx-prod --image nginx -o yaml  -dry-run=true > file.yaml`.
</details>

<details>
<summary>Create namespace cronjob</summary>
  
* You may use `kubectl create cronjob --help and take example of job` or 
it suggests the following:    
  `kubectl create cronjob test-job --image=busybox --schedule="*/1 * * * *"`  
  
* you may use example from https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/
  
</details>
  
<details>
<summary>Create your Service with kubectl expose deployment. Get Services</summary>

## Create Service and it's yaml file.
**FIRST OF ALL YOU NEED DEPLOYMENT YOU WILL EXPOSE**  
* useful command to get Service is to use the following command on exam:   
`kubectl expose deployment <YOUR-DEPLOYMENT> --port=80 --type=NodePort`;  
After applying this command you will see that your Service is deployed instantly. So better to use it with `--dry-run=client` flag:    
`kubectl expose deployment <YOUR-DEPLOYMENT> --port=80 --type=NodePort --dry-run=client -o yaml > <YOUR_SERVICE.yaml>` - dry run for your service.  
  
* Create service: `kubectl create -f <YOUR_SERVICE.yaml>` - to create service from yaml file.
  
PS Notice, this comman allocates a random portal on all backend nodes, so if you want to be in control of used Port you need to use `targetPort` to define the port.
## Get Services
`kubectl get svc` - to get Services.  
`kubectl get svc nginx -o yaml` - will show Service specifics in yaml.
`kubectl get svc nginx -o yaml > my-service.yaml` - Service specifics in yaml to file  

![image](https://user-images.githubusercontent.com/4239376/212551153-bf7a65fd-65d7-49f8-ae33-f7996c3c6b88.png)
![image](https://user-images.githubusercontent.com/4239376/212551721-9ce115e8-9645-4b2e-b553-f3bb6503d247.png)
</details>
  
<details>
<summary>Configure a Service for Deployment. ClusterIP & NodePort</summary>
  Exercise: Configure a service for the Nginx deployment you've created earlier. I don't really care which Nginx deployment you use, as long as you make it accessible. And ensure that the service makes Nginx accessible through port 80, using the ClusterIP type. And also, verify that it works. After making the service accessible this way, change the type to NodePort and expose the service on port 32000. And verified the service is accessible on this node port.  
  
  * `kubectl create deployment nginx-lab-for-service-deployment --image nginx --dry-run=client -o yaml > nginx-lab-for-service-deployment.yaml`  - deployment in fine   
  * `vim nginx-lab-for-service-deployment.yaml` - if you need to tune your deployment   
  * `kubectl create -f nginx-lab-for-service-deployment.yaml` - apply deployment   
  * `kubectl expose deployment nginx-lab-for-service-deployment --type=ClusterIP --port=80 --dry-run=client -o yaml > nginx-lab-for-service-service.yaml` - create Service for your deployment. Rename Service if needed (because now its name has -deployment suffix  
  * `kubectl create -f nginx-lab-for-service-deployment.yaml` -- apply Service  
  * `kubectl describe svc nginx-lab-for-service-deployment` - check that service is working.  
  
![image](https://user-images.githubusercontent.com/4239376/212557873-b6910b95-fc89-427f-b295-8640d209bfd6.png)
  
  * You can verify and endpoints is assigned.  
  * `kubectl get endpoints` - shows exposed endpoints  
  
![image](https://user-images.githubusercontent.com/4239376/212557996-6e57c3dc-a886-41e2-ae05-3b9511d8efb2.png)

  * `kubectl edit svc nginx-lab-for-service-deployment` - edit deployed service
  * replace `targetPort` with `nodePort` because targetPort belongs to ClusterIP type, but we need NodePort Service type now
  
  ![image](https://user-images.githubusercontent.com/4239376/212558640-768641be-6ac3-4d0c-a209-901da33bef50.png)
  
  * changes applied: ![image](https://user-images.githubusercontent.com/4239376/212558660-2ba07036-f9f4-4e5e-9132-853b4b7b0580.png)

  PS: It will be reachable from internal network because of Type. In order to make it reachable from outer world you need to use LoadBalancer!!
</details>
  
<details>
<summary>Ingress. Expose Deployment and Service NodePort outside using Ingress</summary>
  
 # How to 
  0. get initial Ingress Controller example from documentation: https://kubernetes.io/docs/concepts/services-networking/ingress/
    - save in yaml file using `vim test-ingress.yaml`
  1. `kubectl create deployment <YOUR_DEPLOYMENT_NAME>` -  Create your deployment with replicaSet >= 2
  2. `kubectl scale deployment <YOUR_DEPLOYMENT_NAME> --replicas=3` - scale number of pods
  2. `kubectl expose deployment <YOUR_DEPLOYMENT_NAME> --type=NodePort --port=80` - Create Service for deployment. In our case we need NodePort because ClusterIP is useless in this case
  3. `kubectl get svc` - verify we have created Service
  ![image](https://user-images.githubusercontent.com/4239376/213303596-9af863fb-a34c-4d49-b5ca-31b3d2e49d34.png)
  Keep in mind that we dont care of CLUSTER-IP value, we care only about Port because port addresses the Server to Deployment. It means you can reach the Service (and Deployment) using `curl http://<KUBERNETES_CLUSTER_IP>:<YOUR_PORT_ASSIGNED_TO_SERVICE: 32618>`
  4. fix initial Ingress example. Need to change `service: name: test` to our `service: name: <NAME_OF_SERVICE>`
  5. fix initial Ingress example adding host name. Better to start with `kubectl explain ingress.spec.rules` and check `host`
    - you can find the final file in the current repo
  6. create ingress using `kubectl create -f test-ingress.yaml`
  7. `kubectl get ingress  --all-namespaces` - check that ingress created.
  8. If you run it in minicube and locally you may reach your service updating `hosts` file and send request from your address to your local ip address
  9. `curl http://<YOUR_HOSTNAME>.com/testpath` - and voila! 
  
  ![image](https://user-images.githubusercontent.com/4239376/213306479-a1604a8c-7a3e-4811-8d0e-5cdcf7dcf074.png)

# Details
  * Applying the yaml you may find in my root folder, ingress resources will be created and managed by the ingress-nginx instance. Nginx is configured to automatically discover all ingress with the kubernetes.io/ingress.class: "nginx" annotation or where `ingressClassName: nginx` is present. 
  * Please note that the ingress resource should be placed inside the same namespace of the backend resource.
</details>
  
<details>
<summary>Update Deployment. Change Replica set</summary>
  
  You can easily use one of the following commands:
  * `kubectl edit deployment <YOUR_DEPLOYMENT_NAME>` - edit and save changes
  * `kubectl scale deployment <YOUR_DEPLOYMENT_NAME> --replica=3` - scale out your pods number 
</details>
