# AWS VPC Basics – Learning Notes ☁️

This repository contains my **AWS learning notes** on Virtual Private Cloud (VPC) and network mapping.  
Purpose: Understand how on-premises networks translate to AWS VPC networks.

---

## 1️⃣ Introduction to VPC
- **VPC (Virtual Private Cloud)** = Private networking space in AWS ☁️  
- Launch **EC2 instances, databases**, and other resources inside a private network.

---

## 2️⃣ On-Premises Networking Recap
- **LAN (Local Area Network)** connects multiple hosts over physical links 🖥️  
- **Switch**: Operates at Layer 2, forwards packets using MAC addresses 🔀  
- **Router**: Connects multiple LANs and routes packets between networks 🌐  
- Internet access is via **top-level router** 🚪  

---

## 3️⃣ Virtual Networking for Global Offices
- Teams across offices/countries can share the same network 🌏  
- Virtual LAN (VLAN) allows hosts in different physical LANs to appear on the **same network** 💻  

---

## 4️⃣ Mapping Physical Network to AWS VPC
- **Physical LAN → AWS Subnet**  
- **Router → Local VPC Router**  
- **Top-level Internet Router → Internet Gateway**  
- Deploy resources (EC2, RDS) inside subnets  

---

## 5️⃣ VPC Connectivity
- **Internal VPC communication**: Local routing 🛤️  
- **External (Internet) communication**: Requires **Internet Gateway** 🌐  
- VPC without Internet Gateway = private network 🔒  

---

## 6️⃣ Key Points
- AWS VPC allows you to **replicate physical network topology** in the cloud  
- Subnets represent individual LANs  
- EC2 instances deployed inside subnets  
- Internet Gateway enables external access  

---

## 7️⃣ Next Steps
- Deeper dive into **VPC components** like subnets, internet gateways, and routing tables in the following lectures 🔎  
- Focus will remain on **AWS networking** and internal VPC design.

---

*Created for personal learning purposes from Udemy AWS course.*
