# 🌐 AWS NAT Gateway Lecture Notes

---

## Introduction
  

In this lesson, we will understand another important component of the **VPC**: the **NAT Gateway**.

---

## Public vs Private Subnets
- **Public Subnet:**  
  - Instances can communicate with the internet directly.  
  - Route table includes a route via **Internet Gateway (IGW)**.  
- **Private Subnet:**  
  - Instances **cannot** directly communicate with the internet.  
  - Route table has only a **local route** (communication within VPC only).  

💡 **Key point:** Private subnet instances require a NAT to access the internet.

---

## Why Private Subnet Cannot Access Internet
1. Subnet is **private** → no route to IGW.  
2. Instance **does not have a public IP** → even if subnet was public, communication wouldn’t be possible.  

---

## NAT Gateway Overview
- NAT = **Network Address Translation**.  
- Allows **outbound internet access** for instances in **private subnets**.  
- NAT Gateway sits in a **public subnet** with a **public/elastic IP**.  
- Acts as a **proxy** for private instances.  

---

## Routing via NAT Gateway
- **Step 1:** Modify private subnet route table → all internet-bound traffic goes via NAT Gateway.  
- **Step 2:** Traffic flow:
  1. Instance B sends packet → NAT Gateway.  
  2. NAT Gateway replaces **source IP** with its own public IP.  
  3. Packet goes to the internet.  
  4. Internet returns traffic → NAT Gateway.  
  5. NAT Gateway forwards traffic to **original private instance**.  

💡 **Key point:** Private IP of instance B is **not routable over the internet**; NAT handles translation.

---

## NAT Gateway Features
- **AWS Managed:** High availability, fully managed, no need for security group management.  
- **Scalability:** Up to **100 Gbps**, may increase in the future.  
- **Cost:** Per hour + data processed (per GB).  
- **Network ACLs:** Still apply, since NAT sits in subnets.  
- **Protocols Supported:** TCP, UDP, ICMP (ping).  
- **Ports:** Uses **ephemeral port range** for outbound connections.  
- **Elastic IP Required:** Ensures IP remains unchanged during AWS maintenance.  

---

## NAT Gateway & High Availability
- NAT is **AZ-specific** (created in a specific subnet).  
- Other subnets in the same VPC can route through the NAT Gateway.  
- **Limitation:** If AZ goes down → NAT Gateway goes down.  
- **Solution:** Deploy multiple NAT Gateways across **multiple AZs** for production workloads.  

💡 **Trade-off:** Higher cost vs high availability.

---

## NAT vs NAT Instance
- Next lecture will cover **NAT Instance**: using your own EC2 as NAT instead of AWS-managed NAT Gateway.

---

## Summary
- NAT Gateway = **essential for private subnet internet access**.  
- Fully managed by AWS → easy scaling, high availability, and protocol support.  
- **Network ACLs still apply**, security groups not required for NAT Gateway.  
- **Multiple NAT Gateways** across AZs = high availability but additional cost.  

---

## ✅ Key Highlights
- Private subnet **cannot access internet directly**.  
- NAT Gateway sits in **public subnet** with **elastic IP**.  
- NAT Gateway **replaces source IP** of outbound traffic.  
- **Return traffic** is routed back to private instance.  
- **Network ACLs still apply**, security groups not needed for NAT Gateway.  
- Multiple NAT Gateways = **high availability**, higher cost.

---

## 📚 Exam Questions & Answers

1. **Q:** What does NAT stand for?  
   **A:** Network Address Translation  

2. **Q:** Can a private subnet instance access the internet without NAT?  
   **A:** No  

3. **Q:** Where does NAT Gateway sit?  
   **A:** In a **public subnet**  

4. **Q:** Does NAT Gateway have security groups?  
   **A:** No, only **network ACLs apply**  

5. **Q:** Why is elastic IP required for NAT Gateway?  
   **A:** Ensures IP remains **unchanged during AWS maintenance**  

---

## 📝 Interview Questions & Answers

1. **Q:** Explain how NAT Gateway works in AWS.  
   **A:** NAT Gateway allows outbound internet access for instances in private subnets by replacing their private IP with the NAT’s public IP and forwarding return traffic back.  

2. **Q:** Difference between NAT Gateway and NAT Instance.  
   **A:** NAT Gateway is AWS-managed, fully scalable, highly available, and requires no security group management. NAT Instance is self-managed EC2 acting as NAT, with security groups and scaling limitations.  

3. **Q:** How is high availability achieved with NAT Gateway?  
   **A:** By deploying **multiple NAT Gateways across multiple Availability Zones (AZs)**.  

4. **Q:** Can a private subnet instance receive inbound traffic from internet via NAT Gateway?  
   **A:** No, NAT Gateway only allows **outbound access**; private IP is not routable.  

5. **Q:** What protocols does NAT Gateway support?  
   **A:** TCP, UDP, ICMP (ping).  

---

## 🧠 Advanced Exam Questions & Answers

1. **Q:** How does NAT Gateway handle ephemeral ports?  
   **A:** NAT Gateway automatically assigns ephemeral ports for outbound connections to manage multiple simultaneous connections.  

2. **Q:** Explain traffic flow from a private instance to internet via NAT Gateway.  
   **A:** Instance → NAT Gateway (source IP replaced) → Internet → NAT Gateway → Instance.  

3. **Q:** What happens if NAT Gateway is in a single AZ and that AZ fails?  
   **A:** NAT Gateway becomes unavailable → private instances in other subnets may lose internet access.  

4. **Q:** How do network ACLs interact with NAT Gateway?  
   **A:** Network ACLs still apply because NAT Gateway sits in subnets; correct rules must allow inbound and outbound traffic.  

5. **Q:** Why would you choose NAT Instance over NAT Gateway?  
   **A:** To have full control, use security groups, or for cost-saving in small workloads.

---

## 💼 Advanced Interview Questions & Answers

1. **Q:** How can NAT Gateway improve security in VPC architecture?  
   **A:** It hides private subnet IPs from the internet and only exposes NAT’s public IP.  

2. **Q:** Discuss cost implications of deploying multiple NAT Gateways across AZs.  
   **A:** Each NAT Gateway incurs **hourly + data transfer charges**; high availability increases cost.  

3. **Q:** How does NAT Gateway maintain mapping of private to public IPs?  
   **A:** It maintains a table of private-to-public IP mappings for return traffic.  

4. **Q:** Explain how return traffic is routed back to private subnet instances.  
   **A:** NAT Gateway receives return traffic on its public IP and forwards it to the private instance based on its mapping table.  

5. **Q:** Compare scalability differences between NAT Gateway and NAT Instance.  
   **A:** NAT Gateway is fully managed and auto-scalable; NAT Instance has limited throughput and requires manual scaling.

---

