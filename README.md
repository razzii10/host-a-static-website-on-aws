---

# Host a Static Website on AWS

## Project Overview

This project demonstrates how to host a static HTML website on AWS using various AWS services, focusing on high availability, scalability, fault tolerance, and security. The website is deployed on EC2 instances within a Virtual Private Cloud (VPC) and is managed through an Auto Scaling Group, Application Load Balancer, and other AWS infrastructure.

## Architecture

The architecture consists of:

1. **Virtual Private Cloud (VPC)**: Configured with public and private subnets across two different Availability Zones for redundancy.
2. **Internet Gateway**: Facilitates connectivity between the VPC and the Internet.
3. **Security Groups**: Act as a network firewall to control inbound and outbound traffic.
4. **Availability Zones**: Used across the infrastructure to enhance fault tolerance and system reliability.
5. **Public Subnets**: Host infrastructure components like the NAT Gateway and Application Load Balancer.
6. **Private Subnets**: Host the web servers (EC2 instances) for enhanced security.
7. **EC2 Instance Connect Endpoint**: Used for secure connections to both public and private subnets.
8. **NAT Gateway**: Enables private subnet instances to access the Internet securely.
9. **EC2 Instances**: Host the static website.
10. **Application Load Balancer**: Distributes web traffic across EC2 instances to ensure high availability.
11. **Auto Scaling Group**: Automatically manages EC2 instances to maintain availability and scalability.
12. **GitHub**: Used for version control and collaboration.
13. **AWS Certificate Manager (ACM)**: Secures application communication using SSL/TLS certificates.
14. **Simple Notification Service (SNS)**: Sends alerts about activities in the Auto Scaling Group.
15. **Route 53**: Manages domain registration and DNS records.

## Prerequisites

Before starting, ensure you have the following:

- An AWS account with the necessary IAM permissions.
- A registered domain name (can be done through Route 53 or another domain registrar).
- A GitHub repository containing the website files.
- Basic knowledge of AWS services like EC2, VPC, Route 53, and Auto Scaling Groups.

## Deployment Steps

### 1. Launch and Configure AWS Resources

- **VPC**: Create a VPC with both public and private subnets spread across two Availability Zones.
- **Internet Gateway**: Attach an Internet Gateway to the VPC for Internet access.
- **Security Groups**: Set up security groups to allow HTTP/HTTPS traffic to the Application Load Balancer and restrict access to EC2 instances in private subnets.
- **Application Load Balancer**: Deploy an Application Load Balancer in the public subnets to handle incoming traffic.
- **NAT Gateway**: Place the NAT Gateway in the public subnets to allow instances in private subnets to reach the Internet.

### 2. Set Up Auto Scaling and Load Balancing

- **Auto Scaling Group**: Configure an Auto Scaling Group to automatically manage EC2 instances based on traffic demands.
- **Target Group**: Create a target group for the Application Load Balancer to route traffic to EC2 instances.

### 3. Secure Communication

- Use **AWS Certificate Manager** (ACM) to create and attach SSL certificates to the Application Load Balancer, securing the website traffic with HTTPS.

### 4. Domain Registration and DNS Setup

- Use **Route 53** to register your domain and create DNS records pointing to the Application Load Balancer.

### 5. Deploy the Static Website

SSH into the EC2 instances within your Auto Scaling Group and execute the following script:

```bash
#!/bin/bash
# Switch to the root user to gain full administrative privileges
sudo su 
# Update all installed packages to their latest versions
yum update -y 
# Install Apache HTTP Server
yum install -y httpd 
# Change the current working directory to the Apache web root
cd /var/www/html 
# Install Git
yum install git -y 
# Clone the project GitHub repository to the current directory
git clone https://github.com/razzii10/host-a-static-website-on-aws.git 
# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/.  /var/www/html/ 
# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws 
# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd  
# Start the Apache HTTP Server to serve web content
systemctl start httpd
```

### 6. Set Up Alerts

- Configure **Simple Notification Service (SNS)** to send notifications about activities in the Auto Scaling Group.

## Conclusion

This project demonstrates how to leverage various AWS services to deploy a highly available, scalable, and secure static website. The combination of VPC, Auto Scaling, and Load Balancing ensures that the website remains online and performs well under varying traffic conditions.

---


