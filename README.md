# 🚀 kubernetes-cluster-management
Deployed and managed applications on Kubernetes with auto-scaling and load balancing.

### 🧩 Tech Stack
- Kubernetes
- kubectl
- YAML
- AWS EKS

### 📝 Implementation Steps

1. Set up kubernetes Cluster on AWS EKS.
2. Create deployment and service manifests.
3. Configure horizontal pod autoscaling.
4. Monitor cluster health and performance.

> 1. Set up kubernetes Cluster on AWS EKS

For setting up a Kubernetes cluster on EKS, we will use eksctl, a third-party tool that AWS now recommends for creating and managing clusters.
Download the latest version of 'eksctl' from https://github.com/eksctl-io/eksctl 

After installing eksctl check if it's properly installed and working by checking its version.

**`eksctl version`**

<img width="437" height="125" alt="Screenshot 2025-09-04 150744" src="https://github.com/user-attachments/assets/71f4ef87-de7c-4637-b1d3-b1603fcdcb2e" />

Before creating an EKS cluster, we need to configure AWS credentials on your local system. This allows eksctl and the AWS CLI to authenticate with your AWS account and communicate with services such as EKS, EC2, IAM, and VPC.

AWS CLI Download link: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

In AWS Cloud, create an IAM user and generate an Access Key and Secret Access Key, which will be used to configure authentication for the AWS CLI and eksctl.
Open a terminal or command prompt (for example, Git Bash on Windows or Terminal on Linux/Mac). Run the following command to set up your AWS credentials:

**`aws configure`**

You will be prompted to enter:
- AWS Access Key ID
- AWS Secret Access Key
- Default region name (e.g., ap-south-1)
- Default output format (e.g., json)
- This will save your credentials in ~/.aws/credentials and ~/.aws/config, which are automatically used by the AWS CLI, eksctl, and kubectl during EKS cluster setup.

After launching the EKS cluster, you will need kubectl to manage and interact with it.

Download the latest kubectl binary: https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
To run kubectl from anywhere in your terminal, ensure the directory where you placed the binary is included in your system’s PATH environment variable.

### Creating EKS Cluster
Creating the ekscluster using a yaml file
**`eksctl create cluster -f myekscluster.yml`**

<img width="1068" height="648" alt="Screenshot 2025-09-03 162819" src="https://github.com/user-attachments/assets/6f0bee13-2e08-4dee-b6fa-8437b5799c0d" />
<img width="1920" height="1020" alt="Screenshot 2025-09-03 164439" src="https://github.com/user-attachments/assets/73f51721-f6de-428f-a659-63d0cf1bec80" />

To confirm if the setup is ready, run the below command

`kubectl get pods` <br>
`kubectl get nodes`

<img width="1919" height="546" alt="Screenshot 2025-09-03 165830" src="https://github.com/user-attachments/assets/a6b208cb-ffd8-47d8-af59-812d0d7c55ba" />

### Creating Deployment
In Kubernetes, a Deployment is used to manage and update Pods (containers) in a controlled way. 
It ensures that the desired number of Pod replicas are running and can automatically handle rolling updates and rollbacks.

> kubectl create deployment mywebd --image=vimal13/apache-webserver-php

To check the deployement status:
> kubectl get deployment

<img width="1917" height="843" alt="Screenshot 2025-09-03 170055" src="https://github.com/user-attachments/assets/dcf5b1e1-df96-4cd7-bf5b-73f7e952b0b7" />

Now the pod is lauched using deployment
`kubectl get pods`

<img width="1494" height="419" alt="Screenshot 2025-09-03 170155" src="https://github.com/user-attachments/assets/f0f10d20-6a0a-4c46-a677-ef753258179d" />

To check the events of a pod, we can use the kubectl describe pod <pod-name> command, which provides detailed information about the pod including status, conditions, and events.

`kubectl describe pods`

<img width="1920" height="1020" alt="Screenshot 2025-09-03 170229" src="https://github.com/user-attachments/assets/9f08a325-417d-4696-afaa-e61895b497a2" />

<img width="1917" height="368" alt="Screenshot 2025-09-03 170405" src="https://github.com/user-attachments/assets/5cdb3ef8-af35-4e6f-a1db-2907bc58d1d9" />

### To check which node a pod is running on, use the kubectl get pod <pod-name> -o wide command.

`kubectl get pods -o wide`

<img width="1919" height="501" alt="Screenshot 2025-09-03 170641" src="https://github.com/user-attachments/assets/849fe2ab-e69d-4d94-bcfd-80d066825c0c" />

## 📊 Checking Pod Metrics

We can check the CPU and memory usage of pods using the kubectl top pod command. This helps monitor resource consumption and troubleshoot performance issues.

`kubectl top pod`

<img width="1330" height="469" alt="Screenshot 2025-09-03 170758" src="https://github.com/user-attachments/assets/82c52ae9-58d7-4621-b4ef-133cde9926db" />

- 1m means 1 milliCPU, i.e., 0.001 of a CPU core is been used.
- 11Mi means 11 Mebibytes (approx 11 MB) of RAM is been used.

## 📡Exposing the Deployment

- By default, pods in Kubernetes are not accessible from outside the cluster. 
- To make them reachable, we expose a Deployment using a Service. 
- A Service provides a stable IP address and DNS name, and can also load-balance traffic across multiple pod replicas.

We can expose a deployment with the kubectl expose command:

`kubectl expose deployment mywebd --type LoadBalancer --port 80`

<img width="1459" height="393" alt="Screenshot 2025-09-03 170931" src="https://github.com/user-attachments/assets/c68331d0-5081-46dd-8ed8-82efd1baf6f9" />

- --type LoadBalancer → makes it accessible externally (in cloud environments like AWS EKS, GKE, AKS). In my case I am using AWS.

- --port 80 → the port exposed to the outside world.

## 🌐Checking Services

After exposing a deployment, you can list the services in your cluster using:

`kubectl get svc`

<img width="1919" height="483" alt="Screenshot 2025-09-03 172017" src="https://github.com/user-attachments/assets/dea7c0b2-8179-45ee-b45b-f17c7d5838df" />

To check detailed information about a specific Service:

`kubectl describe svc mywebd`

<img width="1920" height="1020" alt="Screenshot 2025-09-03 172041" src="https://github.com/user-attachments/assets/a17c0a73-42eb-4129-8527-c4f5eb12d291" />

## 📈 Scaling Pods manually

- Scaling in Kubernetes means adjusting the number of pod replicas running for a deployment. 
- This helps your application handle more traffic (scale up) or reduce resources when demand is low (scale down).

To scale a deployment manually:
`kubectl scale deployment mywebd --replicas=5`

<img width="1079" height="396" alt="Screenshot 2025-09-03 171150" src="https://github.com/user-attachments/assets/34b90266-2b15-43bd-a734-ab6aa13e2e08" />
<img width="1593" height="608" alt="Screenshot 2025-09-03 171211" src="https://github.com/user-attachments/assets/c4b53d22-4e19-4751-b3f8-61e164a98db9" />


