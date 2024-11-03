<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

# Build and Deploy a Simple Health Application
This repository was created as part of IBM's "Introduction to Containers, Kubernetes and OpenShift" course.<br>
It's a project about Kubernetes using Docker.<br>
TODOOOOOO: Update README.md<br>
Much of the code was cloned from the IBM repository: https://github.com/ibm-developer-skills-network/guestbook<br>
<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#course-information">Course Information</a>
    </li>
    <li>
      <a href="#information-about-the-project">Information about the Project</a>
      <ul>
        <li><a href="#general">General</a></li>
        <li><a href="#tech-stack">Tech Stack</a></li>
      </ul>
    </li>
    <li>
      <a href="#what-i-have-done-as-part-of-the-project">What I have done as part of the project</a></li>
      <ul>
        <li><a href="#build-and-deploy-the-application-to-kubernetes">Build and deploy the application to Kubernetes</a></li>
        <li><a href="#set-up-a-configmap">Set up a ConfigMap</a></li>
        <li><a href="#create-a-daemonset">Create a DaemonSet</a></li>
        <li><a href="#expose-application-within-the-cluster">Expose application within the cluster</a></li>
        <li><a href="#create-and-manage-secrets">Create and manage secrets</a></li>
        <li><a href="#provide-storage-for-application">Provide storage for application</a></li>
      </ul>
    </li>
    <li><a href="#getting-started">Getting Started</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>
<br>



## Course Information
Title: Introduction to Containers, Kubernetes and OpenShift<br>
Type: Final Project<br>
Course Provider: IBM<br>
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>



## Information about the Project
### General
- Client: Myself
- Project Goal: Build and deploy an application to Kubernetes, then understand and create ConfigMaps, DaemonSets, Kubernetes Services, Secrets, and further explore Volumes & Persistent Volume Claims.
- Number of Project Participants: 1 (Cloned repository of IBM. Developed the rest on my own)
- Time Period: November, 2024
- Industry / Area: DevOps
- Role: Developer
- Languages: English
- Result: Application successfully built and deployed. Improved understanding of several objects in Kubernetes.
<br>

### Tech Stack
With regard to my role:
- Containerization Tool: Docker
- Container Registry: IBM Cloud Container Registry
- IBM Cloud IDE (based on Theia and Container)
- Container Orchestration Tool: Kubernetes
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>



## What I have done as part of the project
As a lot of the work was done in the terminal, it is not visible in the repository.<br>
I therefore explain my completed tasks in the project below.<br>
<br>

### Build and deploy the application to Kubernetes
Setup of the environment variable namespace (user name is already predefined in IBM Cloud IDE):<br>

![setup env var](https://github.com/user-attachments/assets/1614d19f-eb82-478a-8e7a-6a246604ca75)

The image was then built using the Dockerfile.<br>

```
docker build . -t us.icr.io/$MY_NAMESPACE/myapp:v1
```

The -t flag was used to set the name and tag of the image according to the IBM course naming convention.<br>
Result:<br>

![docker images](https://github.com/user-attachments/assets/71d10198-9fa1-4c18-9242-115451391641)

After the image has been built, it is pushed into the IBM Container Registry:<br>

```
docker push us.icr.io/$MY_NAMESPACE/myapp:v1 
```

Result:<br>

![ibmcloud cr images](https://github.com/user-attachments/assets/1c94e442-be79-4afc-b067-055765dcbf40)

The K8s object deployment is then created.<br>
Here the placeholder of the namespace must be replaced with my namespace:<br>

![deployment yml](https://github.com/user-attachments/assets/24280b7f-1651-4ace-a9d4-6d593c600d2d)

Apply the deployment:<br>

```
kubectl apply -f deployment.yml
```

Verification of the deployment:<br>

![kubectl pods deployment yml](https://github.com/user-attachments/assets/7a6c8bfb-be3b-45dc-acd3-1d2c51fb85da)

Now start application on port-forward:<br>

```
kubectl port-forward deployment.apps/myapp 3000:3000 
```

Port forwarding in Kubernetes is a mechanism that allows you to access services running within a Kubernetes cluster from your local machine or another external system.<br>
It's useful for debugging, testing, and accessing services during development.<br>
<br>
Output of application:<br>

![application output](https://github.com/user-attachments/assets/bca4a53b-a9d8-4392-ab07-e8aa32760795)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Set up a ConfigMap
To decouple environment-specific configuration from the container image, so that the application is easily portable.<br>
According to the task, literals are to be used in the practical project.<br>
These are defined with the flag '--from-literal'.<br>
This makes it possible to create a ConfigMap via CLI.<br>
<br>
Two environment variables should be created:<br>
- Key: server-url, Value: http://example.com
- Key: timeout, Value: 5000
<br>
This results in the following command:<br>

```
kubectl create configmap myapp-config --from-literal=server-url=http://example.com --from-literal=timeout=5000
```

Verification of the ConfigMap:<br>

![verification configmap](https://github.com/user-attachments/assets/d8aea7b7-a236-4950-bcc2-5efb95923cf4)

Data = 2, which means that two environment variables have been created. Everything fits.<br>
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Create a DaemonSet
DaemonSet is used to ensure that a pod runs on each node in the cluster, including the nodes where the application pods are deployed.<br>
Result: Enhanced availability and fault tolerance of the application through redundancy and load distribution.<br>
<br>
First, the daemonset.yaml file is adjusted.<br>
Here the placeholder of the namespace must be replaced with my namespace:<br>

![adjust daemonset](https://github.com/user-attachments/assets/274b76c2-f9dd-46af-a97f-1714595381a8)

Next, the daemonset.yaml file is applied:<br>

```
kubectl apply -f daemonset.yaml
```

Verification of the DaemonSet:<br>

![verification daemonset](https://github.com/user-attachments/assets/6145f823-170f-4ada-ac81-78e844988d7e)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Expose application within the cluster
K8s services are used to expose the application within the cluster.<br>
In this case: NodePort Service.<br>
NodePort exposes service on a port on all nodes in the cluster.<br>
<br>
First, service.yaml is applied:<br>

```
kubectl apply -f service.yaml
```

Verification of the service:<br>

![verification service](https://github.com/user-attachments/assets/86cc05a1-2b0f-4397-8158-7e42167df1d6)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Create and manage secrets
K8s Secrets are used to securely store sensitive information (such as passwords, tokens and SSH keys).<br>
As with ConfigMap, literals are used in the practice project to create Secrets.<br>
<br>
Two environment variables should be created:<br>
- Key: username, Value: myuser
- Key: password, Value: mysecretpassword
<br>
This results in the following command:<br>

```
kubectl create secret generic myapp-secret --from-literal=username=myuser --from-literal=password=mysecretpassword
```

Verification of the Secret:<br>

![verification secret](https://github.com/user-attachments/assets/9422c5e9-0c4c-4ffb-a22b-88643daf7223)

Again: Data = 2, which means that two environment variables have been created. Everything fits.<br>
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Provide storage for application
To provide storage for the application, two K8s objects are used:
- PersistentVolume (PV): Storage resource provisioned by an administrator in the cluster that exists independently of any Pod that might use it.
- PersistentVolumeClaim (PVC): Request for storage by a user or a Pod which consumes PersistentVolumes. PVCs provide a way for users to request the storage resources they need.
<br>
The definitions of the two objects can be found in the file volume-and-pvc.yaml.<br>
Both definitions are separated from each other by three dashes (---).<br>
<br>
PersistentVolume provides 1 gigabyte of storage and the PVC requests this 1 gigabyte of storage.<br>
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>


## Getting Started
The code was only executable with the help of the IBM Cloud IDE.<br>
The IDE had many necessary tools and accesses preconfigured. These include, for example, Kubernetes installation or IBM Cloud Container Registry.
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>



## Contact
If you have any questions, please feel free to reach out via email: christian-schwanse (at) gmx.net<br>
<p align="right">(<a href="#readme-top">back to top</a>)</p>