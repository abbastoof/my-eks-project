# **Kubernetes End-to-End Project on AWS EKS**

Welcome! This project showcases how to set up a Kubernetes cluster on AWS using EKS and Fargate, then deploy a sample game application (2048). You’ll find clear instructions on how to replicate the setup on your own, even if you're new to Kubernetes or AWS.

## **Project Overview**
The goal of this project is to:
1. Create a Kubernetes cluster on AWS using **EKS**.
2. Deploy the classic **2048 game** in the cluster.
3. Use an **Application Load Balancer (ALB)** to make the game accessible from the internet.

By following the steps, you’ll learn how to:
- Set up a cluster using **eksctl**.
- Deploy resources with **kubectl**.
- Manage permissions with **AWS IAM**.

## **Getting Started**
To get started, clone this repository to your local machine:
```bash
git clone https://github.com/yourusername/my-eks-project.git
cd my-eks-project
```

### **Project Structure**
Here’s how the files are organized:
```
/my-eks-project
  ├── README.md           # You're reading this!
  ├── deployment/         # Contains Kubernetes YAML configuration files
      ├── 2048_full.yaml  # The file for deploying the 2048 game application
  ├── scripts/            # Useful scripts for setting up AWS and Kubernetes
      ├── create-cluster.sh    # Script to create the EKS cluster
      ├── create-iam-role.sh   # Script to set up IAM roles for ALB
  ├── .gitignore          # Standard gitignore file
```

## **Before You Begin**
You’ll need a few tools installed on your machine:
1. **AWS CLI** – Command-line tool to interact with AWS. [Download it here.](https://aws.amazon.com/cli/)
2. **eksctl** – CLI tool for creating and managing EKS clusters. [Download it here.](https://eksctl.io/)
3. **kubectl** – CLI tool for interacting with Kubernetes clusters. [Install it here.](https://kubernetes.io/docs/tasks/tools/)
4. **Helm** – A package manager for Kubernetes. [Install it here.](https://helm.sh/docs/intro/install/)

Once these tools are installed, you're ready to proceed!

## **Step-by-Step Guide**

### 1. **Set up AWS CLI**
You’ll need to configure the AWS CLI so that it can interact with your AWS account:
```bash
aws configure
```
It will ask you for your **AWS Access Key**, **Secret Key**, and region (choose your preferred AWS region).

### 2. **Create an EKS Cluster**
Run the following script to create your cluster:
```bash
./scripts/create-cluster.sh
```
This will set up a Kubernetes cluster named `demo-cluster` in the AWS region `eu-north-1`.

### 3. **Update Kubernetes Configuration**
To make sure `kubectl` can communicate with your cluster, update the kubeconfig:
```bash
aws eks update-kubeconfig --name demo-cluster --region eu-north-1
```

### 4. **Deploy the Game Application**
Deploy the 2048 game into your cluster:
```bash
kubectl apply -f deployment/2048_full.yaml
```

### 5. **Verify the Deployment**
Check if everything is running properly:

- List services in the `game-2048` namespace:
  ```bash
  kubectl get svc -n game-2048
  ```

- List the pods in the same namespace:
  ```bash
  kubectl get pods -n game-2048
  ```

### 6. **Set Up the Application Load Balancer**
To allow users to access the game from their browser, you need to deploy the ALB controller. Run the following commands to set up IAM roles and the ALB controller:

```bash
./scripts/create-iam-role.sh
```

Once the ALB is ready, you can access the game by checking the **Ingress**:
```bash
kubectl get ingress -n game-2048
```
The **ADDRESS** field will show the public URL for your 2048 game.

## **Need Help?**
If you're new to Kubernetes or AWS, don't worry! Feel free to open an issue or reach out for help. Here are some useful links to get more information:
- [AWS EKS Documentation](https://docs.aws.amazon.com/eks/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)

Enjoy deploying your Kubernetes cluster and have fun with the 2048 game! 🚀🎮
