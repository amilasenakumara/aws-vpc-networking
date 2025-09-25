# 🔐 AWS VPC Security Groups

## 📘 Introduction
In AWS VPC, two types of firewalls exist:
- **Security Groups (SGs)** → Instance-level firewall  
- **Network ACLs (NACLs)** → Subnet-level firewall  

This document focuses on **Security Groups**, their features, use cases, and best practices.

---

## 📝 Key Concepts

### 🔄 Firewall Layers
1. **Network ACL (Subnet-level)**
   - First layer of defense.
   - Evaluates traffic before it reaches instances.
2. **Security Groups (Instance-level)**
   - Second layer of defense.
   - Decides whether traffic is allowed into/out of the instance.

---

## ⚡ Features of Security Groups
- 🖥️ Attached to **ENIs (Elastic Network Interfaces)**, not directly to EC2.
- ➡️ **Inbound Rules** → Control incoming traffic.
- ⬅️ **Outbound Rules** → Control outgoing traffic.
- ✅ Only **Allow rules** (no deny/block rules).
- 🌍 Support both **IPv4 and IPv6**.
- 🔄 **Stateful**:
  - If inbound traffic is allowed, return traffic is automatically allowed.
  - If outbound traffic is allowed, return traffic is automatically allowed.
- 🔗 Can **reference other Security Groups** in rules.
- 🛠️ Default Security Group:
  - No inbound traffic allowed (except self-referencing rules).
  - All outbound traffic allowed.

---

## 📌 Security Group Behavior & Examples

### 1. 🛠️ SSH Access from Home
- Add **inbound rule**: Allow TCP on port 22 from your home IP.  
- If home IP changes → must update SG rules.  

### 2. 🌍 SSH from Multiple Locations
- Option 1: Add multiple source IPs.  
- Option 2: Use `0.0.0.0/0` → allows from anywhere (not secure).  

### 3. 🚫 No Deny Rule Support
- SGs cannot block specific IPs.  
- Only **Network ACLs** can be used for explicit deny rules.  

### 4. 🌐 Outbound Internet Access
- Outbound rules required for HTTP/HTTPS traffic.  
- Example: Allow TCP port `80/443` to `0.0.0.0/0`.  

### 5. 🖥️ EC2-to-EC2 Communication
- EC2-A outbound rule → target EC2-B IP.  
- EC2-B inbound rule → allow port 22 from EC2-A.  
- Return traffic automatically allowed (stateful behavior).  

### 6. 🏢 Intra-VPC Communication
- Use **VPC CIDR range** in rules to allow communication among all VPC instances.  

### 7. 🔗 Security Group References
- Instead of adding individual IPs → reference **another SG**.  
- Common for **web servers → app servers** communication.  

---

## 🛡️ Default Security Group Rules
- **Inbound**:
  - Allows traffic from itself (self-referencing rule).
- **Outbound**:
  - All IPv4 traffic allowed.
  - If IPv6 enabled, all IPv6 traffic allowed.  

---

## ✅ Summary
- Security Groups apply at **ENI level** (commonly seen as EC2).  
- **Inbound = blocked by default**, **Outbound = allowed by default**.  
- They are **stateful** firewalls.  
- Best practice: Use **SG references** instead of IPs for dynamic scaling.  

---

## 📚 Exam Questions (Basic)

1. **Q:** What level does a Security Group operate at?  
   **A:** Instance level (ENI/EC2).  

2. **Q:** Are Security Groups stateful or stateless?  
   **A:** Stateful.  

3. **Q:** Can you create deny rules in a Security Group?  
   **A:** No, only allow rules are possible.  

4. **Q:** What happens if inbound traffic is allowed in a SG?  
   **A:** Return outbound traffic is automatically allowed.  

5. **Q:** What is the default outbound rule in a Security Group?  
   **A:** All traffic allowed to all destinations (`0.0.0.0/0`).

---

## 💼 Interview Questions (Basic)

1. **Q:** Difference between Security Groups and Network ACLs?  
   **A:** SG = Instance-level, stateful, only allow. NACL = Subnet-level, stateless, allow + deny.  

2. **Q:** Can one EC2 instance have multiple Security Groups?  
   **A:** Yes, up to 5 SGs by default.  

3. **Q:** Why is `0.0.0.0/0` risky in SGs?  
   **A:** It allows access from all IPs, creating a security risk.  

4. **Q:** Can SGs reference another SG?  
   **A:** Yes, SGs can reference other SGs as a source/destination.  

5. **Q:** What is a default Security Group?  
   **A:** A pre-created SG in every VPC with restrictive inbound and permissive outbound rules.  

---

## 📘 Advanced Exam Questions

1. **Q:** Explain how Security Groups handle ephemeral ports in outbound connections.  
   **A:** They allow return traffic automatically without explicit rules due to stateful nature.  

2. **Q:** How to allow communication between Auto Scaling group instances?  
   **A:** Reference the Auto Scaling group’s SG inside itself.  

3. **Q:** What happens if you remove all outbound rules in a SG?  
   **A:** No outbound traffic will be allowed.  

4. **Q:** If EC2-A is in SG-A and EC2-B is in SG-B, how to allow A to talk to B?  
   **A:** Add an inbound rule in SG-B allowing traffic from SG-A.  

5. **Q:** How do Security Groups handle IPv6 differently?  
   **A:** SG rules must explicitly allow IPv6 CIDRs (e.g., `::/0`).

---

## 💼 Advanced Interview Questions

1. **Q:** Why are Security Groups called "stateful"?  
   **A:** Because return traffic is automatically allowed, reducing rule complexity.  

2. **Q:** How do SGs differ from OS-level firewalls (like iptables)?  
   **A:** SGs operate at the AWS network virtualization layer, not inside the OS.  

3. **Q:** How would you design SGs for a 3-tier architecture (web, app, DB)?  
   **A:** Reference SGs for controlled communication (Web SG → App SG → DB SG).  

4. **Q:** What is the benefit of referencing Security Groups instead of CIDR ranges?  
   **A:** Simplifies management and supports dynamic scaling.  

5. **Q:** Can Security Groups span across VPCs?  
   **A:** No, they are limited to the VPC they are created in.  

---


