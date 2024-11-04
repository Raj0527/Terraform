# Terraform

## Table of Contents
- [Lab 1: Creating an EC2 Instance in AWS and Installing Terraform](#lab-1-creating-an-ec2-instance-in-aws-and-installing-terraform)
  - [Task 1: Installing Terraform on Ubuntu 20.04](#task-1-installing-terraform-on-ubuntu-2004)
  - [Task 2: Install and login to Ubuntu server from AWS EC2 Instance](#task-2-install-and-login-to-ubuntu-server-from-aws-ec2-instance)
  - [Task 3: Launching Your First AWS EC2 Instance using Terraform](#task-3-launching-your-first-aws-ec2-instance-using-terraform)
- [Lab 2: AWS EC2 Instance Creation using Terraform Variables](#lab-2-aws-ec2-instance-creation-using-terraform-variables)
  - [Task 1: Create EC2 Instance using Variables](#task-1-create-ec2-instance-using-variables)
  - [Task 2: Implementing Map Variables to Dynamically Fetch AMI](#task-2-implementing-map-variables-to-dynamically-fetch-ami)
- [Lab 3: Using Terraform Output Feature](#lab-3-using-terraform-output-feature)
  - [Task 1: Getting EC2 Instance IP Address with Output Feature](#task-1-getting-ec2-instance-ip-address-with-output-feature)
- [Lab 4: Understanding Local Values, Functions, and Data Sources](#lab-4-understanding-local-values-functions-and-data-sources)
  - [Task 1: Using Local Values](#task-1-using-local-values)
  - [Task 2: Using Functions](#task-2-using-functions)
  - [Task 3: Using Data Sources](#task-3-using-data-sources)
- [Lab 5: Remote State using Amazon S3](#lab-5-remote-state-using-amazon-s3)
  - [Task 1: Create a S3 Bucket](#task-1-create-a-s3-bucket)
  - [Task 2: Configure Remote State](#task-2-configure-remote-state)
- [Lab 6: Launching VPC and EC2 Instance](#lab-6-launching-vpc-and-ec2-instance)
  - [Task 1: Launching VPC and Creating Subnets](#task-1-launching-vpc-and-creating-subnets)
  - [Task 2: Launching an EC2 Instance](#task-2-launching-an-ec2-instance)
  - [Task 3: Connecting an EBS with EC2 Instance](#task-3-connecting-an-ebs-with-ec2-instance)

---

## Lab 1: Creating an EC2 Instance in AWS and Installing Terraform

### Task 1: Installing Terraform on Ubuntu 20.04
1. Launch a `t2.micro` instance with OS version Ubuntu 20.04 in `us-east-1` with security group including ports 22 and 80.
2. Log in with the username `ubuntu`.
3. Update hostname and install required packages:
    ```bash
    sudo hostnamectl set-hostname terraform
    sudo apt update
    sudo apt install wget unzip -y
    ```
4. Download and install Terraform:
    ```bash
    wget https://releases.hashicorp.com/terraform/1.4.0/terraform_1.4.0_linux_amd64.zip
    unzip terraform_1.4.0_linux_amd64.zip
    sudo mv terraform /usr/local/bin
    terraform -v
    ```

### Task 2: Install and login to Ubuntu server from AWS EC2 Instance
1. Install `awscli` and configure AWS credentials:
    ```bash
    sudo apt-get install python3-pip -y
    sudo pip3 install awscli
    aws configure
    ```
2. Verify S3 access:
    ```bash
    aws s3 ls
    ```

### Task 3: Launching Your First AWS EC2 Instance using Terraform
1. Create Terraform configuration for EC2 instance (`example.tf`):
    ```hcl
    provider "aws" {
      region = "us-east-2"
    }
    resource "aws_instance" "example" {
      ami           = "ami-00eeedc4036573771"
      instance_type = "t2.micro"
      tags = {
        Name = "Yourname-TF-1"
      }
    }
    ```
2. Initialize and apply the configuration:
    ```bash
    terraform init
    terraform plan
    terraform apply
    ```
3. Destroy resources:
    ```bash
    terraform destroy
    ```

## Lab 2: AWS EC2 Instance Creation using Terraform Variables

### Task 1: Create EC2 Instance using Variables
1. Configure provider, variables, and instance files.
2. Initialize and apply the configuration:
    ```bash
    terraform init
    terraform plan
    terraform apply
    ```

### Task 2: Implementing Map Variables to Dynamically Fetch AMI
1. Update `instance.tf` and `vars.tf` to include map variables.
2. Run the plan with variable selection:
    ```bash
    terraform plan -var 'Linux_distro=redhat' -out myplan
    terraform apply myplan
    ```

## Lab 3: Using Terraform Output Feature

### Task 1: Getting EC2 Instance IP Address with Output Feature
1. Configure output variables for EC2 instance IP.
2. Use `terraform output` to display the public IP:
    ```bash
    terraform output Public_ip
    terraform output Private_ip
    ```

## Lab 4: Understanding Local Values, Functions, and Data Sources

### Task 1: Using Local Values
1. Define `local.tf` with custom tags:
    ```hcl
    locals {
      custom_tags = {
        Team = "DevOps"
        company = "CloudThat"
      }
    }
    ```
2. Apply the configuration.

### Task 2: Using Functions
1. Create instances with mapped AMI values and variable-based tags.

### Task 3: Using Data Sources
1. Fetch AMI dynamically using `data` block and launch an instance.

## Lab 5: Remote State using Amazon S3

### Task 1: Create a S3 Bucket
1. Create an S3 bucket with ACLs enabled and versioning.

### Task 2: Configure Remote State
1. Set up backend configuration (`backend.tf`) to store state files in S3.
2. Initialize and apply:
    ```bash
    terraform init
    terraform plan
    terraform apply
    ```

## Lab 6: Launching VPC and EC2 Instance

### Task 1: Launching VPC and Creating Subnets
1. Set up VPC configuration with subnets, NAT, and security groups.
2. Apply configuration:
    ```bash
    terraform init
    terraform apply -auto-approve
    ```

### Task 2: Launching an EC2 Instance
1. Define security groups and SSH key pairs.
2. Launch instance in created VPC subnet.

### Task 3: Connecting an EBS with EC2 Instance
1. Attach EBS volume to EC2 instance and apply configuration.
2. Clean up resources:
    ```bash
    terraform destroy
    ```

---
