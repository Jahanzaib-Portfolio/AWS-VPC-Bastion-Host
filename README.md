(images/image.png)

# VPC with Bastion Host Project

## Overview
This project demonstrates how to securely access private EC2 instances using a Bastion Host within a Virtual Private Cloud (VPC). A Bastion Host acts as a secure entry point, allowing administrators to connect to EC2 instances in private subnets without exposing them directly to the internet.

The setup ensures a layered security approach by keeping application servers in private subnets while providing controlled access via a publicly accessible Bastion Host.

---

## Architecture
The project consists of the following components:

* **VPC** - Custom VPC to logically isolate resources.
* **Subnets** - 
  * **Public subnet** - For hosting the Bastion Host.
  * **Private subnet** - For hosting backend EC2 instances.
* **Internet Gateway** - Provides internet access to the public subnet.
* **NAT Gateway (Optional)** - Allows private instances to access the internet for updates without being exposed.
* **Route Tables** - Configured for proper routing between subnets.
* **Security Groups** - 
  * **Bastion Host SG** - Allows SSH access from trusted IPs only.
  * **Private Instance SG** - Allows SSH access only from the Bastion Host SG identity.
* **Bastion Host EC2** - Acts as a jump server to access private instances.
* **Private EC2 Instances** - Application or database servers inside private subnets.

---

## Steps to Implement

### 1. Configure the Network
* Create a VPC with a CIDR block (e.g., `10.0.0.0/16`).
* Create your subnets:
  * Public subnet for Bastion Host (e.g., `10.0.1.0/24`).
  * Private subnet for application servers (e.g., `10.0.2.0/24`).
* Attach an Internet Gateway to the VPC and update the route table for the public subnet.

### 2. Create Route Tables
* **Public subnet route table** -> Route traffic to the Internet Gateway.
* **Private subnet route table** -> Route traffic to the NAT Gateway (if required).

### 3. Launch EC2 Instances
* Deploy the Bastion Host in the public subnet with an Elastic IP or public IP address assigned.
* Deploy the Private EC2 instance in the private subnet without a public IP.

### 4. Configure Security Groups
* **Bastion Host SG** -> Allow inbound SSH (Port 22) from your IP address.
* **Private Instance SG** -> Allow inbound SSH (Port 22) only from the Bastion Host SG.

### 5. Access Flow
* SSH into the Bastion Host using its public IP address.
* From inside the Bastion Host terminal, SSH into the private EC2 instance using its internal private IP address.

---

## Key Learnings
* Secure access management to private EC2 instances using a Bastion Host.
* Structural differences and distinct use cases between public vs private subnets in a VPC.
* The explicit role of an Internet Gateway and a NAT Gateway in infrastructure routing.
* The importance of the principle of least privilege security using chained Security Groups.
* Hands-on experience with VPC architecture design and implementation.
