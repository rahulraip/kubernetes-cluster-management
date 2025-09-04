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
