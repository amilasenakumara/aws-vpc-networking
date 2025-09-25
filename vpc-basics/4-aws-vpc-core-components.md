# AWS VPC Core Components – Learning Notes ☁️

This repository contains **organized notes on the core components of AWS VPC** for easier understanding and documentation.  
Purpose: Understand VPC building blocks, connectivity, and security for deploying secure cloud networks.

---

## 1️⃣ Overview

- VPC is the foundation for creating a **secure and isolated network** in AWS.
- Key steps before deployment:
  1. Select an **AWS Region** 🌎
  2. Create a **VPC** within the selected region
  3. Create **subnets** inside the VPC
  4. Assign **Availability Zones (AZs)** to subnets
- VPC spans multiple AZs for **high availability**.

---

## 2️⃣ VPC Addressing

- Assign a **private IP address range** when creating a VPC (CIDR block).  
  - Example: `10.0.0.0/16`
- CIDR = Classless Inter-Domain Routing (replaces old Class A/B/C IP ranges)
- Subnets created within VPC use portions of this CIDR block.

---

## 3️⃣ Subnets

- Subnet = division of VPC IP space, mapped to a specific **AZ**.  
- Multiple subnets can be created based on application needs.
- Enables **isolation and traffic management** between resources.
- Example: Launch EC2 instances in different subnets across AZs for redundancy.

---

## 4️⃣ Routing

- **Route Tables** control traffic flow:
  - Between subnets
  - To the internet
- Associate route tables with subnets to enable communication.
- Supports **internal and external network traffic**.

---

## 5️⃣ Security

### 5.1 Security Groups
- Firewall at **EC2 instance level** 🛡️
- Controls inbound and outbound traffic for individual EC2 instances.

### 5.2 Network ACLs (NACLs)
- Firewall at **subnet level** 🔒
- Applies rules to all resources within a subnet.
- Different rules and scope compared to security groups.

---

## 6️⃣ Internet Connectivity

### 6.1 Internet Gateway (IGW)
- Required for **internet access** from VPC.
- Without IGW, VPC remains a **private, isolated network**.
- Enables EC2 instances to connect to and from the internet.

### 6.2 Virtual Private Gateway (VPN Gateway)
- Connects **on-premises networks securely** to AWS VPC.
- Supports encrypted traffic via VPN.
- Essential for enterprise-level secure connectivity.

### 6.3 AWS Direct Connect
- **Dedicated physical connection** between on-premises network and AWS.
- Provides more stable, low-latency connectivity than standard internet VPN.

---

## 7️⃣ DNS in VPC

- **DNS** is essential for resolving application names.
- Two types:
  - **Private DNS**: Accessible only within the VPC
  - **Public DNS**: Accessible from the internet
- AWS Route 53 enables **public and private DNS management**.

---

## 8️⃣ Summary of Core Components

- **VPC**: Isolated network in a region
- **Subnets**: Segments of VPC mapped to AZs
- **Route Tables**: Control traffic flow
- **Security Groups**: EC2-level firewall
- **Network ACLs**: Subnet-level firewall
- **Internet Gateway**: Enables internet connectivity
- **Virtual Private Gateway**: Secure VPN connectivity
- **Direct Connect**: Physical network connection
- **DNS**: Private and public name resolution via Route 53

---

## 📝 Exam Questions & Answers

1. **Q:** What is a CIDR block and why is it important in a VPC?  
   **A:** CIDR defines the private IP address range of a VPC. It allows subnetting and ensures unique IPs for resources.

2. **Q:** How do subnets map to Availability Zones?  
   **A:** Each subnet is created in a specific AZ. Multiple subnets across AZs enable high availability.

3. **Q:** What is the role of route tables in a VPC?  
   **A:** Route tables define how traffic flows between subnets and to external networks like the internet.

4. **Q:** Difference between Security Groups and Network ACLs?  
   **A:** Security Groups = EC2-level firewall; Network ACLs = Subnet-level firewall applying to all instances in the subnet.

5. **Q:** What is the purpose of an Internet Gateway?  
   **A:** Allows EC2 instances in a VPC to communicate with the internet.

---

## 💼 Interview Questions & Answers

1. **Q:** How do you enable secure connectivity between on-premises network and AWS VPC?  
   **A:** Attach a Virtual Private Gateway to the VPC and establish a VPN connection for encrypted traffic.

2. **Q:** When would you use AWS Direct Connect?  
   **A:** When low-latency, reliable, private connectivity is needed between on-premises data centers and AWS.

3. **Q:** Can a VPC span multiple AWS regions?  
   **A:** No, VPCs are regional. For cross-region connectivity, use VPC Peering or Transit Gateway.

4. **Q:** How do private and public DNS differ in VPC?  
   **A:** Private DNS resolves names within the VPC; public DNS is accessible from the internet via Route 53.

5. **Q:** Why do enterprises prefer Network ACLs along with Security Groups?  
   **A:** Provides multi-layered security; ACLs control subnet-wide traffic while security groups manage instance-level traffic.

---

## 📝 Advanced Exam Questions & Answers

1. **Q:** How would you design a VPC for maximum high availability?  
   **A:** Deploy subnets across multiple AZs, place EC2 instances in different AZs, and configure route tables for inter-subnet communication.

2. **Q:** What is the difference between an Internet Gateway and a Virtual Private Gateway?  
   **A:** Internet Gateway connects VPC to the internet; Virtual Private Gateway connects VPC to on-premises networks securely.

3. **Q:** How do you configure traffic between private subnets and the internet?  
   **A:** Use a NAT Gateway in a public subnet and update route tables in private subnets to route outbound traffic through NAT.

4. **Q:** Explain the role of Route 53 in VPC architecture.  
   **A:** Provides private and public DNS resolution, allowing resources to be accessed via domain names instead of IPs.

5. **Q:** Can security groups and NACLs work together? How?  
   **A:** Yes, Security Groups filter traffic at instance level; NACLs filter traffic at subnet level. Both enforce layered security.

---

## 💼 Advanced Interview Questions & Answers

1. **Q:** How would you secure a hybrid cloud environment connecting AWS VPC and on-premises network?  
   **A:** Use VPN/Direct Connect with Virtual Private Gateway, enforce security groups and NACLs, and monitor traffic.

2. **Q:** What strategy would you use to plan CIDR blocks for multiple subnets and future growth?  
   **A:** Use large enough CIDR blocks, segment logically by application or AZ, and reserve ranges for expansion.

3. **Q:** How does Direct Connect improve performance over VPN?  
   **A:** Provides dedicated physical link, reducing latency, jitter, and dependence on public internet.

4. **Q:** Explain subnet-level and instance-level security in a VPC.  
   **A:** Subnet-level = Network ACLs; Instance-level = Security Groups. Together they provide layered defense.

5. **Q:** Describe a scenario using both private and public DNS in a VPC.  
   **A:** Internal services use private DNS for intra-VPC communication, while public-facing applications use public DNS via Route 53.

---

*AWS VPC Core Components – Organized and Documented.*  

---


