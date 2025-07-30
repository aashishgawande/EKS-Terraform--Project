<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/c231d86f-0f00-40ef-8a6b-e3178a43b21b" />


**AWS EKS Cluster with Terraform**

This project automates the provisioning of a fully functional Amazon Elastic Kubernetes Service (EKS) cluster on AWS using Terraform. It sets up the networking layer (VPC, subnets, route tables), EKS control plane, managed worker nodes (node group), and supporting resources like IAM roles and security groups.
________________________________________
**🔧 Key Components Breakdown**

**1. Terraform Infrastructure**
   
You are using Terraform to write Infrastructure as Code (IaC), which:

•	Ensures consistent and repeatable deployments

•	Makes infrastructure easier to version, review, and audit

Your .tf files are structured to define:

•	Providers (terraform.tf) – AWS region and version lock

•	Variables (variable.tf and terraform.tfvars) – to parameterize reusable infrastructure

•	Main Infrastructure (main.tf) – all major resources like VPC, IAM, EKS cluster, and worker nodes

________________________________________
**☁️ What the Project Does (Step-by-Step)**

**2. Provisioning the EKS Cluster**

**Terraform provisions:**

**•	VPC & Subnets**

o	Custom VPC with multiple public subnets

o	Route tables and IGW for internet access

**•	Security Groups**

o	Open ports for cluster API server, EC2 SSH, and node communication

**•	IAM Roles**

o	For the EKS control plane and EC2 worker nodes

o	Includes policies like AmazonEKSClusterPolicy, AmazonEKSWorkerNodePolicy, etc.

**•	EKS Cluster**

o	Launches the EKS control plane using aws_eks_cluster

o	Provides Kubernetes API access

**•	Managed Node Group**

o	Creates a scalable EC2-backed worker group using aws_eks_node_group

o	Runs Kubernetes workloads in your cluster
________________________________________
**🖥️ ec2.sh: Bootstrapping EC2 Instance**

This shell script automates EC2 setup for admins or CI/CD:

•	Installs AWS CLI, kubectl, eksctl, and Docker

•	Configures AWS credentials and default region

•	Updates kubeconfig to communicate with the EKS cluster

aws eks --region ap-south-1 update-kubeconfig --name EKS-ecommerse-cluster

This allows the user to run:

kubectl get nodes

To confirm if EKS and worker nodes are ready.
________________________________________
**📊 Input Variables (terraform.tfvars)**

The cluster is customizable through these values:

region         = "ap-south-1"

cluster_name   = "EKS-ecommerse-cluster"

node_group_name = "EKS-node-group"

instance_type  = "t2.micro"

desired_size   = 2

max_size       = 3

min_size       = 1

**You can change:**

•	Cluster name

•	Region

•	Worker node scaling settings

•	EC2 instance type
________________________________________
**📤 Outputs (main.tf)**

Terraform usually provides these outputs:

•	EKS cluster name

•	Cluster endpoint

•	IAM role ARNs

•	Kubeconfig setup commands

This information helps configure kubectl or CI/CD tools like Jenkins.
________________________________________
**🔄 Workflow Summary**

1.	Initialize Terraform

terraform init

2.	Plan and apply

terraform apply -auto-approve

3.	Configure kubectl access

aws eks update-kubeconfig

4.	Deploy apps using Kubernetes
   
kubectl apply -f <manifest>.yaml
________________________________________
**📉 Infrastructure Diagram**

Refer to the diagram provided above. It shows:

•	VPC with public subnets

•	EKS control plane

•	EC2-based node group

•	IAM integration

•	EC2 instance (admin/jump server)
________________________________________
**💡 Key Concepts Demonstrated**

Concept	Explanation

IaC	Manages AWS infra using Terraform

EKS	Fully managed Kubernetes control plane

Node Group	Auto-scalable worker EC2 nodes

IAM Roles	Secure resource access policies

CI/CD Ready	You can easily extend this with Jenkins, GitHub Actions, etc.

Networking	Isolated VPC with internet-enabled subnets
________________________________________
**❗ What Can Be Improved**

•	Use Remote Backend (S3 + DynamoDB) for state locking

•	Add Helm charts for add-ons (Ingress, Prometheus, etc.)

•	Improve secrets management (avoid hardcoding AWS keys in ec2.sh)

•	Add a CI/CD pipeline for application deployment
