# AWS VPC Basics – Learning Notes ☁️

This repository contains my **AWS learning notes** on Virtual Private Cloud (VPC) and network mapping.  
Purpose: Understand how on-premises networks translate to AWS VPC networks.

---

## 1️⃣ Introduction to VPC
- **VPC (Virtual Private Cloud)** = Private networking space in AWS ☁️  
- Launch **EC2 instances, databases**, and other resources inside a private network.

---

## 2️⃣ On-Premises Networking Recap
- **LAN (Local Area Network)** connects multiple hosts over physical links 🖥️  
- **Switch**: Operates at Layer 2, forwards packets using MAC addresses 🔀  
- **Router**: Connects multiple LANs and routes packets between networks 🌐  
- Internet access is via **top-level router** 🚪  

---

## 3️⃣ Virtual Networking for Global Offices
- Teams across offices/countries can share the same network 🌏  
- Virtual LAN (VLAN) allows hosts in different physical LANs to appear on the **same network** 💻  

---

## 4️⃣ Mapping Physical Network to AWS VPC
- **Physical LAN → AWS Subnet**  
- **Router → Local VPC Router**  
- **Top-level Internet Router → Internet Gateway**  
- Deploy resources (EC2, RDS) inside subnets  

---

## 5️⃣ VPC Connectivity
- **Internal VPC communication**: Local routing 🛤️  
- **External (Internet) communication**: Requires **Internet Gateway** 🌐  
- VPC without Internet Gateway = private network 🔒  

---

## 6️⃣ Key Points
- AWS VPC allows you to **replicate physical network topology** in the cloud  
- Subnets represent individual LANs  
- EC2 instances deployed inside subnets  
- Internet Gateway enables external access  

---

## 7️⃣ Next Steps
- Deeper dive into **VPC components** like subnets, internet gateways, and routing tables in the following lectures 🔎  
- Focus will remain on **AWS networking** and internal VPC design.

---

🔗 [Physical on premises network vs VPC](https://github.com/amilasenakumara/aws-vpc-networking/blob/d7d7c4d81c635bc08e0f3e3aab6d3562a2ffecec/images/physical-cloud.png)

*AWS Networking Notes – Organized and Documented.*

## 📝 Exam Questions & Answers

1. **Q:** What is a VPC in AWS?  
   **A:** A Virtual Private Cloud (VPC) is a private networking space in AWS where you can launch EC2 instances, databases, and other resources.

2. **Q:** How does a subnet in a VPC relate to an on-premises LAN?  
   **A:** A subnet represents a segment of the VPC IP range and functions like a physical LAN in an on-premises network.

3. **Q:** What AWS component provides internet access to a VPC?  
   **A:** An Internet Gateway (IGW) enables external communication from resources inside a VPC.

4. **Q:** How is routing handled inside a VPC?  
   **A:** Internal communication is handled by the local VPC router, while external communication requires proper route table entries pointing to an IGW.

5. **Q:** What happens if a VPC does not have an Internet Gateway?  
   **A:** The VPC remains private, and its resources cannot communicate directly with the internet.

---

## 💼 Interview Questions & Answers

1. **Q:** Explain how an on-premises network maps to an AWS VPC.  
   **A:** Physical LANs map to subnets, routers map to the local VPC router, and the top-level internet router maps to the Internet Gateway.

2. **Q:** What is the purpose of a subnet in a VPC?  
   **A:** Subnets isolate and organize resources within a VPC, similar to LANs in physical networks.

3. **Q:** How can you make a private VPC communicate with the internet?  
   **A:** By attaching an Internet Gateway to the VPC and updating the route tables for relevant subnets.

4. **Q:** What is the role of the local VPC router?  
   **A:** It handles internal routing within the VPC, directing traffic between subnets.

5. **Q:** Why is it important to understand physical networking concepts before learning AWS VPC?  
   **A:** Understanding LANs, routers, and internet access helps map on-premises network design to AWS VPC architecture efficiently.


## 📝 Advanced Exam Questions & Answers

1. **Q:** You have a VPC with two subnets: one public and one private. How would you allow EC2 instances in the private subnet to access the internet?  
   **A:** Attach an Internet Gateway to the VPC and configure a NAT Gateway in the public subnet. Update the private subnet’s route table to route internet-bound traffic through the NAT Gateway.

2. **Q:** Describe how you would connect two VPCs in different AWS regions.  
   **A:** Use VPC Peering or AWS Transit Gateway with inter-region peering to enable private communication between the VPCs using private IPs.

3. **Q:** A user reports that their EC2 instance in a public subnet cannot access the internet. What steps would you take to troubleshoot?  
   **A:** Check if:  
   - The subnet has a route to the Internet Gateway  
   - The EC2 instance has a public IP  
   - Security groups and NACLs allow outbound traffic

4. **Q:** Explain the difference between a public subnet and a private subnet.  
   **A:** Public subnets have a route to an Internet Gateway and can communicate directly with the internet. Private subnets have no direct route to the internet and typically use a NAT Gateway for outbound traffic.

5. **Q:** You need high availability for a database in a VPC. How would you design it?  
   **A:** Place database instances in private subnets across multiple Availability Zones and enable synchronous replication to ensure resilience if one AZ fails.

---

## 💼 Advanced Interview Questions & Answers

1. **Q:** How do you secure VPC resources that must remain private but still access AWS services like S3?  
   **A:** Use VPC Endpoints (Gateway for S3/DynamoDB or Interface for other services) to allow private connectivity without traversing the internet.

2. **Q:** Explain the role of route tables in controlling network traffic within a VPC.  
   **A:** Route tables define how traffic is directed between subnets and external networks. Each subnet is associated with a route table specifying routes for internal and external communication.

3. **Q:** Your private subnet instances need to access a third-party API on the internet. How would you achieve this securely?  
   **A:** Deploy a NAT Gateway or NAT instance in a public subnet. Configure the private subnet’s route table to send outbound traffic through the NAT Gateway, allowing access without exposing instances directly.

4. **Q:** What strategies would you use to minimize latency for users across multiple regions?  
   **A:** Deploy resources in multiple AWS regions, use CloudFront for content delivery, and leverage Route 53 latency-based routing to direct users to the nearest endpoint.

5. **Q:** How would you handle IP address planning when scaling a VPC to accommodate multiple subnets and future growth?  
   **A:** Use CIDR blocks with sufficient IP space, segment them logically by application or AZ, and reserve blocks for future expansion to avoid overlapping or shortage of IP addresses.
