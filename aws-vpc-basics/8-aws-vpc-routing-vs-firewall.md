# 🌐 AWS VPC: Routing vs Firewall (Security Groups & NACLs)

## 📖 Overview
Before diving into **VPC Firewalls** (Security Groups & NACLs), it’s essential to clearly understand the difference between **Routing** and **Firewalling** in AWS networking.

---

## 🔑 Key Concepts

### 1️⃣ Routing
- Determines **how packets travel** from source to destination.  
- Analogy: **Roads** inside a country connecting cities and towns.  
- AWS Example: Route tables in a VPC.

### 2️⃣ Firewall
- Controls **whether packets are allowed** to reach the destination once they arrive.  
- Analogy: **Checkposts or tollbooths** that validate your identity before entry.  
- AWS Example: Security Groups and Network ACLs.

---

## 🏙️ Analogy for Better Understanding

- **Country → VPC** (your private network space).  
- **Cities → Subnets** inside the VPC.  
- **Buildings → EC2 Instances or other resources**.  
- **Roads → Routers / Route tables** (decide the path of traffic).  
- **Tollbooths → Network ACLs** (subnet-level protection).  
- **Security guards at buildings → Security Groups** (instance-level protection).  
- **Car → IP Packet** (data traveling in the network).  
- **Visa/Identity Check → Permissions & Rules** that decide if access is allowed.

##Diagram [car](https://github.com/amilasenakumara/aws-vpc-networking/blob/563b1aff203cacbf791417440942bda3a304b809/images/vpc-routing-firewall-analogy.png) vs [IP packet](https://github.com/amilasenakumara/aws-vpc-networking/blob/563b1aff203cacbf791417440942bda3a304b809/images/vpc-routing-firewall.png)

---

## 🛡️ Layered Security in AWS Networking
- **Internet Gateway (IGW):** Entry point for external traffic into a VPC.  
- **Network ACLs (NACLs):** Subnet-level firewall, controls inbound/outbound rules.  
- **Security Groups (SGs):** Instance-level firewall, protects individual EC2 instances.  
- **Routers & Routes:** Only decide the **path**, not permissions.  

---

## ✅ Point-wise Summary
1. **Routing = Path** (how packets travel).  
2. **Firewall = Access control** (allow/deny at destination).  
3. **VPC = Country**, **Subnets = Cities**, **Instances = Buildings**.  
4. **Routers = Roads**, **Packets = Cars**.  
5. **NACLs = Tollbooths (subnet-level check)**.  
6. **Security Groups = Security guards (instance-level check)**.  
7. **Internet Gateway = Entry point into VPC**.  
8. Both **NACL + Security Group** checks must pass before traffic reaches the instance.  
9. Firewalls provide **layered defense** inside AWS.  
10. Routing does not block traffic; **firewalls enforce rules**.

---

## 📘 Exam Questions & Answers

### 📝 Basic Exam Q&A
1. **Q:** What is the role of a route table in AWS VPC?  
   **A:** It defines the path that network traffic takes from a subnet to other subnets or the internet.

2. **Q:** What is the difference between Security Groups and NACLs?  
   **A:** SGs work at the **instance level**, while NACLs work at the **subnet level**.

3. **Q:** Does a route table allow or deny traffic?  
   **A:** No, it only defines the **path**. Allow/deny is handled by firewalls (SGs/NACLs).

4. **Q:** Can an EC2 instance exist without a security group?  
   **A:** No, every instance must be associated with at least one security group.

5. **Q:** What is the function of an Internet Gateway (IGW)?  
   **A:** It allows communication between instances in a VPC and the internet.

---

### 💼 Basic Interview Q&A
1. **Q:** Explain routing vs firewall in simple terms.  
   **A:** Routing decides **where traffic goes**, firewall decides **whether it is allowed**.

2. **Q:** Which is stateful: Security Groups or NACLs?  
   **A:** Security Groups are **stateful**, NACLs are **stateless**.

3. **Q:** Give an analogy for Security Groups.  
   **A:** Like a **security guard** at a building checking IDs before entry.

4. **Q:** Can a packet reach an EC2 instance if the route exists but SG denies it?  
   **A:** No, the firewall (SG) will block it.

5. **Q:** Why do we need both SGs and NACLs?  
   **A:** To provide **multiple layers of defense**.

---

## 🎯 Advanced Exam Questions & Answers

1. **Q:** How does AWS handle packet return traffic for Security Groups?  
   **A:** Security Groups are stateful, so return traffic is automatically allowed.

2. **Q:** Can you associate multiple NACLs with a subnet?  
   **A:** No, only one NACL per subnet, but it can have multiple rules.

3. **Q:** What happens if there is no route to the Internet Gateway in a public subnet?  
   **A:** Instances won’t have internet access even if SG/NACL rules allow it.

4. **Q:** Which takes precedence: Security Group or NACL?  
   **A:** Both must allow traffic; if either denies, the packet is dropped.

5. **Q:** Can you block specific IPs using Security Groups?  
   **A:** No, SGs only allow rules. To block, you must use NACLs.

---

## 🧠 Advanced Interview Questions & Answers

1. **Q:** Explain the difference between stateless and stateful firewalls in AWS.  
   **A:** SGs are **stateful** (automatically allow return traffic), NACLs are **stateless** (explicit rules needed both ways).

2. **Q:** How do SGs and NACLs complement each other?  
   **A:** SGs secure at the **instance level**, NACLs at the **subnet level**, together forming **defense in depth**.

3. **Q:** Can a security group span across multiple subnets in a VPC?  
   **A:** Yes, SGs can be attached to instances in different subnets within the same VPC.

4. **Q:** In a VPC with both private and public subnets, where would you typically attach an Internet Gateway?  
   **A:** To the VPC, then add a route from the **public subnet** to the IGW.

5. **Q:** How would you design a layered firewall system in AWS?  
   **A:** Use NACLs for coarse subnet-level filtering + SGs for fine-grained instance-level control.

---


