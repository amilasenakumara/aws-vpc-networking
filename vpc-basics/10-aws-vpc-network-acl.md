# 🛡️ AWS Network ACLs vs Security Groups – Learning Notes

This document provides a clear understanding of **AWS Network ACLs (NACLs)**, how they differ from **Security Groups (SGs)**, and how they work together to secure your VPC.

---

## 📌 Key Points Summary

### 🔹 Network ACL Basics
- Applied at **subnet level** → affects all instances inside a subnet.  
- Contains both **allow** ✅ and **deny** ❌ rules.  
- Rules are **numbered** → evaluated in order, first match is applied.  
- Rules can be numbered from `1` to `32766`.  
  - Best practice: use gaps (100, 200, 300) to insert new rules easily.  
- **Stateless**:
  - Must explicitly allow **inbound** and **outbound** traffic.  
  - Return traffic is not automatically allowed.  

---

### 🔹 Security Group vs Network ACL
| Feature | Security Group | Network ACL |
|---------|----------------|-------------|
| Level | **Instance/ENI level** | **Subnet level** |
| Rules | Only **allow** | Both **allow** and **deny** |
| Stateful/Stateless | **Stateful** (return traffic auto allowed) | **Stateless** (return traffic must be allowed separately) |
| Rule Evaluation | All rules evaluated | First matching rule is applied |
| Default | Deny all inbound, allow all outbound | Allows all inbound & outbound |

---

### 🔹 Default Network ACL
- Created automatically with each **VPC**.  
- By default:
  - Allows all **inbound** traffic.  
  - Allows all **outbound** traffic.  
  - Ends with an implicit **deny all rule** (`*`).  

---

### 🔹 Rule Examples
- **Explicit Deny**: Block malicious IP traffic with a deny rule.  
- **Inbound Example**:
  - Allow inbound HTTP traffic (`port 80`) from `1.2.3.4`.  
  - Must also configure outbound rules for return traffic.  
- **Outbound Example**:
  - EC2 initiating traffic to internet (`port 80/443`).  
  - Must allow return traffic from ephemeral ports (range depends on OS, e.g., 1024–65535).  

---

### 🔹 Ephemeral Ports
- Temporary ports chosen randomly by client OS when initiating connections.  
- Outbound rules must allow return traffic from **ephemeral port ranges**.  
- Example ranges:
  - Linux: `32768–60999`  
  - Windows: `49152–65535`  

---

### 🔹 Troubleshooting with NACLs
- If inbound works but outbound fails → check ephemeral port rules.  
- Always remember **stateless** nature of NACLs.  
- Use rule numbering wisely to manage precedence.  

---

## 🎓 Exam Questions & Answers

### 📘 Basic Exam Q&A
1. **Q:** At what level do Network ACLs operate?  
   **A:** At the **subnet level**.

2. **Q:** Can a Security Group explicitly deny traffic?  
   **A:** ❌ No, only Network ACLs can deny traffic.

3. **Q:** Why are NACLs considered stateless?  
   **A:** Return traffic must be explicitly allowed in outbound rules.

4. **Q:** What is the default rule at the end of every NACL?  
   **A:** An implicit **deny all** rule (`*`).

5. **Q:** What happens if no rule matches in NACL?  
   **A:** The traffic is denied by the default rule.

---

## 💼 Interview Questions & Answers

1. **Q:** Difference between NACL and Security Group?  
   **A:** NACL → Subnet level, stateless, allow & deny rules.  
   SG → Instance level, stateful, only allow rules.

2. **Q:** Why do we need ephemeral ports in NACL rules?  
   **A:** Because clients use random ports for return traffic.

3. **Q:** What’s the best practice for NACL rule numbering?  
   **A:** Use gaps (100, 200, 300) for easier future modifications.

4. **Q:** Can one subnet be associated with multiple NACLs?  
   **A:** ❌ No, each subnet can be associated with only one NACL at a time.

5. **Q:** What is the default behavior of a Security Group vs NACL?  
   **A:** SG: Deny inbound, allow outbound.  
   NACL: Allow all inbound/outbound.

---

## 📘 Advanced Exam Q&A

1. **Q:** How does AWS handle conflicting NACL rules?  
   **A:** The **first matching rule** (by rule number) is applied.

2. **Q:** What happens if you allow inbound traffic but forget outbound ephemeral port rules?  
   **A:** Return traffic will be blocked due to stateless behavior.

3. **Q:** Can you block a specific IP address using Security Groups?  
   **A:** ❌ No, only NACLs can block via deny rules.

4. **Q:** What’s the ephemeral port range for Linux vs Windows?  
   **A:** Linux: `32768–60999`, Windows: `49152–65535`.

5. **Q:** Why might you see intermittent failures with NACLs in web traffic?  
   **A:** Ephemeral port range not correctly configured in outbound rules.

---

## 💼 Advanced Interview Q&A

1. **Q:** How do NACLs and SGs complement each other?  
   **A:** NACLs provide subnet-wide stateless filtering; SGs provide instance-level stateful filtering.

2. **Q:** How would you block a malicious IP at the subnet level?  
   **A:** Add a **deny rule** in the NACL with a low rule number.

3. **Q:** Why is NACL not enough for protecting an EC2 instance?  
   **A:** NACL is stateless and coarse; SG provides finer instance-level security.

4. **Q:** If traffic is allowed in SG but denied in NACL, what happens?  
   **A:** Traffic is denied because NACL rules apply first at the subnet boundary.

5. **Q:** How would you design NACL rules for a public subnet hosting a web server?  
   **A:**  
   - Inbound: Allow HTTP/HTTPS from internet, allow ephemeral ports from client IPs.  
   - Outbound: Allow return traffic via ephemeral ports, allow outbound to internet (80/443).  

---

# AWS Network ACLs Complete Lecture Notes

Hi.  

So now that you understood AWS security groups in this lecture, let's understand the network ACLs and then how network ACL differs than the security group.  

So for your troubleshooting of the network it is important that you understand both security group and network ACL.  

So as I said network ACL sits at the subnet level.  

So if you see this diagram, all the traffic coming in and going out of the subnet is first monitored by the network ACL.  

And if it allows that traffic to go in then only it reaches to the EC2 machine.  

And before reaching to the EC2 instance it will first be authorized through the security group.  

Okay.  

## High-Level Overview

- Network ACLs are applied at the **subnet level**.  
- They apply to **all instances inside the subnet**.  
- Contains both **allow** and **deny** rules.  

> Important distinction:  
> Security groups only have **allow rules**, while network ACLs can have **allow and deny rules**.  

- Rules are **numbered**.  
- Evaluated **in order of rule number**, starting with the smallest.  
- Maximum rule number: 32766.  
- Best practice: use numbers like 100, 200, 300 for easier insertion later.  

### Stateless Nature

- Network ACL traffic is **stateless**.  
- Inbound traffic allowed does **not automatically allow outbound return traffic**.  
- Unlike security groups, return traffic must be explicitly allowed.  

### Default Network ACL

- Every subnet gets a **default network ACL**.  
- Default rules allow **both inbound and outbound traffic**.  
- Important: Network ACLs allow you to **block traffic from a particular IP address** (via deny rules).  

---

## Example: Network ACL Rules

### Inbound Rules

- Rule 100: Deny all IPv4 traffic from a malicious IP.  
- Star rule at the end: Deny all traffic not matching above rules (cannot be removed).  

### Outbound Rules and Ephemeral Ports

- Example: Allow TCP traffic to port 80 from source `1.2.3.4`.  
- **Return traffic** requires outbound rule to allow ephemeral ports (random high-numbered ports).  
- Ephemeral ports are used by the client to initiate connections.  
- If return traffic port is not allowed → packet is dropped.  
- Best practice: specify ephemeral port range or `all`.  

---

### Scenario Examples

1. **Implicit Deny:** Traffic from `9.10.11.12` without rule → blocked by star rule.  
2. **Explicit Deny:** Add rule to deny traffic from `9.10.11.12`.  
3. **Return Traffic Issue:** Inbound allowed, outbound not allowed → packet dropped. Must allow ephemeral ports for return.  
4. **Outbound Traffic Initiated by EC2:** Random ephemeral port used. Must explicitly allow inbound return traffic.  

---

### Default Rules Recap

- Inbound rules: allow all by default (IPv4/IPv6).  
- Outbound rules: allow all by default.  
- No configuration needed initially for default VPC/subnets.  

---

## Security Group vs Network ACL

| Feature | Security Group | Network ACL |
|---------|----------------|-------------|
| Applied at | EC2 instance | Subnet |
| Rules | Allow only | Allow & Deny |
| Stateful | Yes | No (stateless) |
| Rule evaluation | All rules evaluated | Numbered, first match applied |
| Return traffic | Automatic | Must explicitly allow |

> Note: Limits for rules and security groups change frequently. Check the **AWS VPC quota page** for the latest numbers.  

---

## AWS Console Walkthrough

1. Navigate to **VPC** in AWS console.  
2. Select a **VPC** (default or custom).  
3. Go to **Subnets** and select one.  
4. Go to **Network ACL** → view default inbound/outbound rules.  
5. Default IPv4 rules allow all traffic; default deny rule exists at the end.  

> Exercises in Section 2 will clarify these concepts further.  

---

Thanks for watching and stay tuned.

