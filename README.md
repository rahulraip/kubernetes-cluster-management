# kubernetes-cluster-management
Deployed and managed applications on Kubernetes with auto-scaling and load balancing.

### Tech Stack
- Kubernetes
- kubectl
- YAML
- AWS EKS

### Implementation Steps

1. Set up kubernetes Cluster on AWS EKS.
2. Create deployment and service manifests.
3. Configure horizontal pod autoscaling.
4. Monitor cluster health and performance.

> 1. Set up kubernetes Cluster on AWS EKS

For setting up a Kubernetes cluster on EKS, we will use eksctl, a third-party tool that AWS now recommends for creating and managing clusters.
Download the latest version of 'eksctl' from https://github.com/eksctl-io/eksctl 

After installing eksctl check if it's properly installed and working by checking its version.
> eksctl version
<img width="437" height="125" alt="Screenshot 2025-09-04 150744" src="https://github.com/user-attachments/assets/71f4ef87-de7c-4637-b1d3-b1603fcdcb2e" />

Before creating an EKS cluster, we need to configure AWS credentials on your local system. This allows eksctl and the AWS CLI to authenticate with your AWS account and communicate with services such as EKS, EC2, IAM, and VPC.

AWS CLI Download link: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

In AWS Cloud, create an IAM user and generate an Access Key and Secret Access Key, which will be used to configure authentication for the AWS CLI and eksctl.
Open a terminal or command prompt (for example, Git Bash on Windows or Terminal on Linux/Mac). Run the following command to set up your AWS credentials:
> aws configure

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

