# AWS VPC IP Addressing – IPv4 and IPv6 📚

This document contains **learning notes** on AWS VPC IP addressing, covering **IPv4 private, public, and Elastic IPs**, as well as **IPv6 global addressing**.  
Purpose: Understand the behavior, scope, and use cases of different IP address types in AWS.

---

## 1️⃣ Types of IP Schemes
- **IPv4**  
  - Private IP  
  - Public IP  
  - Elastic IP  
- **IPv6**  
  - All addresses are globally unique and public  

---

## 2️⃣ Private IPv4 Addresses
- Allocated from **subnet CIDR range** when EC2 is launched.  
- Managed by **Elastic Network Interface (ENI)**.  
- Remain associated with the EC2 instance for its lifetime.  
- Example:  
  - Subnet CIDR → `10.10.0.0/24`  
  - Usable IPs → 251 (first 4 + last 1 reserved).  

---

## 3️⃣ Public IPv4 Addresses
- Assigned from AWS’s pool of public IPs.  
- Provide internet connectivity (with IGW + route).  
- Characteristics:  
  - Automatically allocated, not chosen by user.  
  - Released when instance is stopped.  
  - New IP assigned on restart.  

⚠️ Limitation → Changing IP causes connectivity issues for external clients.

---

## 4️⃣ Elastic IPv4 Addresses (EIP)
- **Static public IP** associated with AWS account.  
- Persists across stop/start of instances.  
- Can be **remapped** to another EC2 instance for failover or migration.  
- Ensures clients always connect to the same IP.  

---

# Private, Public and Elastic IP (EIP) 📊

| Feature                  | Private IP                                                                 | Public IP                                                                                     | Elastic IP (EIP)                                                                 |
|--------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **Communication**        | Communication within VPC                                                   | Can communicate over internet                                                                 | Can communicate over internet                                                     |
| **Address range**        | Gets IP address from subnet range. <br>Ex: `10.200.0.1`                     | Gets IP address from Amazon Pool within region                                                 | Gets IP address from Amazon Pool within region                                    |
| **Instance stop/start**  | Once assigned cannot be changed                                             | Changes over instance stop and start <br>(not on instance OS reboot)                          | Do not change over instance stop and start                                        |
| **Releasing IP**         | Released when instance is terminated                                        | Released to pool when instance is stopped or terminated                                       | Not released. Remains in your account. <br>(Billed)                               |
| **Automatic Assignment** | Receives private IP on launch of EC2 instance                               | Receives public IP on launch of EC2 instance if *Public IP addressing attribute* is set to true for subnet | Have to explicitly allocate and attach EIP to EC2 instance. <br>Can be reattached to other EC2 |
| **Examples**             | Application servers, databases                                              | Web servers, Load Balancers, Websites                                                         | Web servers, Load Balancers, Websites                                             |


---

## 5️⃣ IPv6 Addresses
- **128-bit addresses**, written in hexadecimal (8 blocks of 16 bits).  
- VPC prefix → `/56`, Subnet prefix → `/64`.  
- All are **public and globally unique** (no private/public distinction).  
- Behavior:  
  - Persist during stop/start (like Elastic IP).  
  - Released when EC2 is terminated.  
- Supports **dual stack mode** (resources can have both IPv4 + IPv6).  

---

## 6️⃣ Key Comparisons
- **Private IPv4** → Internal only, persists with instance.  
- **Public IPv4** → Internet access, changes on stop/start.  
- **Elastic IP** → Static public IP, remappable, permanent.  
- **IPv6** → Public + globally unique, works like static IPs.  

---

## 7️⃣ Summary
- IPv4 has **private, public, elastic**.  
- IPv6 has **only public, globally unique addresses**.  
- Elastic IPs solve the problem of **changing public IPs**.  
- Dual stack allows simultaneous IPv4 + IPv6 addressing.  

---

## 8️⃣ Exam Questions (with Answers)

**Q1:** What is the difference between private and public IPv4 addresses in AWS?  
**A1:** Private IPs are from subnet CIDR and used within VPC; public IPs are from AWS pool and enable internet access.  

**Q2:** What happens to a public IP when an EC2 instance is stopped?  
**A2:** It is released back to AWS pool, and a new one is assigned when restarted.  

**Q3:** What is the purpose of an Elastic IP?  
**A3:** Provides a permanent public IP that persists across stop/start and can be remapped.  

**Q4:** How many usable IPs exist in a `/24` subnet?  
**A4:** 251 (256 total − 5 reserved).  

**Q5:** What is the CIDR prefix for IPv6 subnets in AWS?  
**A5:** `/64`.  

---

## 9️⃣ Interview Questions (with Answers)

**Q1:** Why would you use an Elastic IP instead of a public IP?  
**A1:** To maintain static connectivity for clients and avoid IP changes on stop/start.  

**Q2:** Can you assign a specific private IP to an EC2 instance?  
**A2:** Yes, you can manually assign one during launch.  

**Q3:** How do Elastic Network Interfaces (ENIs) relate to IPs?  
**A3:** ENIs manage private, public, and Elastic IPs for EC2 instances.  

**Q4:** How is IPv6 addressing different from IPv4 in AWS?  
**A4:** IPv6 addresses are globally unique and always public, unlike IPv4’s private/public split.  

**Q5:** Can IPv6 addresses persist after stopping and starting instances?  
**A5:** Yes, they persist like Elastic IPs, but are released when instance is terminated.  

---

## 🔟 Advanced Exam Questions (with Answers)

**Q1:** How many Elastic IPs can you allocate by default per region?  
**A1:** 5 Elastic IPs per region (limit can be increased).  

**Q2:** Which AWS service allows assigning multiple IP addresses to a single instance?  
**A2:** Secondary private IPs via ENIs.  

**Q3:** What’s the main drawback of using Elastic IPs at scale?  
**A3:** Limited availability, potential charges for unused Elastic IPs.  

**Q4:** In dual-stack mode, how does DNS resolve requests?  
**A4:** DNS can return both A (IPv4) and AAAA (IPv6) records; client decides which to use.  

**Q5:** Why does AWS encourage IPv6 adoption?  
**A5:** Exhaustion of IPv4 public addresses and need for global uniqueness.  

---

## 1️⃣1️⃣ Advanced Interview Questions (with Answers)

**Q1:** How would you design a fault-tolerant architecture using Elastic IPs?  
**A1:** Use Elastic IPs remapped to standby EC2 instances or autoscaling groups in failover scenarios.  

**Q2:** What happens if two different VPCs use the same private IPv4 CIDR range?  
**A2:** Overlapping IP conflicts prevent direct routing unless NAT, VPC peering, or Transit Gateway with translation is used.  

**Q3:** Can IPv6 be used with NAT Gateway in AWS?  
**A3:** No, NAT Gateway supports IPv4 only; IPv6 has direct internet access with IGW.  

**Q4:** How do you control inbound traffic for IPv6-enabled resources?  
**A4:** Using security groups and NACLs configured with IPv6 CIDR rules.  

**Q5:** In what case would you assign multiple Elastic IPs to a single instance?  
**A5:** For hosting multiple SSL websites on one server or for advanced networking requirements.  

---

*AWS Networking Notes – IP Addressing (Private, Public, Elastic, IPv6)*  

