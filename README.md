# Terraform

## Table of Contents
- [Project 1: Creating an EC2 Instance in AWS and Installing Terraform](#project-1-creating-an-ec2-instance-in-aws-and-installing-terraform)
  - [Task 1: Installing Terraform on Ubuntu 20.04](#task-1-installing-terraform-on-ubuntu-2004)
  - [Task 2: Installing and Logging into an Ubuntu Server from an AWS EC2 Instance](#task-2-installing-and-logging-into-an-ubuntu-server-from-an-aws-ec2-instance)
  - [Task 3: Launching My First AWS EC2 Instance using Terraform](#task-3-launching-my-first-aws-ec2-instance-using-terraform)
- [Project 2: AWS EC2 Instance Creation using Terraform Variables](#project-2-aws-ec2-instance-creation-using-terraform-variables)
  - [Task 1: Creating an EC2 Instance using Variables](#task-1-creating-an-ec2-instance-using-variables)
  - [Task 2: Implementing Map Variables to Dynamically Fetch AMI](#task-2-implementing-map-variables-to-dynamically-fetch-ami)
- [Project 3: Using Terraform Output Feature](#project-3-using-terraform-output-feature)
  - [Task 1: Getting EC2 Instance IP Address with Output Feature](#task-1-getting-ec2-instance-ip-address-with-output-feature)
- [Project 4: Understanding Local Values, Functions, and Data Sources](#project-4-understanding-local-values-functions-and-data-sources)
  - [Task 1: Using Local Values](#task-1-using-local-values)
  - [Task 2: Using Functions](#task-2-using-functions)
  - [Task 3: Using Data Sources](#task-3-using-data-sources)
- [Project 5: Remote State using Amazon S3](#project-5-remote-state-using-amazon-s3)
  - [Task 1: Creating an S3 Bucket](#task-1-creating-an-s3-bucket)
  - [Task 2: Configuring Remote State](#task-2-configuring-remote-state)
- [Project 6: Launching VPC and EC2 Instance](#project-6-launching-vpc-and-ec2-instance)


---

## Project 1: Creating an EC2 Instance in AWS and Installing Terraform

I have performed all the tasks in this project to set up my first EC2 instance and install Terraform on it.

### Task 1: Installing Terraform on Ubuntu 20.04
I started by updating the package repository and installing `curl` to download Terraform. I then set up the HashiCorp key, added the Terraform repository, and installed Terraform. Finally, I verified the installation by running the `terraform -version` command.

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

### Task 2: Installing and Logging into an Ubuntu Server from an AWS EC2 Instance
I created a new EC2 instance on AWS and configured the necessary security group rules for SSH access. Once the instance was running, I used SSH to connect to it, confirming I could access the server remotely.

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

### Task 3: Launching My First AWS EC2 Instance using Terraform
Using Terraform, I set up a configuration file defining my desired EC2 instance setup. I executed `terraform apply` to launch the instance, confirmed its status in the AWS Console, and verified the instance IP address.

1. Create Terraform configuration for EC2 instance (`example.tf`):
    ```hcl
    provider "aws" {
      region = "us-east-2"
    }
    resource "aws_instance" "example" {
      ami           = "ami-00eeedc4036573771"
      instance_type = "t2.micro"
      tags = {
        Name = "Rajssole-TF-1"
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

## Project 2: AWS EC2 Instance Creation using Terraform Variables

In this project, I explored using variables to create EC2 instances, allowing for more flexibility in my configurations.

### Task 1: Creating an EC2 Instance using Variables
I set up Terraform configuration files with variables for instance type, AMI ID, and region, making it easy to modify these values without changing the core configuration. Running `terraform apply` launched an EC2 instance based on the provided variable values.

1. Configure provider, variables, and instance files.
2. Initialize and apply the configuration:
    ```bash
    terraform init
    terraform plan
    terraform apply
    ```

### Task 2: Implementing Map Variables to Dynamically Fetch AMI
I created a map variable to dynamically retrieve the correct AMI based on the instance region, making the configuration adaptable for various environments. This approach allowed me to deploy instances in different regions without manual adjustments.

1. Update `instance.tf` and `vars.tf` to include map variables.
2. Run the plan with variable selection:
    ```bash
    terraform plan -var 'Linux_distro=redhat' -out myplan
    terraform apply myplan
    ```

## Project 3: Using Terraform Output Feature

In this project, I utilized Terraform’s output feature to display instance details post-deployment.

### Task 1: Getting EC2 Instance IP Address with Output Feature
I configured Terraform outputs to retrieve the public IP address of my newly created EC2 instance. Running `terraform apply` now displays this IP address in the terminal output, which I confirmed by connecting to the instance using SSH.

1. Configure output variables for EC2 instance IP.
2. Use `terraform output` to display the public IP:
    ```bash
    terraform output Public_ip
    terraform output Private_ip
    ```

## Project 4: Understanding Local Values, Functions, and Data Sources

I explored local values, functions, and data sources in Terraform to enhance configuration flexibility.

### Task 1: Using Local Values
I defined local values to store reusable configuration elements, simplifying my Terraform code. For example, I stored frequently used tags and security groups in local values to avoid repetition.

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
I experimented with various functions, such as `length`, `lower`, and `join`, to modify and manage data within my configuration. These functions helped optimize configurations and manage string manipulation.

1. Create instances with mapped AMI values and variable-based tags.

### Task 3: Using Data Sources
I implemented data sources to retrieve information about existing AWS resources, such as AMIs and VPCs. This setup allowed me to reference these resources without manually defining them.

1. Fetch AMI dynamically using `data` block and launch an instance.

## Project 5: Remote State using Amazon S3

To enable shared state management, I configured Terraform to store its state file in an Amazon S3 bucket.

### Task 1: Creating an S3 Bucket
I created a new S3 bucket dedicated to storing Terraform state files, enabling centralized state management for my infrastructure deployments.

1. Create an S3 bucket with ACLs enabled and versioning.

### Task 2: Configuring Remote State
I modified my Terraform configuration to use the S3 bucket as the remote state backend, ensuring that state data is securely stored and accessible for future deployments or collaboration.

1. Set up backend configuration (`backend.tf`) to store state files in S3.
2. Initialize and apply:
    ```bash
    terraform init
    terraform plan
    terraform apply
    ```

## Project 6: Launching VPC and EC2 Instance

This project focused on creating a Virtual Private Cloud (VPC) along with an EC2 instance within it.

### Task 1: Configuring VPC and Subnets
I used Terraform to define a custom VPC with multiple subnets, setting up public and private subnet configurations. This VPC structure allows for isolated network resources.

1. Set up VPC configuration with subnets, NAT, and security groups.
2. Apply configuration:
    ```bash
    terraform init
    terraform apply -auto-approve
    ```

### Task 2: Launching an EC2 Instance in the VPC
Within the newly created VPC, I launched an EC2 instance, configuring it to use the appropriate subnet and security groups. After deployment, I verified the instance’s network settings and connectivity.

1. Define security groups and SSH key pairs.
2. Launch instance in created VPC subnet.

### Task 3: Connecting an EBS with EC2 Instance
1. Attach EBS volume to EC2 instance and apply configuration.
2. Clean up resources:
    ```bash
    terraform destroy
    ```

---

## Conclusion
Through these projects, I have gained extensive hands-on experience with Terraform and AWS. Each project allowed me to deepen my understanding of infrastructure automation, from setting up basic EC2 instances to managing complex networking and state management. By leveraging Terraform’s capabilities, I developed skills in creating reusable and flexible infrastructure code, and I am now better equipped to handle real-world deployment scenarios efficiently. This portfolio demonstrates my technical ability and dedication to continuous learning in cloud infrastructure.
