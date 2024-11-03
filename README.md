<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

# Build and Deploy a Simple Guestbook Application
This repository was created as part of IBM's "Introduction to Containers, Kubernetes and OpenShift" course.<br>
It's a project about Kubernetes using Docker.<br>
Much of the code was cloned from the IBM repository: https://github.com/ibm-developer-skills-network/guestbook<br>
<br>
Guestbook is a simple web application that is built and deployed with Docker and Kubernetes.<br>
The application consists of a web front end which will have a text input where you can enter any text and submit.<br>
Kubernetes Deployments and Pods are created for all of these.<br>
Then Horizontal Pod Scaling is applied to the Guestbook application.<br>
Rolling Updates and Rollbacks are performed as well.<br>
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

### Autoscale application using HPA
xxx<br>
<p align="right">(<a href="#readme-top">back to top</a>)</p>
<br>
<br>

### Rolling Updates and Rollbacks
xxx<br>
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