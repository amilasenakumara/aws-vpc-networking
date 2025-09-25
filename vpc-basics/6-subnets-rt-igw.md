# AWS VPC Subnets, Routing & Internet Gateway Deep Dive 🌐

This repository contains notes on **AWS VPC subnets, routing, and internet gateway** concepts.  
It explains **how traffic flows** within a VPC and between subnets and the internet.Subnets, Route Tables and Internet Gateway

🔗[Subnets, Route Tables and Internet Gateway](https://github.com/amilasenakumara/aws-vpc-networking/blob/a216052de402202780f3773b2f8785611fe29572/images/subnets-rt-igw.png)

---

## 1️⃣ Introduction

- Understanding VPC CIDR and subnet allocation is essential.  
- Next step: understand **subnet communication** and **internet connectivity**.  
- Key focus: how **routing tables** control traffic flow inside VPC.

---

## 2️⃣ VPC & Subnets Overview

- AWS region comes with multiple **Availability Zones (AZs)**.  
- Create a **VPC** and associate a **CIDR block**.  
- AWS automatically provisions a **local router** inside the VPC.  
- Create **subnets** (can be in same or different AZs).  
- Example:
  - Subnet A → AZ1  
  - Subnet B → AZ2  
- EC2 instances launched in a subnet receive **private IPs** from subnet range.  

---

## 3️⃣ Traffic Between Subnets

- EC2 → EC2 traffic depends on **route table** configuration.  
- Every VPC comes with a **main route table**:
  - Has a **default route** for local VPC traffic.  
  - Destination: VPC CIDR  
  - Target: **local router**  
- Result: **instances within VPC can communicate by default**.

---

## 4️⃣ Internet Connectivity

- By default, EC2 instances **cannot reach the internet**.  
- Requirements for internet access:
  1. **Internet Gateway (IGW)** associated with VPC.  
  2. **Route table entry** for `0.0.0.0/0` pointing to IGW.  
  3. **Public IP** assigned to instance.  
- Without all three, traffic to internet is dropped.  

---

## 5️⃣ Public vs Private Subnets

- **Public subnet:** route table includes **internet gateway**.  
- **Private subnet:** route table **does not include internet gateway**.  
- Use case:
  - Public subnet → web servers (internet accessible)  
  - Private subnet → app servers, databases (internal only)

---

## 6️⃣ Subnet Specific Route Tables

- Subnet can have **custom route table**:
  - Overrides main route table.  
  - Useful to restrict certain subnets from internet access.  
- Only **one route table per subnet**.  
- Example:
  - Subnet A (web server) → custom route table with IGW  
  - Subnet B (database) → custom route table without IGW  
- Helps **control traffic flow per subnet**.

---

## 7️⃣ Route Table Concepts

- **Main route table:** default for all subnets.  
- **Custom route tables:** subnet-specific routing.  
- Each route table has **immutable local route** for VPC CIDR.  
- Additional routes needed for:
  - Internet (via IGW)  
  - On-premises network (via VPN Gateway)  
- **No custom route table → subnet follows main route table.**

---

## 8️⃣ Reserved IPs in Subnets

- AWS reserves **5 IPs per subnet**:
  1. First address → network address  
  2. Second → reserved by AWS  
  3. Third → DNS  
  4. Fourth → future use  
  5. Last → broadcast (not used)  
- Example:
  - `/24` subnet → 256 total IPs → 251 usable IPs  
  - `/28` subnet → 16 total IPs → 11 usable IPs  

---

## 9️⃣ AWS Console Notes

- **Main route table** automatically associated with new subnets.  
- You can **create custom route tables** and associate with specific subnets.  
- Modify route table for static routes as needed (internet, VPN, etc.).  

---

## 🔟 Summary Points

1. VPC comes with **main route table** and **local route** by default.  
2. EC2 instances in same VPC can communicate via **local route**.  
3. Internet access requires **IGW, route entry, public IP**.  
4. Subnets can have **custom route tables** to control traffic.  
5. Public vs private subnets determined by **route table entries**.  
6. AWS reserves **5 IPs per subnet** for internal use.  
7. Proper subnet planning is essential for traffic management.

---

## 📚 Exam Questions & Answers

1. **Q:** Can an EC2 instance communicate with another EC2 in same VPC by default?  
   **A:** Yes, via the local route in the main route table.  

2. **Q:** What are the three conditions for internet access from an EC2 instance?  
   **A:** Internet Gateway, route table entry, public IP.  

3. **Q:** What is a public subnet?  
   **A:** Subnet with a route to the internet via IGW.  

4. **Q:** How many IPs are usable in a `/24` subnet?  
   **A:** 251 usable IPs (256 total - 5 reserved).  

5. **Q:** Can a subnet have its own route table?  
   **A:** Yes, subnet can have a custom route table.  

---

## 💼 Interview Questions & Answers

1. **Q:** What is the purpose of a main route table?  
   **A:** Provides default routing for all subnets in the VPC.  

2. **Q:** How do you restrict a subnet from internet access?  
   **A:** Create a custom route table without IGW and associate it with the subnet.  

3. **Q:** Why does AWS reserve 5 IPs per subnet?  
   **A:** For network address, broadcast, DNS, future use, and AWS internal purposes.  

4. **Q:** What determines if a subnet is public or private?  
   **A:** Presence of IGW route in its route table.  

5. **Q:** Can all subnets share a single route table?  
   **A:** Yes, they will follow the main route table by default.

---

## 🧮 Advanced Exam Questions & Answers

1. **Q:** What is the usable IP range in a `/28` subnet?  
   **A:** 16 total IPs - 5 reserved → 11 usable IPs.  

2. **Q:** What happens if a subnet has no route table entry for the internet?  
   **A:** Instances cannot access the internet.  

3. **Q:** Can a subnet override the main route table?  
   **A:** Yes, by associating a custom route table.  

4. **Q:** How many route tables can a subnet have?  
   **A:** Only one route table per subnet.  

5. **Q:** What are the three key elements needed for public subnet internet access?  
   **A:** Internet Gateway, route table entry to 0.0.0.0/0, public IP on instance.  

---

## 🚀 Advanced Interview Questions & Answers

1. **Q:** How would you design subnet routing for a multi-tier application?  
   **A:** Public subnets for web servers with IGW, private subnets for app/db servers without IGW.  

2. **Q:** How can you control internet access for specific subnets?  
   **A:** Create subnet-specific route tables and exclude IGW.  

3. **Q:** Explain immutable local route in AWS route table.  
   **A:** Automatically allows traffic between all subnets in VPC, cannot be deleted.  

4. **Q:** How does AWS handle route table associations for new subnets?  
   **A:** By default, new subnets are associated with main route table unless a custom route table is specified.  

5. **Q:** How can you provide internet access only to web server subnets?  
   **A:** Associate a custom route table with IGW only to web server subnets.  

---

**Suggested file name:**  
