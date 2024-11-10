<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

# Build and Deploy a Simple Guestbook Application
<!-- TABLE OF CONTENTS -->
<details open>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#introduction">Introduction</a></li>
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
        <li><a href="#build-and-deploy-the-v1-application-to-kubernetes">Build and deploy the v1 application to Kubernetes</a></li>
        <li><a href="#autoscale-application-using-hpa">Autoscale application using HPA</a></li>
        <li><a href="#rolling-updates-and-rollbacks">Rolling Updates and Rollbacks</a></li>
      </ul>
    </li>
    <li><a href="#getting-started">Getting Started</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>
<br>


## Introduction
This repository was created as part of IBM's "Introduction to Containers, Kubernetes and OpenShift" course.<br>
It's a project about Kubernetes using Docker.<br>
Much of the code was cloned from the IBM repository: https://github.com/ibm-developer-skills-network/guestbook<br>
<br>
Guestbook is a simple web application that is built and deployed with Docker and Kubernetes.<br>
The application consists of a web front end which will have a text input where you can enter any text and submit.<br>
Kubernetes Deployments and Pods are created for all of these.<br>
Then Horizontal Pod Scaling is applied to the Guestbook application.<br>
Rolling Updates and Rollbacks are performed as well.<br>

![adding entries application](https://github.com/user-attachments/assets/99d22cfb-7513-43ef-8db9-3ed624de4ecd)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
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
- Project Goal: Build and deploy an application to Kubernetes. Gain a deeper understanding of Autoscaling (Horizontal Pod Autoscaler) and Rolling Updates + Rollbacks.
- Number of Project Participants: 1 (Cloned repository of IBM. Developed the rest on my own)
- Time Period: November, 2024
- Industry / Area: DevOps
- Role: Developer
- Languages: English
- Result: Application successfully built and deployed. Improved understanding of Autoscaling, Rolling Updates and Rollbacks.
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

### Build and deploy the v1 application to Kubernetes
Setup of the environment variable namespace (user name is already predefined in IBM Cloud IDE):<br>

![set namespace](https://github.com/user-attachments/assets/e9260c81-83ab-457d-9548-b85a9dc8a290)

Missing commands have been added to the Dockerfile in ./v1/guestbook:<br>

![fill missing dockerfile commands](https://github.com/user-attachments/assets/3dd1b5cd-75db-437c-9514-554e58159e3a)

The image was then built using the Dockerfile.<br>

```
docker build . -t us.icr.io/$MY_NAMESPACE/myguestbook:v1
```

The -t flag was used to set the name and tag of the image according to the IBM course naming convention.<br>
Result:<br>

![verification docker images](https://github.com/user-attachments/assets/58b0e5a7-ee59-4be5-8320-5f077c98c6f7)

After the image has been built, it is pushed into the IBM Container Registry:<br>

```
docker push us.icr.io/$MY_NAMESPACE/myguestbook:v1 
```

Result:<br>

![verification cloud cr images](https://github.com/user-attachments/assets/52e36c2f-f92c-4de0-9dab-9082925d5149)

The K8s object deployment is then created.<br>
Here the placeholder of the namespace must be replaced with my namespace:<br>

![insert namespace](https://github.com/user-attachments/assets/cde3e4a1-5115-4bd7-889f-144e5c057b1d)

Apply the deployment:<br>

```
kubectl apply -f deployment.yml
```

Verification of the deployment:<br>

![verification deployment](https://github.com/user-attachments/assets/a6ee6f54-f74d-4a66-9d6a-fd8f0e193b59)

Now start application on port-forward:<br>

```
kubectl port-forward deployment.apps/myguestbook 3000:3000 
```

Port forwarding in Kubernetes is a mechanism that allows you to access services running within a Kubernetes cluster from your local machine or another external system.<br>
It's useful for debugging, testing, and accessing services during development.<br>
<br>
Output of application:<br>

![output application](https://github.com/user-attachments/assets/cbe76944-8ea4-4280-bee3-137d2d1b5590)

It is possible to add guests or entries in this application.<br>
Fill in the text field, press the "Submit" button and the entry is added and appears above the text field:<br>

![adding entries application](https://github.com/user-attachments/assets/c595b354-f727-40d6-83da-9e40baa95876)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Autoscale application using HPA
A Horizontal Pod Autoscaler with the following properties is applied to the previously created deployment object "myguestbook":
- CPU Percent: 5%
- Minimum number of pods: 1
- Maximum number of pods: 10

This results in the following command:<br>

```
kubectl autoscale deployment myguestbook --cpu-percent=5 --min=1 --max=10
```

The created HPA:<br>

![hpa created](https://github.com/user-attachments/assets/ce07caeb-6ae6-4af5-8fc1-b34ace843ee1)

However, there is currently no load.<br>
In the IBM project, the following command is used to generate load so that the effect of the HPA can be seen (URL comes from the Kubernetes generated application. Port forwarding must also be active for this - see section "Build and deploy the v1 application to Kubernetes" above):<br>

```
kubectl run -i --tty load-generator --rm --image=busybox:1.36.0 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- https://christians21-3000.theiaopenshiftnext-1-labs-prod-theiaopenshift-4-tor01.proxy.cognitiveclass.ai/; done"
```

The following command can now be used to check the status of the HPA:<br>

```
kubectl get hpa myguestbook --watch
```

Result:<br>

![autoscale watch](https://github.com/user-attachments/assets/2460ad1c-4a2f-42c9-9793-b9cf4ca2a61a)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Rolling Updates and Rollbacks
Changes are now being made to the current version (v1).<br>
The updated version will then be deployed.<br>
Changes:
- Title and h1 in index.html are adjusted (e.g. "Guestbook - v1" -> "Guestbook - v2")
- CPU in deployment.yml is adjusted (e. g. 50m -> 5m)
<br>
The image is rebuilt and pushed into the IBM Container Registry.<br>
In addition, changed deployment.yml is applied.<br>
If the application is made available with the help of port-forwarding and visited, the new change is visible:<br>

![output application v2](https://github.com/user-attachments/assets/7bb2029d-8080-447d-b325-efd57f44de6e)

The application has been updated. A rollback of the deployment object is now being performed.<br>
The following command is executed to view the deployment rollout history:<br>

```
kubectl rollout history deployment/myguestbook
```

Result:<br>

![history rollouts](https://github.com/user-attachments/assets/13c28567-53da-404c-91d7-07d1c1f73b7f)

To obtain more information about the version 1 deployment object (e.g. replica sets used), the following command is executed:<br>

```
kubectl rollout history deployments myguestbook --revision=1
```

Result:<br>

![information about rollout version 1](https://github.com/user-attachments/assets/a24fca51-949d-4b08-9d2f-c47483cc4960)

If you compare the hash above with the hash of the “kubectl get rs” command, it becomes clear that the ReplicaSet of version 1 is not active.<br>
Instead, the ReplicaSet of version 2 is active.<br>

![replicasets rollout v2](https://github.com/user-attachments/assets/b33158db-a66c-42f7-a9b9-0f4c29baf2c0)

The version / revision is set to 1 again:<br>

```
kubectl rollout undo deployment/myguestbook --to-revision=1
```

ReplicaSet version 2 is scaled down and ReplicaSet version 1 is scaled up:<br>

![undo deployment version 2 rs](https://github.com/user-attachments/assets/45cb769f-8034-4900-b274-5c61e0877058)

Conclusion: Rollback of the deployment object was carried out successfully.<br>
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
