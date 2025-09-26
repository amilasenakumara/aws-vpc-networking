# 🚀 AWS VPC Exercise: Private Subnet with Bastion (Jump Host) Access  

This exercise builds upon the previous lab where we created a **public subnet** with one EC2 instance (`EC2-A`).  
Now, we extend it by creating a **private subnet** with an EC2 instance (`EC2-B`) that is only reachable through the public EC2.  

---

## 📝 Point-wise Summary  

### 🔹 1. Create Private Subnet  
- CIDR Block: `10.100.11.0/24`  
- Belongs to same VPC (`VPC-A`)  
- Located in the same Availability Zone  

### 🔹 2. Create Private Route Table  
- Create new route table → Associate it with the private subnet  
- Only **local VPC route** (no internet route)  
- Ensures **no direct internet access** for private subnet  

### 🔹 3. Launch Private EC2 Instance (EC2-B)  
- AMI: Default Amazon Linux  
- Subnet: **Private subnet**  
- Public IP: **Disabled**  
- Key Pair: Same as earlier (`.pem` file)  

### 🔹 4. Configure Security Group for EC2-B  
- Allow SSH (`port 22`) only from **VPC CIDR (10.100.0.0/16)**  
- Add **ICMP (ping)** rule from VPC CIDR (optional)  

### 🔹 5. Accessing EC2-B  
- Cannot connect directly from local workstation (no public IP)  
- Workflow:  
  1. Connect to **EC2-A** (public instance) via SSH  
  2. Copy SSH key (`.pem`) from local workstation → EC2-A  
  3. From EC2-A, SSH into EC2-B using private IP  

### 🔹 6. Internet Access Test  
- `EC2-A`: Can access internet (public subnet + IGW route)  
- `EC2-B`: **Cannot access internet** (private subnet, no IGW route)  

---

## 📚 Exam Questions  

1. **What is a private subnet in AWS?**  
   - A subnet without a direct route to the internet gateway.  

2. **Why can’t you SSH directly into an EC2 instance in a private subnet?**  
   - Because it has no public IP or internet access.  

3. **How can you access an EC2 instance in a private subnet?**  
   - Via a bastion/jump host in a public subnet (e.g., EC2-A).  

4. **What’s the difference between public and private route tables?**  
   - Public has a route to IGW; private only has local VPC routes.  

5. **What happens if you ping google.com from EC2-B in this setup?**  
   - It will fail (no internet access).  

---

## 💼 Interview Questions  

1. **Explain how to connect to a private EC2 instance.**  
   - Use a bastion host (public EC2), SSH key forwarding, or SSM Session Manager.  

2. **What role do security groups play here?**  
   - They restrict access so only VPC CIDR or specific instances can connect.  

3. **Why would you design workloads in private subnets?**  
   - Security best practice: keep sensitive workloads away from direct internet access.  

4. **Can you reuse the same key pair for multiple EC2 instances?**  
   - Yes, the same key pair can be used across instances in the same region.  

5. **What is the main difference between EC2-A and EC2-B in this setup?**  
   - EC2-A has a public IP (internet-accessible), EC2-B has only a private IP.  

---

## 🎯 Advanced Exam Questions  

1. **What is a bastion host?**  
   - A hardened public EC2 instance used to securely access private instances.  

2. **Why is it important to limit SSH access in private subnet security groups?**  
   - To reduce attack surface and enforce least privilege.  

3. **Can NAT Gateway provide internet access to private subnets?**  
   - Yes, by routing outbound traffic through NAT Gateway.  

4. **What would happen if you added a route to IGW in private subnet’s route table?**  
   - Instances would still not reach the internet (no public IPs).  

5. **How can you connect to private EC2 without bastion host?**  
   - Using AWS Systems Manager Session Manager.  

---

## 🧠 Advanced Interview Questions  

1. **Compare Bastion Host vs AWS Session Manager for private EC2 access.**  
   - Bastion requires public EC2 + SSH; Session Manager allows direct browser/CLI access without SSH keys.  

2. **What are the risks of copying SSH private keys into EC2 instances?**  
   - Keys may be compromised; best practice is to use Session Manager or temporary certificates.  

3. **How would you enable secure outbound internet access for EC2-B?**  
   - By using a NAT Gateway/Instance in the public subnet.  

4. **What is the difference between Security Groups and NACLs for private subnet access?**  
   - SG = stateful (track connections), NACL = stateless (must allow inbound + outbound separately).  

5. **How does this architecture support layered security in AWS?**  
   - Public subnet as controlled entry point, private subnet for protected workloads, route separation.  

---
