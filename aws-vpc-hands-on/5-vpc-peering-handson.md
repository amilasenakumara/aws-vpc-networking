# 🌐 AWS VPC Peering Setup Guide

This exercise demonstrates **VPC peering**, allowing two VPCs to communicate privately without exposing EC2 instances to the internet.

---

## 📖 What is VPC Peering?
- **Definition**: Connects two VPCs privately over private IP addresses.  
- **Purpose**: Allows EC2 instances in different VPCs to communicate without using public IPs or the internet.  
- **Use Case**: Different departments hosting internal applications in separate VPCs can communicate securely.  
- **Types**:
  - **Intra-region peering**: Between VPCs in the same region.  
  - **Inter-region peering**: Between VPCs in different AWS regions.  

---

## ⚡ Key Points About VPC Peering
- VPC CIDR blocks **must not overlap**.  
- **Point-to-point** connection (not transitive).  
- Maximum VPC peering connections per VPC:
  - Default: 50  
  - Maximum: 125  
- Traffic flows **over AWS backbone network** and is **encrypted** even across regions.  

---

## 🛠️ Steps to Setup VPC Peering

### 1️⃣ Prepare Existing VPC Setup
- Clean up previous setups (e.g., remove NAT EC2 instances and private route table entries).  
- Ensure public and private subnets exist for VPC A.  

### 2️⃣ Create VPC A (Mumbai Region)
- **Create VPC**: Assign CIDR (e.g., `10.0.0.0/16`).  
- **Create Internet Gateway** and attach to VPC A.  
- **Create Subnets**:
  - Public subnet (CIDR /24, enable auto-assign public IP)  
  - Private subnet (CIDR /24)  
- **Create Route Tables**:
  - Public: Add route to Internet Gateway. Associate with public subnet.  
  - Private: Associate with private subnet (no internet route).  
- **Launch EC2 Instances**:
  - Public EC2 A: Public subnet, security group allows SSH from anywhere.  
  - Private EC2 B: Private subnet, security group allows SSH & ICMP only from VPC A CIDR.  

### 3️⃣ Create VPC B (North Virginia Region)
- **Create VPC B** with a unique CIDR (e.g., `10.100.0.0/16`).  
- **Create Private Subnet**.  
- **Create Route Table** and associate with the subnet.  
- **Launch EC2 Instance C** in private subnet.  
  - Security group allows SSH & ICMP from VPC A CIDR.  

### 4️⃣ Test Connectivity
- Connect to EC2 B from EC2 A.  
- Ping EC2 C → will fail before peering.  

### 5️⃣ Create VPC Peering Connection
- In VPC A (Mumbai) console, create a peering request:
  - Requester: VPC A  
  - Target VPC: VPC B (copy VPC ID)  
- Accept the peering request in VPC B (North Virginia).  

### 6️⃣ Update Route Tables
- **VPC A Private Route Table**: Add route to VPC B via peering connection.  
- **VPC B Private Route Table**: Add route to VPC A via peering connection.  

### 7️⃣ Verify Connectivity
- From EC2 B, ping EC2 C → should succeed.  
- This confirms successful **VPC peering setup**.  

### 8️⃣ Clean Up
- Delete instances and VPCs when not in use to avoid unnecessary charges.  

---

## 📚 Questions & Answers

### 📝 Exam Questions
1. **What is VPC peering?**  
   Private connection between two VPCs allowing traffic via private IPs.

2. **Can VPC peering be transitive?**  
   No, it is strictly point-to-point.

3. **Can VPC peering be across regions?**  
   Yes, inter-region VPC peering is possible.

4. **Do VPCs with overlapping CIDRs support peering?**  
   No, CIDRs must be unique.

5. **Does traffic through VPC peering go over the internet?**  
   No, it travels over AWS backbone and is encrypted.  

---

### 💼 Interview Questions
1. **Why use VPC peering instead of public IP access?**  
   Ensures secure private communication without exposing instances to the internet.

2. **How many VPC peering connections can a VPC have?**  
   Default: 50, Maximum: 125.

3. **What is required on route tables after peering?**  
   Add routes to the peered VPC via the peering connection on both sides.

4. **Can peering be established between VPCs in different AWS accounts?**  
   Yes, requires providing the AWS account ID of the target VPC.

5. **Why does traffic remain secure in inter-region peering?**  
   It uses AWS backbone network and is encrypted.  

---

### 🧠 Advanced Exam Questions
1. **How do private and public subnets interact in VPC peering?**  
   Only private subnets can communicate via peering; public subnet connectivity remains internet-based.

2. **Can you restrict traffic in VPC peering?**  
   Yes, via security groups and NACLs.

3. **What happens if route tables are not updated after peering?**  
   Traffic will not flow between VPCs despite the peering connection.

4. **Is it possible to have multiple peering connections between the same VPC pair?**  
   No, each VPC pair can only have one active peering connection.

5. **How does AWS ensure security for cross-region peering?**  
   Traffic stays on AWS private backbone and is encrypted in transit.  

---

### 🔥 Advanced Interview Questions
1. **Explain step-by-step how to set up cross-region VPC peering.**  
   - Create VPCs in respective regions.  
   - Launch subnets and EC2 instances.  
   - Request peering from VPC A → accept in VPC B.  
   - Update route tables on both sides.  
   - Verify connectivity between EC2 instances.  

2. **Can VPC peering work with VPN or Direct Connect?**  
   Yes, VPC peering works alongside VPN/Direct Connect for private connectivity.

3. **How to allow only specific instances access over VPC peering?**  
   Use security group rules with specific IPs or CIDRs.

4. **Can VPC peering affect latency?**  
   Slightly, depending on inter-region distance; intra-region is minimal.

5. **How to troubleshoot VPC peering connectivity issues?**  
   Check CIDR overlap, route tables, security groups, and peering acceptance status.  

---

# ✅ Suggested File Name
`aws-vpc-peering-guide.md`
