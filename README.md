# Kubernetes for developers, tech leads and solution architects
This repository contains materials prepared by myself and collected over the internet regarding Kubernetes and how to properly use it in case you are a developer, a tech lead or a solution architect 

# Recommended video course:
[Pluralsight: Kubernetes for developers](https://app.pluralsight.com/library/courses/kubernetes-developers-core-concepts/table-of-contents)  
Video Course with a good intro about Docker and Docker runtime: [Oreilly CKAD: Certified Kubernetes Application Developer](https://learning.oreilly.com/videos/certified-kubernetes-application/9780136677628)

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
  
# Service. Ingress. Kubernetes networking. Exposing pods outside. Port forwarding

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
<summary>Ingress. When users connect indirectly. DNS</summary>
  * Users can connect services either directly or indirectly. If they wanna do that indirectly there is another component known as Ingress.
  * Ingress is about DNS name which is connected to a Service.
  
![image](https://user-images.githubusercontent.com/4239376/211929866-e96c184e-58ca-4df2-9a28-aaf71a54dcdc.png)
  
</details>
  
`kubectl expose` 
  
# Kubernetes Persistent Storages. Volumes. Azure Shared Disks

![image](https://user-images.githubusercontent.com/4239376/197339361-f2862df2-ac3b-461d-aa31-80cb1077c911.png)  
Article: https://learn.microsoft.com/en-us/azure/aks/concepts-storage

* Nowadays, you may start sharing Azure Shared between nodes and pods (but only for Premium disks):
https://stackoverflow.com/questions/67078009/is-it-possible-to-mount-a-shared-azure-disk-in-azure-kubernetes-to-multiple-pods  
![image](https://user-images.githubusercontent.com/4239376/197342695-cb7217d0-20c0-4021-8f1c-fea066b0ef0b.png)

Traditional volumes are created as Kubernetes resources backed by Azure Storage. You can manually create data volumes to be assigned to pods directly, or have Kubernetes automatically create them. Data volumes can use: Azure Disks, Azure Files, Azure NetApp Files, or Azure Blobs.

# Exam. Tip & Tricks.

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
