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
### When Kubernetes is worth?
* New devs onboarding.
* You can eliminate Application conflicts. (You can run multiple versions of one application simultaneously)

<details>
<summary>Kubernetes application instance creation diagram</summary>
  
  ![MicrosoftTeams-image](https://user-images.githubusercontent.com/4239376/188572077-42c51924-f2de-4173-8837-b26bb5d9d2a3.png)
</details>

## Kubernetes PODS
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
    
## Kubernetes building Blocks

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

## Running Kubernetes Locally

<details>
<summary>Options to run Kubernetes locally:</summary>

1) minikube (little version of K8s, but with full list of abilities from the full version) - but should have only one master node
2) docker desktop
3) kubernetes in docker (kind) - install kubernetes right in docker desktop application. and you can use all commands from kubectl
4) kubeadm - full version of k8s running locally
  
</details>

## Kubernetes User. RBAC. Authorization Authentication
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
  
kubectl version  
kubectl cluster-info  
kubectl gel all (retrieve all inf about pods, deployments...)  
kubectl run [cont-name] --image=[image-name]  
kubectl port-forward [pod] [ports] - configure your proxy to expose your POD.  
kubectl expose (expose your ports)  
kubectl create [resource] - create resource in k8s based on yml file  
kubectl apply [res] - create or MODIFY EXISTING  
</details>
  
# Kubernetes Persistent Storages. Volumes. Azure Shared Disks

![image](https://user-images.githubusercontent.com/4239376/197339361-f2862df2-ac3b-461d-aa31-80cb1077c911.png)  
Article: https://learn.microsoft.com/en-us/azure/aks/concepts-storage

* Nowadays, you may start sharing Azure Shared between nodes and pods (but only for Premium disks):
https://stackoverflow.com/questions/67078009/is-it-possible-to-mount-a-shared-azure-disk-in-azure-kubernetes-to-multiple-pods  
![image](https://user-images.githubusercontent.com/4239376/197342695-cb7217d0-20c0-4021-8f1c-fea066b0ef0b.png)

Traditional volumes are created as Kubernetes resources backed by Azure Storage. You can manually create data volumes to be assigned to pods directly, or have Kubernetes automatically create them. Data volumes can use: Azure Disks, Azure Files, Azure NetApp Files, or Azure Blobs.

# Deployment strategies. Blue-green, Canary, A\B, Multi-service
 ![image](https://user-images.githubusercontent.com/4239376/197362803-243e0580-737f-4042-8cf0-1ed7ab0173c8.png)
 
# Exam. Tip & Tricks.

<details>
<summary>How to get initial deployment file</summary>

You may use `kubectl run` and then export deployment to yaml file, change it and use `kube apply`
 ![image](https://user-images.githubusercontent.com/4239376/204347366-d6385bc2-0d9e-4b0d-8d75-818a2d010536.png)
</details>


