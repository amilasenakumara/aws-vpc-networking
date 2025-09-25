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
---

# 🌐 Proxy vs VPN in AWS (This is extra note)

## 🔹 Proxy in AWS (Example: NAT Gateway / Proxy Server)
- **NAT Gateway** acts like a **proxy** for instances in a **private subnet**.  
- Private EC2 instances **cannot directly access the internet**.  
- Instead, they **send traffic to the NAT Gateway**, which forwards the request to the internet.  
- The response comes back through NAT → to the EC2 instance.  
- ✅ NAT Gateway hides the **private instance’s IP address** and uses its own **public IP** → just like a proxy.  

### 📌 Example use case:
Your app servers in a private subnet need to **download OS updates** or fetch data from the internet.  
They use the **NAT Gateway** as a proxy to go out, but outsiders **cannot directly connect back** to them.

---

## 🔹 VPN in AWS (Example: AWS Site-to-Site VPN / Client VPN)
- **VPN** creates a **secure, encrypted tunnel** between **your on-premises network** and **your VPC**.  
- Unlike a proxy, VPN covers **all traffic** between the two networks.  
- Every packet is encrypted → ensuring **confidentiality and integrity**.  

### 📌 Example use case:
Your office network (on-premises) needs to connect securely to AWS.  
You set up a **Site-to-Site VPN** between your office router and AWS Virtual Private Gateway.  
Now, all your office computers can securely access AWS resources **as if they were on the same local network**.

---

## ✅ Key Comparison in AWS

| Feature  | Proxy (NAT Gateway / Proxy Server) | VPN (AWS VPN) |
|----------|------------------------------------|---------------|
| **Scope** | Works for specific traffic (outbound internet) | Encrypts & tunnels all traffic between networks |
| **Security** | ❌ No encryption (just IP masking) | ✅ Encrypted (secure tunnel) |
| **Use Case** | Private subnet EC2 instances accessing internet | On-premises network securely connecting to AWS VPC |
| **Example** | NAT Gateway, Squid Proxy | Site-to-Site VPN, Client VPN |

---

## 🎯 Final Takeaway
- **Proxy = NAT Gateway (outbound only, no encryption)**  
- **VPN = Secure tunnel (two-way, full encryption)**  


---
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

# AWS NAT Gateway Lecture Notes - Complete Lesson

So now in this lecture let's understand another important component of the VPC that is **Nat gateway**.

Okay.

Now if you go back to our previous lectures around the **public subnets** and the **private subnets**, we understood

that the **instances in public subnet can communicate with the internet directly**.

And the **route table of the public subnet has route to the internet via the internet gateway**.

And **this traffic is allowed now**.

With respect to the **private subnet**, we know that **private subnet route table doesn't have route to the

internet gateway**.

And there is only a **local route** which allows the communication within the VPC, which means this instance

in the private subnet **cannot directly communicate with the internet**.

So **this traffic is not allowed**.

Now I'm just trying to represent the same network architecture in a different way.

So again there is a **VPC public subnet**.

And there is an **instance with the public IP**.

And then there is another subnet which is a **private subnet with the instance with only the private IP**.

Now if this is your network architecture, your **instance B could be your application server**.

And often **application server has to make some outbound API calls or download some packages from the

internet**.

But given this situation the question is **can this instance B access an internet?**

So obviously based on what we learned, the answer is **no**.

**Instance B cannot communicate with the internet**.

Now what are the reasons due to which this is not possible.

So as you know, the first reason is that the subnet in which **instance B is launched is not a public

subnet**, it's a **private subnet**.

So there is **no route to internet**.

And second, even if this subnet would have been a public subnet, still this communication wasn't possible

because **instance B does not have a public IP**.

So we understood that these are the **essential requirements for any communication with the internet**.

So now the basic question is then **how do you allow instance B to communicate to the internet**.

So as you might have guessed the solution for this is having the **Nat component**, right?

So in AWS world there is something called a **Nat gateway** which is **managed Nat device**, Nat is **network

address translation**.

And the role of the **Nat** is to **allow the outbound internet access to the instances in the private subnet**.

So **Nat gateway itself sits into the public subnet** because it has to communicate with the internet directly.

It has a **public or elastic IP**.

And then all the traffic which initiates from **instance B goes via this Nat gateway**.

So it acts as kind of **proxy for your EC2 instance P**.

So here, in order to route all the outbound traffic through this **Nat gateway**, you should modify the

**route table of this subnet B** and say all the traffic to the internet should go via this **Nat gateway**.

Once you do that and if you hit some internet IP address, the traffic will first go to the **Nat gateway**

and then from there **Nat gateway sends it to the internet**.

And the same thing happens for the **return traffic**.

The internet will return the traffic to the **Nat gateway IP address**, which is the **public IP address**.

And then **Nat gateway will send that packet back to the instance** which has originally sent this packet

to the **Nat gateway**.

And this is how the **entire traffic flows**.

Okay.

So at high level I hope that is clear.

Now let's take an example and see step by step, what happens when **instance B tries to reach out to

some internet IP address through the Nat gateway**.

So we have **instance P with the private IP address as 10.1.1.11**.

And let's assume that it is trying to reach to some **internet IP address 33.44.55.66**.

So of course **instance B will create an IP packet** which has a **source as its own IP address** and **destination

as this internet IP address**.

Now, as **instance B sends this packet**, it will go to the **route table of instance B that is subnet B**,

and it will see the traffic.

Matches 0.0.0.0/0 and it should go to the **Nat gateway**.

So this packet is going now to the **Nat gateway**.

Now **Nat gateway understands that traffic is received from instance B** and now it has to forward it to

the **internet**.

Now here before it sends it to the internet, the **Nat has to tell internet that where to return that

traffic and that return address is Nat gateway's public IP address**.

So what **Nat does** is now this is called **network address translation**.

What it does is it **replaces the source IP of instance B with its own source IP**.

So that **internet can return the traffic to that IP address**.

So it just replaces the **source with its own source IP address**.

And now this packet goes from the **Nat gateway to the internet**.

Now **Nat has access to the internet** because **Nat is sitting in a public subnet**.

And you can see that there is a **route which goes through the internet gateway**.

Okay.

So the packet reaches the **internet**.

So at this moment now the **internet site will send the traffic back**.

That is a **return traffic**.

And in this case it will have **source as its own IP address** and **destination as the Nat gateway IP address**.

Right.

So it sends it back.

It comes through the **internet gateway** to the **Nat gateway**.

And whenever **Nat gateway sends the traffic to the internet**, it **maintains a table** which says this packet

was originally received by this **EC2 machine**.

So it knows that this packet was originally sent by this particular **EC2 instance B** because it maintains

that in a table.

And now after it gets this **return packet back**, it will just again replace the **destination IP address**

as the **instance B address**.

And at this point, the **traffic then goes back from the Nat gateway to the EC2 instance**.

So this is how ultimately the **traffic flows back from the internet to EC2 instance**.

So this is how **entire flow works**.

So **Nat is network address translation** which actually **allows the outbound internet access to your machine

inside the private subnet**.

Now at this point you must be wondering **can anybody from the internet reach to my EC2 instance B if

I have a Nat gateway?**

So the answer is **no**, because if somebody tries to reach to the **private IP of instance B, which is

10.1.1.11**, it is **not resolvable over the internet**.

Nobody knows this address because it's **inside a private VPC**.

So it is **not routable IP address**.

Maximum people can reach to the **Nat gateway which has a public IP address**, but they can't go beyond

that **Nat gateway IP address**.

So that is the functionality of the **Nat**.

And I hope it is clear to you.

---

### Features of NAT Gateway

- **AWS managed NAT device**  
- Provides **higher bandwidth**  
- Fully managed by AWS  
- No need to worry about **availability and scalability**  
- Typically used so **machines inside the private subnet are not exposed to the internet directly**  
- **Charged per hour** and based on **data sent**  
- Supports **TCP, UDP, and ICMP (ping traffic)**  
- **No security groups**, but **network ACLs still apply**  
- **Ephemeral port range** for outbound connections  
- Requires **publicly routable IP** (Elastic IP preferred)  
- **High availability** depends on **availability zone (AZ)**  

> To achieve **high availability**, multiple NAT gateways across multiple AZs are recommended.  

- Tradeoff between **cost and availability**  

---

So that's it about the **NAT gateways**.

Again, **very important component of the VPC**.

Whenever we talk about **providing outbound internet access to the machine inside the private subnet**, **NAT gateways** are used.

---

Now we will also see the **NAT EC2 instance**, which means what if you **don't want to use AWS managed NAT gateways** and rather **use your own EC2 machine as a NAT**?

This is a **valid case** and you can definitely do that.

So in the next lecture let's understand **NAT instance**.

So that's it for this lecture.
