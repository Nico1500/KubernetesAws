# Kubernetes on AWS
![image](https://github.com/Nico1500/KubernetesAws/assets/63806892/b2a41a9e-f429-483a-bb56-10fd55aa92d2)

This project demonstrates the deployment of a microservices application on AWS using Kubernetes. The application consists of three main components:

- **User API**: Manages users.
- **Task API**: Manages tasks.
- **Authentication API**: Handles user authentication. For security reasons, this API is only accessible via a Cluster IP.

The User API is configured to use an AWS EFS CSI persistent volume for user data persistence.

## Configuration on AWS

To deploy this project on AWS, I followed these steps:

### 1. Cluster Configuration with VPC

- **Public and Private Cluster Configuration**: Set up a VPC (Virtual Private Cloud) to isolate your Kubernetes cluster in a virtual network on AWS.
- **VPC Roles Creation**: Create the necessary IAM roles to allow the EKS cluster to interact with other AWS services.

### 2. Connecting to AWS

- **Connection via Terminal**: Use the AWS CLI to connect my AWS account. Configure the AWS region to deploy the EKS cluster.

  ```
  aws configure
  ```

### 3. Nodes Configuration

- **Node Configuration**: Specify the machine specifications (EC2 instances) and scaling options for my nodes.
- **Node Roles Creation**: Create the necessary IAM roles for the nodes to access AWS resources.

### 4. Kubernetes Deployment

- **Applying YAML Files**: Apply my Kubernetes YAML files to configure the nodes, deploy my pods, and create my services. AWS automatically configured the Load Balancers to expose my services with accessible URLs.

  ```
  kubectl apply -f <file>.yaml
  ```

### 5. Security Configuration

- **Security Group Creation**: I created a security group with an NFS rule on the VPC to secure access to the cluster and allow the VPC to manage the network file system.

### 6. Persistent Volumes with AWS CSI

- **Adding AWS CSI Persistent Volumes**: I used AWS's CSI to create persistent volumes that function similarly to Docker volumes but with native integration into AWS.

### 7. File System on the VPC

- **File System Creation**: Create an EFS file system on the VPC so it's accessible by pods within the same network.
- **File System Linking to Regions**: Ensure the file system is available in both availability zones so the APIs can use the same data.
