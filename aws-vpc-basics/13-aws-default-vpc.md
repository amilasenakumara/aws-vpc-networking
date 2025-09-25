# 🏗️ Default VPC in AWS

## 📌 Introduction
- Every AWS **region** automatically comes with a **Default VPC**.  
- Purpose: **Ease of use** → You can quickly launch an EC2 instance without setting up networking from scratch.  
- **Default VPC behaves like any normal VPC** (no special functionality, just pre-configured).  

---

## 🔎 What is Default VPC?
- Created by **AWS by default in every region**.  
- Allows launching resources **immediately** without creating VPC, subnets, or routing manually.  
- Useful for **new AWS users** and quick testing.  
- CIDR block of Default VPC: **172.31.0.0/16** (RFC 1918 private IP range).  

---

## 🏗️ Default VPC Structure
1. **VPC** → Automatically created in each region.  
2. **Subnets** → One subnet per **Availability Zone (AZ)**.  
   - CIDR prefix: **/20**.  
   - Each subnet has **4096 IP addresses** (2^(32-20)).  
   - Larger than typical /24 subnets (256 IPs).  
3. **Internet Gateway (IGW)** → Attached to Default VPC for internet connectivity.  
4. **Route Table**  
   - **Main Route Table** with:
     - Local route.  
     - Route to Internet Gateway.  
   - Makes all default subnets **public subnets**.  

---

## 🌍 Example: Mumbai Region
- Mumbai region has **3 AZs** → hence, AWS creates **3 subnets**.  
- All subnets are **public** (since route table points to IGW).  
- Default VPC has CIDR: **172.31.0.0/16**.  
- Subnets each have **/20** range.  

---

## ⚙️ Behavior
- When you launch an **EC2 instance** in a new AWS account:  
  - By default, it goes into **Default VPC**.  
  - Instance gets a **Public IP**.  
  - It sits in a **public subnet**.  
  - Accessible over **SSH/Internet**.  

---

## 🗑️ Deleting and Recreating Default VPC
- You **can delete** the Default VPC.  
- You can **recreate it** from AWS Console (AWS provides option).  
- Useful for labs where you want to practice building VPCs manually.  

---

## ✅ Key Takeaways
- Default VPC = **ready-to-use network environment**.  
- Every region has **1 Default VPC**.  
- Subnets = **1 per AZ**, all **public**.  
- Route Table = **pre-configured with IGW**.  
- CIDR = **172.31.0.0/16**.  
- You can **delete & recreate** anytime.  
- Great for **beginners and quick testing** but not for complex production setups.  

---

## 📚 Exam Questions (Basic)

**Q1.** What is the CIDR range of the default VPC?  
**A1.** `172.31.0.0/16`.  

**Q2.** How many subnets are created by default in a region with 3 AZs?  
**A2.** 3 subnets (one per AZ).  

**Q3.** What makes Default VPC subnets public?  
**A3.** A route to the Internet Gateway in the main route table.  

**Q4.** Can you delete the default VPC?  
**A4.** Yes, and it can be recreated from AWS Console.  

**Q5.** Why does AWS create a default VPC in every region?  
**A5.** To simplify launching resources quickly without setting up networking manually.  

---

## 💼 Interview Questions (Basic)

**Q1.** Is there any difference in functionality between Default VPC and a custom VPC?  
**A1.** No, Default VPC is the same as a custom VPC; it’s just pre-configured.  

**Q2.** Why are Default VPC subnets always public?  
**A2.** Because their route table includes a route to the Internet Gateway.  

**Q3.** What CIDR prefix is used for Default VPC subnets?  
**A3.** `/20`.  

**Q4.** What happens if you delete the Default VPC?  
**A4.** You can recreate it via the AWS Console.  

**Q5.** What is the benefit of Default VPC for beginners?  
**A5.** Allows launching EC2 instances immediately with internet access.  

---

## 📚 Advanced Exam Questions

**Q1.** How many IPs are available in a /20 subnet?  
**A1.** 4096 IPs (2^(32-20)).  

**Q2.** Why does AWS choose /20 for Default VPC subnets instead of /24?  
**A2.** To allow launching a larger number of EC2 instances per subnet.  

**Q3.** Which AWS component ensures that Default VPC subnets are public?  
**A3.** The **Internet Gateway** and its route in the **Main Route Table**.  

**Q4.** If a region has 4 AZs, how many Default VPC subnets will it have?  
**A4.** 4 subnets (one per AZ).  

**Q5.** Can Default VPC support IPv6 by default?  
**A5.** No, by default it is only IPv4 unless IPv6 is explicitly added.  

---

## 💼 Advanced Interview Questions

**Q1.** How does the Default VPC simplify EC2 launches for new AWS users?  
**A1.** By providing a pre-built VPC, subnets, IGW, and route tables so users can launch EC2 without manual networking setup.  

**Q2.** What would happen if the Default VPC did not exist in a new AWS account?  
**A2.** Users would have to manually create VPC, subnets, IGW, and routing before launching EC2.  

**Q3.** How does AWS ensure high availability in Default VPC subnets?  
**A3.** By creating **one subnet per AZ**, allowing instances to be distributed across AZs.  

**Q4.** Is there any difference in network performance between Default VPC and Custom VPC?  
**A4.** No, performance is identical; only setup differs.  

**Q5.** How do you check which VPC is the Default VPC in AWS Console?  
**A5.** Look for the **“Default VPC = Yes” flag** in the VPC console.  

---

