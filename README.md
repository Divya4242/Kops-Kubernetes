# Kubernetes Cluster Setup with Kops on AWS

This guide provides a step-by-step process to create a Kubernetes cluster on AWS using Kops. 

## Prerequisites

- An AWS account
- Basic knowledge of AWS and Kubernetes
- AWS CLI configured with appropriate permissions

## Steps

### 1. Launch an EC2 Instance

Launch an EC2 instance with the `t2.micro` instance type. You can either create a new instance or use an existing one. Ensure you have SSH access to the instance.

### 2. Install Kops

SSH into your EC2 and install Kops. Follow the installation instructions from the official Kops documentation [here](https://kops.sigs.k8s.io/getting_started/install/).

### 3. Update the System

Update the system packages.

```sh
sudo apt update -y
```

### 4. Create IAM Role and Attach it to EC2

Create an IAM role with necessary permissions and programmatic access. Attaching **AdministratorAccess** is recommended for simplicity. Attach the IAM role to your EC2 instance.

### 5. Create an S3 Bucket

Create an S3 bucket which will be used by Kops to store cluster state information.

### 6. Create a Hosted Zone in Route53

Create a hosted zone in Route53. If you already have a hosted zone, you can skip this step.

### 7. Install kubectl

Install kubectl by following the instructions [here](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/).

### 8. Create the Cluster

Run the following command to create your cluster. Update the placeholders with your specific values.

```sh
kops create cluster --name=k8s-cluster.example.com --state=s3://<your-s3-bucket> --zones=<your-availability-zone> --node-count=1 --node-size=t2.medium --control-plane-size=t2.medium --dns-zone=<your-domain>
```

### 9. Update and Apply the Cluster Configuration

The previous command generates a preview of all the resources Kops will create. If the preview looks good, run the following command to create the resources. (Same like terraform plan(9th step) and terraform apply(10th step))

```sh
kops update cluster --name=k8s-cluster.example.com--yes --admin --state=s3://<your-s3-bucket>
```
This process may take 5-10 minutes.

### 10. Validate the Cluster

Validate that the cluster is ready.

```sh
kops validate cluster --name=k8s-cluster.example.com --state=s3://<your-s3-bucket>
```

This `README.md` provides clear, concise instructions and references for setting up a Kubernetes cluster using Kops on AWS. Adjust the placeholders to fit your specific configurations and ensure that all steps are followed accurately for a successful setup.
