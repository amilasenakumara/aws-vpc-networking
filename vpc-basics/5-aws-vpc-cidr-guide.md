# 🖥️ AWS VPC & CIDR Learning Notes

## Table of Contents
- [Introduction](#introduction)
- [IP Addresses](#ip-addresses)
- [CIDR (Classless Inter-Domain Routing)](#cidr-classless-inter-domain-routing)
- [VPC & Subnet Examples](#vpc--subnet-examples)
- [Private IP Address Ranges](#private-ip-address-ranges)
- [Exam Questions & Answers](#exam-questions--answers)
- [Interview Questions & Answers](#interview-questions--answers)
- [Advanced Exam Questions & Answers](#advanced-exam-questions--answers)
- [Advanced Interview Questions & Answers](#advanced-interview-questions--answers)

---

## Introduction
- VPCs (Virtual Private Clouds) are private networks inside AWS.
- Understanding **CIDR** is crucial for designing VPCs and subnets.
- Lecture focuses on **IPv4 CIDR ranges** for simplicity.
- IPv6 basics are also touched upon.

---

## IP Addresses
- Two types of IP addresses:
  - **IPv4**: 32-bit addresses (sufficient for most workloads)
  - **IPv6**: 128-bit addresses (for large-scale IoT and modern applications)
- Every machine on the network must have a unique IP address.
- IPv4 representation:
  - Four octets separated by dots, e.g., `192.168.56.212`
  - Each octet is 8 bits (binary representation used for calculation)
- IPv6 is 128-bit long, represented in hexadecimal.

---

## CIDR (Classless Inter-Domain Routing)
- Replaces old class-based system (Class A/B/C)
- CIDR notation: `<base_address>/<prefix>`, e.g., `192.168.0.0/16`
  - **Prefix** determines the number of network bits.
  - Remaining bits are for hosts in that network.
- **Example:**
  - `192.168.22.0/24`
    - 24 bits for network → first three octets fixed
    - 8 bits for host → 256 possible hosts
- Total hosts in a network can be calculated as:  
  `2^(32 - prefix)`  
  - `/16` → 2^16 = 65,536 hosts
  - `/17` → 2^15 = 32,768 hosts
  - `/28` → 2^4 = 16 hosts
- **Subnetting** allows dividing VPCs into smaller networks:
  - Example: `/24` subnet inside `/16` VPC → 256 IPs per subnet
  - Small subnets (`/28`) are useful for resources like NAT Gateways or Load Balancers.

---

## VPC & Subnet Examples
- **VPC Creation:**
  - Choose AWS region → create VPC → assign CIDR range (e.g., `10.10.0.0/16`)
  - Scope: Regional
- **Subnets:**
  - Subnets are mapped to availability zones (AZs)
  - Example:
    - Subnet A → AZ1 → `10.10.0.0/24` (256 IPs)
    - Subnet B → AZ2 → `10.10.1.0/24` (256 IPs)
- Smaller subnets can be created for specific workloads:
  - Example: `/28` subnet → 16 IPs
- **IPv6 in VPC:**
  - AWS assigns `/56` to VPC and `/64` for subnets.
  - IPv6 addresses are globally unique and public by default.

---

## Private IP Address Ranges

| RFC 1918 Range | Prefix | Notes |
|----------------|--------|-------|
| `10.0.0.0 – 10.255.255.255` | /8 | Large-scale private networks |
| `172.16.0.0 – 172.31.255.255` | /12 | Medium-scale private networks |
| `192.168.0.0 – 192.168.255.255` | /16 | Small-scale private networks |

> ✅ Subnet CIDR ranges must be smaller or equal to the VPC CIDR.

---

## Exam Questions & Answers

1. **Q:** What is CIDR in networking?  
   **A:** CIDR (Classless Inter-Domain Routing) allows flexible allocation of IP addresses by specifying network prefix and host bits.

2. **Q:** How many hosts can a `/24` subnet support?  
   **A:** 2^(32-24) = 256 hosts

3. **Q:** What are the private IPv4 ranges as per RFC 1918?  
   **A:** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`

4. **Q:** What is the main difference between IPv4 and IPv6 addresses?  
   **A:** IPv4 is 32-bit (limited), IPv6 is 128-bit (vast, globally unique).

5. **Q:** What is the smallest CIDR you can assign to a subnet in AWS?  
   **A:** `/28` (16 IP addresses)

---

## Interview Questions & Answers

1. **Q:** Why is CIDR preferred over class-based IP addressing?  
   **A:** CIDR allows flexible allocation of IPs without wasting addresses.

2. **Q:** Can subnets in AWS be larger than the VPC?  
   **A:** No, subnets must be equal to or smaller than the VPC CIDR.

3. **Q:** What prefix would you use for a subnet requiring 500 hosts?  
   **A:** `/23` (2^(32-23) = 512 hosts)

4. **Q:** How does IPv6 differ in privacy from IPv4 inside a VPC?  
   **A:** IPv4 addresses are private; IPv6 addresses are globally unique and public.

5. **Q:** Can AWS automatically allocate IPv6 CIDR?  
   **A:** Yes, AWS assigns IPv6 CIDR ranges; you cannot choose them manually.

---

## Advanced Exam Questions & Answers

1. **Q:** Calculate hosts in a VPC with CIDR `10.0.0.0/18`.  
   **A:** 2^(32-18) = 16,384 hosts

2. **Q:** What subnet mask corresponds to `/26`?  
   **A:** 255.255.255.192

3. **Q:** How many `/24` subnets can fit into a `/16` VPC?  
   **A:** 2^(24-16) = 256 subnets

4. **Q:** Explain the impact of prefix length on host availability.  
   **A:** Larger prefix → fewer host addresses; smaller prefix → more host addresses.

5. **Q:** How do you allocate subnets across multiple AZs for redundancy?  
   **A:** Divide VPC CIDR into equal subnets and assign one subnet per AZ.

---

## Advanced Interview Questions & Answers

1. **Q:** Why would you use a `/28` subnet for AWS resources?  
   **A:** To efficiently use IPs for small workloads like NAT Gateways or Load Balancers.

2. **Q:** Can you create overlapping CIDR ranges for VPCs?  
   **A:** No, overlapping CIDRs cause routing conflicts.

3. **Q:** What’s the maximum number of hosts in a `/16` VPC?  
   **A:** 2^(32-16) = 65,536 hosts

4. **Q:** How does IPv6 affect AWS routing compared to IPv4?  
   **A:** IPv6 addresses are public by default; routing must consider global reachability.

5. **Q:** How do you determine the starting IP of the N-th subnet?  
   **A:** Increment the last host bits of previous subnet by the subnet size (2^(32-subnet_prefix)).

---

## 🔗 References
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [RFC 1918 - Private IP Addresses](https://tools.ietf.org/html/rfc1918)
- [IPv6 Addressing](https://www.ietf.org/rfc/rfc4291.txt)
