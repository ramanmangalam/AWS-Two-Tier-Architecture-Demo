# AWS-Two-Tier-Architecture-Demo
This repository demonstrates a two-tier architecture in AWS using services like VPC, Auto Scaling, Launch Templates, Bastion Host, and an Application Load Balancer (ALB). This architecture is designed to host a web application on a private subnet and expose it securely via an ALB.

## Architecture Overview

The architecture consists of:

- **VPC**: Custom VPC with public and private subnets.

- **Auto Scaling Group (ASG)**: Running application instances on private subnets.

- **Launch Template**: Defines the instance configuration for the ASG.

- **Bastion Host**: Provides SSH access to instances in the private subnet.

- **Application Load Balancer**: Distributes traffic across instances running on private subnets.

## Step-by-Step Setup

### 1. VPC Creation
**VPC Name**: My-prod-Demo

Created a VPC with both Public Subnet and Private Subnet for isolating resources.

### 2. Auto Scaling Group (ASG) and Launch Template
   
- **Auto Scaling Group Name**: My-prodASG-Demo

- **Launch Template Name**: My-prodLT-Demo

- **Security Group**: My-ProdSG-Demo

 - **Inbound Rules**:
    - Port 22 for SSH access.
    - Port 8000 for the application.

The application is set to run on instances in the Private Subnet.

**Desired Capacity**: 2 instances (minimum: 1, maximum: 4).

The ASG distributes instances across two availability zones: ap-south-1a and ap-south-1b.

### 3. Bastion Host Setup
   
Created a separate Bastion Host in the Public Subnet to allow SSH access to the instances running in the private subnet.

Used Windows PowerShell to connect to the Bastion Host:
code: scp -i Prod-key.pem path/to/file ubuntu@bastion-public-ip:/home/ubuntu/

Then SSH into the Bastion Host:
code:ssh -i Prod-key.pem ubuntu@bastion-public-ip

From the Bastion Host, accessed the private subnet instances using:
code:ssh -i Prod-key.pem ubuntu@private-instance-ip

### 4. Application Deployment
On the private instances, created a simple web application (index.html) and started a Python HTTP server:
code:python3 -m http.server 8000

### 5. Application Load Balancer (ALB) Setup

 **ALB Name**: My-ProdLB-Demo

 **Target Group (TG) Name**: My-ProdTG-Demo

 **Load balancer port**: 80 (HTTP)

Target instances on port 8000 (for the web application).

The ALB is associated with two public subnets for load balancing.

Attached the Target Group to the ALB.

Fixed a port mismatch error by updating the security group to allow port 80.

### 6. Testing the Setup
   
Once the ALB was successfully configured, copied the DNS name of the ALB and accessed the application in the browser:
 **URL**: http://<ALB-DNS>

Result: The index.html page served by the instances in the private subnet was successfully displayed.

Key Components
 **VPC**: Isolated environment for hosting application resources.

 **Auto Scaling Group**: Automatically scales EC2 instances based on demand.

 **Bastion Host**: Securely access private instances.

 **Application Load Balancer**: Distributes incoming traffic across EC2 instances running in the private subnet.

Conclusion
This two-tier architecture in AWS leverages the power of Auto Scaling and Load Balancing to provide a scalable and highly available environment for web applications. By isolating the application instances in private subnets and using a Bastion Host for secure access, the architecture ensures both security and scalability.

Future Improvements

Add SSL/TLS for secure communication using HTTPS.

Implement a fully automated CI/CD pipeline for application deployment.
