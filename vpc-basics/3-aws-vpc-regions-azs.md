🖼️Regions and AZs 🖼️

[AWS services scope with respect to VPC](https://github.com/amilasenakumara/aws-vpc-networking/blob/82ffd6c730f872ca69f674d9984a7cfa86c4712f/images/regoins-az.png)

# AWS VPC, Regions, and Availability Zones – Learning Notes 📚

This repository contains my **AWS learning notes** on VPCs, Regions, Availability Zones, and service scopes.  
Purpose: Understand **VPC scope** and its relationship with AWS accounts, regions, and AZs.

---

## 1️⃣ Purpose of the Lecture
- Clarifies the scope of VPCs and their relation to AWS account, regions, and AZs.  
- Answers questions like: *Can a VPC host resources across AWS regions or accounts?*  

---

## 2️⃣ AWS Regions & Availability Zones (AZs)
- AWS currently has **32 regions** and **102 AZs** (numbers growing over time).  
- Regions are chosen based on:
  - User base location 🌍  
  - Data residency requirements 🗄️  
- Each region has multiple AZs for **high availability**:
  - Physically separated with redundant power ⚡  
  - Designed to survive natural disasters 🌪️  

---

## 3️⃣ VPC Scope
- **VPCs are regional resources** — created in a specific AWS region.  
- To use multiple regions, **create separate VPCs** in each region.  
- Communication between VPCs:
  - VPC Peering  
  - Transit Gateway  

---

## 4️⃣ AWS Account and Region Relationship
- AWS account = **top-level entity** 🏢  
- Within an account, you can deploy workloads in any region.  
- Examples:
  - Mumbai → 3 AZs  
  - North Virginia → 6 AZs  

---

## 5️⃣ Multiple VPCs in a Region
- Default limit: 5 VPCs per region (can be increased).  
- Reasons to create multiple VPCs:
  - Isolate applications logically  
  - Example: Finance app in one VPC, AI app in another  

---

## 6️⃣ Subnets and AZ Mapping
- **EC2 instances** are deployed in a specific AZ.  
- **Subnets** define which AZ resources reside in.  
- To deploy across multiple AZs → create multiple subnets in the VPC.  

---

## 7️⃣ AWS Service Scopes
| Service Type | Examples | Scope |
|-------------|---------|-------|
| AZ-level | EC2, RDS | Resources deployed in a specific AZ |
| VPC-level | ELB | Distributes traffic across AZs in a VPC |
| Regional-level | S3 | Exists in a region, outside VPC, accessible via internet |
| Global-level | Route 53, IAM, Billing | Operate across regions globally |

---

## 8️⃣ Summary Hierarchy
1. **AWS Account** → Top-level entity  
2. **Regions** → Multiple per account  
3. **VPCs** → One or more per region  
4. **Subnets** → Mapped to AZs within a VPC  
5. **Resources** → Deployed in subnets (EC2, RDS), accessed depending on service scope  

---

*Created for personal learning purposes from Udemy AWS course.*


