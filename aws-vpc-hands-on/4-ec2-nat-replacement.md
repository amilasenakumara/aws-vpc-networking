# 🚀 Replacing NAT Gateway with Self-Managed EC2 NAT Instance

This exercise demonstrates how to replace an AWS-managed **NAT Gateway** with a **self-managed EC2 instance** configured as a NAT.  
The purpose is to understand how NAT works behind the scenes and to save cost (though **AWS NAT Gateway** is recommended for production due to scalability and reliability).

---

## 📌 Key Concepts
- NAT Gateway is **not magical** — you can configure your own EC2 instance to act as NAT.
- AWS NAT Gateway:
  - Scales automatically.
  - Supports **up to 100 Gbps bandwidth**.
  - Incur **extra charges**.
- Self-managed EC2 NAT:
  - Cost-saving.
  - Requires manual setup & configuration.
  - Suitable for **learning/demo**, not production.

---

## 📝 Step-by-Step Process

### 1️⃣ Clean Up NAT Gateway
- Delete the **NAT Gateway**.
- Release the **Elastic IP** associated with it.
- Remove the **route table entry** in the private subnet pointing to NAT Gateway.
- Blackhole routes (due to deleted NAT) should be removed.

---

### 2️⃣ Launch NAT EC2 Instance
- Launch a new **EC2 instance** in the **Public Subnet**.
- Use a **NAT AMI** (Amazon-provided from Community AMIs).
- Configure:
  - Instance type: **t2.micro** (free tier).
  - Networking: Place in **VPC Public Subnet**.
  - Assign **Public IP** (Elastic IP recommended).
  - Attach proper **Security Group**.
  - SSH Key Pair: existing key.

---

### 3️⃣ Security Group Configuration
- Allow traffic from **Private Subnet (VPC CIDR)**:
  - HTTP (80)
  - HTTPS (443)
  - ICMP (Ping)
- This enables EC2-B (private subnet) to route outbound traffic via EC2-NAT.

---

### 4️⃣ Disable Source/Destination Check
- By default, EC2 only processes traffic addressed **to itself**.
- For NAT, EC2 must handle traffic **not destined to its own IP**.
- Disable **Source/Destination Check**:
  - Go to **Actions → Networking → Change Source/Dest Check → Disable**.

---

### 5️⃣ Update Private Subnet Route Table
- Edit **Private Subnet Route Table**.
- Add a route:
  - Destination: `0.0.0.0/0` (Internet)
  - Target: **NAT EC2 instance (ENI)**
- This ensures private EC2 instances send internet traffic via the NAT EC2.

---

### 6️⃣ Test Connectivity
- From **Private EC2 (EC2-B)**:
  - Try `ping google.com`.
  - Test `curl http://example.com`.
- Internet traffic should now flow through the **NAT EC2 instance**.

---

### 7️⃣ Cleanup
- If not continuing, delete unused resources:
  - NAT EC2 instance.
  - Route table entries.
  - Elastic IPs.
- Remember **Free Tier = 750 hours/month** across all t2.micro instances.

---

## 📊 Architecture Diagram
```plaintext
Private Subnet (EC2-B) ---> NAT EC2 Instance (Public Subnet) ---> Internet Gateway ---> Internet
```

# 📚 Questions & Answers

---

## 📝 Exam Questions (with Answers)

**Q: Why replace a NAT Gateway with a self-managed EC2 NAT?**  
A: To save costs and understand how NAT works internally.

**Q: Which feature must be disabled on a NAT EC2 instance?**  
A: Source/Destination Check.

**Q: Which AMI should be used for a NAT EC2 instance?**  
A: AWS-provided NAT AMI from Community AMIs.

**Q: What traffic types should be allowed in the NAT EC2 Security Group?**  
A: HTTP (80), HTTPS (443), and ICMP (ping).

**Q: In which subnet should the NAT EC2 instance be launched?**  
A: Public Subnet.

---

## 💼 Interview Questions (with Answers)

**Q: Difference between NAT Gateway and EC2 NAT instance?**  
A: NAT Gateway is AWS-managed, scalable, reliable; EC2 NAT is manual, cost-saving, but less scalable.

**Q: Why disable source/dest check in NAT EC2?**  
A: Because NAT forwards traffic not destined for itself.

**Q: Can private EC2 instances access the internet without NAT?**  
A: No, they need a NAT (or proxy) to forward traffic.

**Q: Why attach Elastic IP to NAT EC2?**  
A: To maintain a consistent public IP even after restarts.

**Q: What happens if NAT Gateway is deleted but route table still points to it?**  
A: Route shows **Blackhole**, meaning unreachable.

---

## 🧠 Advanced Exam Questions (with Answers)

**Q: How does a NAT device modify packets?**  
A: It replaces the source IP with its own public IP and translates responses back to the private IP.

**Q: Can multiple private subnets use a single NAT EC2?**  
A: Yes, if route tables point to the NAT EC2.

**Q: What are the scalability limitations of EC2 NAT vs NAT Gateway?**  
A: EC2 NAT is limited by instance size; NAT Gateway scales automatically up to 100 Gbps.

**Q: What happens if source/dest check is not disabled?**  
A: NAT EC2 will drop packets not addressed to its own IP.

**Q: Why should you prefer NAT Gateway for production?**  
A: Better reliability, scalability, and AWS management.

---

## 🔥 Advanced Interview Questions (with Answers)

**Q: Explain packet flow in NAT EC2 for outbound internet requests.**  
A: Private EC2 → NAT EC2 (SNAT replaces private IP with NAT’s public IP) → Internet → Response back → NAT EC2 → Private EC2.

**Q: How would you make NAT EC2 highly available?**  
A: Launch multiple NAT EC2s in different AZs + configure route tables for failover.

**Q: How do NACLs affect traffic in NAT setup?**  
A: NACLs must allow outbound/inbound traffic for NAT EC2 to function.

**Q: Can a NAT EC2 be used for inbound connections?**  
A: No, NAT is for outbound only. For inbound, use Load Balancer or Bastion Host.

**Q: What’s the difference between Elastic IP and Auto-assign Public IP in NAT EC2?**  
A: Elastic IP remains static across restarts, Auto-assign changes on stop/start.
