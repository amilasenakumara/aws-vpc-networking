# 🚀 AWS VPC Basics – Exercise 1  

This exercise focuses on **creating a VPC with a single public subnet**, launching an **EC2 instance**, and accessing it from your workstation via **SSH**.  

---

## 📌 Key Concepts Covered
- ✅ Create a **VPC** with a CIDR range (private IP range).  
- ✅ Attach an **Internet Gateway (IGW)** to the VPC for external communication.  
- ✅ Create a **Public Subnet** within the VPC.  
- ✅ Configure a **Route Table** to route internet traffic via the IGW.  
- ✅ Associate the route table with the subnet to make it public.  
- ✅ Enable **Auto-assign Public IPs** for instances launched in the subnet.  
- ✅ Launch an **EC2 instance** inside the subnet.  
- ✅ Use **SSH key pairs** to connect securely from your workstation.  

---

## 🛠️ Step-by-Step Workflow | 

![High level steps](https://github.com/amilasenakumara/aws-vpc-networking/blob/ad5b08fdfd955b07f46973ab85549738ceffc877/images/exercise-public-subnet.png?raw=true)

1. **Prerequisites**
   - AWS account activated.
   - SSH key pair created (`.pem` or `.ppk` depending on OS).

2. **VPC Setup**
   - Create a VPC (e.g., CIDR: `10.100.0.0/16`).
   - Default VPC exists but create a new one for learning.

3. **Internet Gateway**
   - Create an IGW.
   - Attach it to the VPC.

4. **Public Subnet**
   - Create subnet (e.g., `10.100.0.0/24`).
   - Place it in an Availability Zone (AZ).

5. **Route Table**
   - Create a route table.
   - Add route: `0.0.0.0/0 → Internet Gateway`.
   - Associate the route table with the public subnet.

6. **Enable Auto-Assign Public IP**
   - In subnet settings, enable auto-assign public IPv4.

7. **Launch EC2 Instance**
   - Instance type: `t2.micro` (free tier).
   - Network: Select created VPC + Public Subnet.
   - Security Group: Allow **SSH (port 22)** from `0.0.0.0/0`.
   - Key Pair: Use existing SSH key.

8. **Access EC2 Instance**
   - Copy Public IP.
   - Connect using:
     ```bash
     ssh -i "your-key.pem" ec2-user@<PUBLIC-IP>
     ```
   - For Windows, use PuTTY with `.ppk`.

9. **Validation**
   - Successful login into EC2.
   - Test outbound connectivity:
     ```bash
     ping google.com
     ```

10. **Cleanup**
    - Terminate EC2 when not needed to avoid costs.

---

## 📝 Diagram (Conceptual)
- **Region** → Availability Zone → **VPC** → **Subnet** → **EC2 Instance**  
- Internet Gateway provides connectivity between **Public Subnet** and the Internet.  

---

## 🎯 Exam Questions (with Answers)

### 1. What is the role of an Internet Gateway (IGW)?
- **Answer:** IGW enables communication between instances in a VPC and the Internet.

### 2. What CIDR range was used in the exercise VPC?
- **Answer:** `10.100.0.0/16`.

### 3. How do you make a subnet public?
- **Answer:** Associate a route table that routes `0.0.0.0/0` traffic to the Internet Gateway.

### 4. What key is required to connect to EC2 using SSH?
- **Answer:** An SSH private key (`.pem` on Linux/Mac, `.ppk` on Windows with PuTTY).

### 5. What instance type is typically free in AWS Free Tier?
- **Answer:** `t2.micro`.

---

## 💼 Interview Questions (with Answers)

### 1. What is the difference between a Public and Private Subnet?
- **Answer:** Public subnets have routes to the Internet via an IGW; private subnets don’t.

### 2. Why can’t you connect to an EC2 instance in a private subnet from the Internet?
- **Answer:** Because it has no direct route via the Internet Gateway and no public IP.

### 3. What happens if you launch an EC2 without enabling Auto-Assign Public IP?
- **Answer:** The instance won’t have a public IP, requiring Elastic IP or NAT for connectivity.

### 4. What is the default VPC in AWS?
- **Answer:** A pre-created VPC in each region for launching instances without custom networking.

### 5. How does Security Group differ from a Network ACL?
- **Answer:** Security Groups are **stateful**, while Network ACLs are **stateless**.

---

## 🧠 Advanced Exam Questions (with Answers)

### 1. What is the maximum number of VPCs per region (by default)?
- **Answer:** 5 (can be increased by request).

### 2. What is the difference between IGW and NAT Gateway?
- **Answer:** IGW allows **public Internet access**; NAT Gateway allows **private instances outbound Internet access**.

### 3. Can you have overlapping CIDR ranges across VPCs?
- **Answer:** No, overlapping ranges can cause routing conflicts.

### 4. What’s the default route inside a new route table?
- **Answer:** A local route (`VPC CIDR`) allowing communication within the VPC only.

### 5. Why is SSH restricted to port 22?
- **Answer:** Port 22 is the default for secure shell communication.

---

## 🧑‍💻 Advanced Interview Questions (with Answers)

### 1. Why is VPC Peering non-transitive?
- **Answer:** Because VPC Peering is point-to-point and does not allow traffic to automatically pass through one VPC to another.

### 2. In which scenario would you prefer Elastic IP over Auto-Assigned Public IP?
- **Answer:** When you need a **static public IP** that persists even after instance restart.

### 3. Why should you avoid `0.0.0.0/0` in Security Groups?
- **Answer:** It opens access to everyone, creating security risks.

### 4. What is the difference between a VPC Endpoint and an IGW?
- **Answer:** VPC Endpoints provide **private connectivity to AWS services**, while IGW connects to the Internet.

### 5. How would you design a highly available VPC architecture?
- **Answer:** Use multiple subnets across different Availability Zones, NAT Gateways for private subnets, and Load Balancers for redundancy.

---

# 📝 STEPS – Create VPC with Public Subnet & Launch EC2

This guide walks you through the process of creating a **VPC with a public subnet**, attaching an **Internet Gateway**, setting up **routes**, and finally launching and connecting to an **EC2 instance**.

---

## 1️⃣ Create VPC
- **AWS Console** → Go to **VPC service** → **Your VPCs** → **Create VPC**  
- Resources to create: `VPC Only`  
- **Name tag:** `VPC-A`  
- **IPv4 CIDR block:** Manual input → `10.100.0.0/16`  
- **Tenancy:** Default  
- Click **Create VPC** ✅  

---

## 2️⃣ Create Internet Gateway
- Go to **Internet Gateways** → **Create Internet Gateway**  
  - **Name tag:** `VPC-A-IGW`  
  - Click **Create Internet Gateway**  
- Select the Internet Gateway → **Actions** → **Attach to VPC**  
  - Select your VPC (`VPC-A`)  
  - Click **Attach Internet Gateway** ✅  

---

## 3️⃣ Create Subnet
- Go to **Subnets** → **Create Subnet**  
  - **VPC ID:** `VPC-A`  
  - **Subnet Name:** `VPC-A-Public`  
  - **AZ:** Select `AZ-1`  
  - **IPv4 CIDR block:** `10.100.0.0/24`  
  - Click **Create Subnet**  

- Enable auto-assign public IP:  
  - Select Subnet → **Actions** → **Edit Subnet Settings**  
  - Modify Auto-Assign IP Settings → **Enable** → **Save** ✅  

---

## 4️⃣ Create Route Table
- Go to **Route Tables** → **Create Route Table**  
  - **Name:** `VPC-A-Public-RT`  
  - **VPC:** `VPC-A`  
  - Click **Create Route Table**  

- Edit Routes:  
  - Select Route Table → **Routes** → **Edit routes**  
  - Add another route:  
    - **Destination:** `0.0.0.0/0`  
    - **Target:** Internet Gateway (`igw-xxxxx`)  
  - Save changes ✅  

---

## 5️⃣ Associate Route Table with Subnet
- Select Route Table → **Subnet Associations** → **Edit subnet associations**  
- Check the **VPC-A-Public** subnet  
- Save associations ✅  

---

## 6️⃣ Launch EC2 Instance in Public Subnet
- Go to **EC2 Service** → **EC2 Dashboard** → **Launch Instances**  
- **Name:** `EC2-A`  
- **Application and OS Image (AMI):** Amazon Linux (default)  
- **Instance type:** `t2.micro` (default, Free Tier)  
- **Key pair:** Select the key pair you created earlier  

### 🔧 Network Settings
- Select **VPC:** `VPC-A`  
- Select **Subnet:** `VPC-A-Public`  
- Ensure **Auto-Assign Public IP** is **enabled**  

### 🔒 Firewall (Security Group)
- Create a new Security Group:  
  - **Name:** `EC2-A-SG`  
  - **Inbound Rule:**  
    - Type → `SSH`  
    - Port Range → `22`  
    - Source Type → `My IP`  

### 💾 Storage
- Configure storage: `8 GiB`, `gp3` (default)  

- ✅ Click **Launch Instance**  

---

## 7️⃣ Connect to EC2 Instance
- Copy the **Public IP** of your instance  
- Connect from your workstation:  

### 🔹 Linux / Mac Terminal
```bash
ssh -i "your-key.pem" ec2-user@<PUBLIC-IP>


# 📝 STEPS – Create VPC with Public Subnet & Launch EC2

This guide walks you through the process of creating a **VPC with a public subnet**, attaching an **Internet Gateway**, setting up **routes**, and finally launching and connecting to an **EC2 instance**.

---

## 🏗 Architecture Diagram

```mermaid
graph TD
    Laptop[Your Workstation]
    EC2[EC2 Instance]
    Subnet[Public Subnet (VPC-A-Public)]
    VPC[VPC (VPC-A)]
    IGW[Internet Gateway (VPC-A-IGW)]

    Laptop -->|SSH| EC2
    EC2 --> Subnet
    Subnet --> VPC
    VPC --> IGW

