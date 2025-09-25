# 📚 AWS Networking Overview

This document summarizes key concepts of **AWS networking**, **VPC**, and related services for easier understanding and documentation.

🔗 [Which AWS network services you should know?](https://github.com/amilasenakumara/aws-vpc-networking/blob/d7d7c4d81c635bc08e0f3e3aab6d3562a2ffecec/images/aws-networking-services.png)
---

## 🌐 Big Picture of AWS Networking

- AWS networking allows you to **deploy applications securely** in the cloud.  
- Always start with a **high-level overview** before diving into individual components.  
- Revisit this overview after learning to **connect all dots visually**.  
- [Big Picture of AWS Networking](https://github.com/amilasenakumara/aws-vpc-networking/blob/d7d7c4d81c635bc08e0f3e3aab6d3562a2ffecec/images/course-overview.png)
---

## 🏗️ Basic Deployment Scenario

- Example: Deploying a web application with:
  - Web/application servers  
  - Database  
  - Users accessing via the internet  

- Steps in AWS deployment:  
  1. **Choose AWS Region** 🌎  
     - Geographic area with multiple Availability Zones (AZs)  
     - Chosen based on user location  
  2. **Availability Zones (AZs)** ⚡  
     - Multiple data centers per region  
     - Designed for **high availability**  
  3. **VPC (Virtual Private Cloud)** ☁️  
     - Private network space similar to on-premises network  
     - Launch EC2 instances, databases, and other resources inside VPC  

---

## 🛣️ Networking Components in VPC

- **Local Router**: Routes traffic within the VPC  
- **Route Table**: Contains route entries for traffic  
  - Example: `10.10.0.0/16` → routed via local router  
- **Subnets**:  
  - 1:1 mapping with AZs  
  - Multiple subnets per AZ  
- **Internet Gateway (IGW)** 🌐  
  - Connects VPC to the internet  
  - Requires route table update to allow external traffic  

---

## 💻 EC2 & DNS

- **EC2 Instances**: AWS compute service hosting your application  
- **Public IPs**: Allow users to access instances  
- **DNS (Route 53)** 🖥️  
  - Maps domain names to IP addresses  
  - Example: `example.com` → `11.22.33.44`  

---

## 🗺️ Custom Routing

- Different subnets may need **different routing logic**  
  - Public subnets → Internet access via IGW  
  - Private subnets → Internal-only traffic  
- Create **custom route tables** for specific subnet types  

---

## ⚖️ High Availability & Scaling

- Horizontal scaling: Launch multiple instances behind **Application Load Balancer (ALB)**  
- ALB distributes traffic using algorithms (e.g., round-robin)  
- Private subnets → EC2 instances not directly exposed to the internet  
- Isolate databases in private subnets for **security and control**  

---

## 🔄 Database Replication

- Use **synchronous replication** across AZs for **resiliency**  
- Ensures **high availability** even if one AZ fails  

---

## 🌐 Internet Access for Private Instances

- **NAT Gateway**: Allows outbound internet access for private instances  
- Update route table to send traffic to NAT Gateway  
- Internet cannot initiate inbound connections to private instances  

---

## 🪝 Accessing AWS Services Privately

- **VPC Endpoints**: Connect to AWS services privately without using NAT Gateway  
  - Types: Gateway (e.g., S3, DynamoDB), Interface (e.g., SQS, API Gateway)  
- **AWS PrivateLink** supports private connectivity to other AWS services or SaaS providers  

---

## 🔗 VPC-to-VPC & Hybrid Networking

- **VPC Peering**: Connect two VPCs directly using private IP addresses  
- **Transit Gateway**: Hub for connecting multiple VPCs and on-premises networks  
- **Site-to-Site VPN**: IPsec VPN connection to on-premises network  
- **Direct Connect**: Dedicated physical connection between on-prem network and AWS (50 Mbps–100 Gbps)  
- **Client VPN**: Remote users access VPC securely using VPN client  

---

## 📦 Content Delivery for Applications

- Serving static content (videos/images) directly from **S3** saves compute resources  
- **CloudFront (CDN)**:  
  - Edge locations cache content near users  
  - Reduces latency and improves end-user experience  
- Route traffic through **Route 53** to CloudFront distribution for DNS resolution  

---

## ✅ Key Takeaways

- AWS networking builds the foundation for **highly available, scalable, and secure applications**  
- Understanding VPC, subnets, routing, and gateways is critical for designing resilient architectures  
- Services like **ALB, NAT Gateway, VPC Endpoints, CloudFront, and Direct Connect** enhance performance, cost-efficiency, and private connectivity  
- Revisit this overview after completing the course to solidify your understanding  
