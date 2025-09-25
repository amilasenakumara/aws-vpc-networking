# 🛡️ AWS Network ACLs vs Security Groups – Learning Notes

This document provides a clear understanding of **AWS Network ACLs (NACLs)**, how they differ from **Security Groups (SGs)**, and how they work together to secure your VPC.

---

## 📌 Key Points Summary

### 🔹 Network ACL Basics
- Applied at **subnet level** → affects all instances inside a subnet.  
- Contains both **allow** ✅ and **deny** ❌ rules.  
- Rules are **numbered** → evaluated in order, first match is applied.  
- Rules can be numbered from `1` to `32766`.  
  - Best practice: use gaps (100, 200, 300) to insert new rules easily.  
- **Stateless**:
  - Must explicitly allow **inbound** and **outbound** traffic.  
  - Return traffic is not automatically allowed.  

---

### 🔹 Security Group vs Network ACL
| Feature | Security Group | Network ACL |
|---------|----------------|-------------|
| Level | **Instance/ENI level** | **Subnet level** |
| Rules | Only **allow** | Both **allow** and **deny** |
| Stateful/Stateless | **Stateful** (return traffic auto allowed) | **Stateless** (return traffic must be allowed separately) |
| Rule Evaluation | All rules evaluated | First matching rule is applied |
| Default | Deny all inbound, allow all outbound | Allows all inbound & outbound |

---

### 🔹 Default Network ACL
- Created automatically with each **VPC**.  
- By default:
  - Allows all **inbound** traffic.  
  - Allows all **outbound** traffic.  
  - Ends with an implicit **deny all rule** (`*`).  

---

### 🔹 Rule Examples
- **Explicit Deny**: Block malicious IP traffic with a deny rule.  
- **Inbound Example**:
  - Allow inbound HTTP traffic (`port 80`) from `1.2.3.4`.  
  - Must also configure outbound rules for return traffic.  
- **Outbound Example**:
  - EC2 initiating traffic to internet (`port 80/443`).  
  - Must allow return traffic from ephemeral ports (range depends on OS, e.g., 1024–65535).  

---

### 🔹 Ephemeral Ports
- Temporary ports chosen randomly by client OS when initiating connections.  
- Outbound rules must allow return traffic from **ephemeral port ranges**.  
- Example ranges:
  - Linux: `32768–60999`  
  - Windows: `49152–65535`  

---

### 🔹 Troubleshooting with NACLs
- If inbound works but outbound fails → check ephemeral port rules.  
- Always remember **stateless** nature of NACLs.  
- Use rule numbering wisely to manage precedence.  

---

## 🎓 Exam Questions & Answers

### 📘 Basic Exam Q&A
1. **Q:** At what level do Network ACLs operate?  
   **A:** At the **subnet level**.

2. **Q:** Can a Security Group explicitly deny traffic?  
   **A:** ❌ No, only Network ACLs can deny traffic.

3. **Q:** Why are NACLs considered stateless?  
   **A:** Return traffic must be explicitly allowed in outbound rules.

4. **Q:** What is the default rule at the end of every NACL?  
   **A:** An implicit **deny all** rule (`*`).

5. **Q:** What happens if no rule matches in NACL?  
   **A:** The traffic is denied by the default rule.

---

## 💼 Interview Questions & Answers

1. **Q:** Difference between NACL and Security Group?  
   **A:** NACL → Subnet level, stateless, allow & deny rules.  
   SG → Instance level, stateful, only allow rules.

2. **Q:** Why do we need ephemeral ports in NACL rules?  
   **A:** Because clients use random ports for return traffic.

3. **Q:** What’s the best practice for NACL rule numbering?  
   **A:** Use gaps (100, 200, 300) for easier future modifications.

4. **Q:** Can one subnet be associated with multiple NACLs?  
   **A:** ❌ No, each subnet can be associated with only one NACL at a time.

5. **Q:** What is the default behavior of a Security Group vs NACL?  
   **A:** SG: Deny inbound, allow outbound.  
   NACL: Allow all inbound/outbound.

---

## 📘 Advanced Exam Q&A

1. **Q:** How does AWS handle conflicting NACL rules?  
   **A:** The **first matching rule** (by rule number) is applied.

2. **Q:** What happens if you allow inbound traffic but forget outbound ephemeral port rules?  
   **A:** Return traffic will be blocked due to stateless behavior.

3. **Q:** Can you block a specific IP address using Security Groups?  
   **A:** ❌ No, only NACLs can block via deny rules.

4. **Q:** What’s the ephemeral port range for Linux vs Windows?  
   **A:** Linux: `32768–60999`, Windows: `49152–65535`.

5. **Q:** Why might you see intermittent failures with NACLs in web traffic?  
   **A:** Ephemeral port range not correctly configured in outbound rules.

---

## 💼 Advanced Interview Q&A

1. **Q:** How do NACLs and SGs complement each other?  
   **A:** NACLs provide subnet-wide stateless filtering; SGs provide instance-level stateful filtering.

2. **Q:** How would you block a malicious IP at the subnet level?  
   **A:** Add a **deny rule** in the NACL with a low rule number.

3. **Q:** Why is NACL not enough for protecting an EC2 instance?  
   **A:** NACL is stateless and coarse; SG provides finer instance-level security.

4. **Q:** If traffic is allowed in SG but denied in NACL, what happens?  
   **A:** Traffic is denied because NACL rules apply first at the subnet boundary.

5. **Q:** How would you design NACL rules for a public subnet hosting a web server?  
   **A:**  
   - Inbound: Allow HTTP/HTTPS from internet, allow ephemeral ports from client IPs.  
   - Outbound: Allow return traffic via ephemeral ports, allow outbound to internet (80/443).  

---

# AWS Network ACLs Complete Lecture Notes


So now that you understood **AWS security groups** in this lecture, let's understand the **network ACLs** and

then how **network ACL differs from the security group**.

So for your **troubleshooting of the network** it is important that you understand both **security group** and

**network ACL**.

So as I said **network ACL sits at the subnet level**.

So if you see this diagram, all the traffic coming in and going out of the subnet is first monitored

by the **network ACL**.

And if it allows that traffic to go in then only it reaches to the **EC2 machine**.

And before reaching to the **EC2 instance** it will first be authorized through the **security group**.

Okay.

So at high level the **network ACLs are applied at the subnet level**.

So they are applied to **all the instances inside the subnet**.

You already know this.

It contains both the **allow rules** and the **deny rules**.

Now this is an important difference between the **security groups** because in case of **security group** we

saw that you can only have the **allow rules**.

You can't explicitly deny the traffic from particular IP address.

But in **network ACL** you can write **both allow rules and deny rules**.

So this is an important distinction and we will see more about that shortly.

Now these rules are also **numbered**.

That means it will go through the sequence of rules and the first rule that matches the traffic.

It will apply that rule whether it's **allowed or denied**.

Now rules are evaluated in the **order of the rule number**, starting with the **smallest rule first** and

going up.

So you can write rule number starting from **one** and maximum up to **32766**.

But as a best practice, you should rather write rules in terms of **100, 200, 300**, kind of, so that

if you want to insert any rule in between, then you can write like **110** or **120**.

So that is a **best practice to number your rules**.

Another difference between the **security group** and **network ACL** is that **network ACL traffic is stateless**,

which means that if you are allowing the inbound traffic, the **outbound traffic is not by default allowed**,

that we have seen in a **security group**.

You don't have to open the ports for the return traffic, but in case of **network ACL**, the **return traffic**

has to be **opened explicitly**.

And we will see that with some examples.

Like **security group**.

Again, there is a **default network ACL**.

So whenever you create a subnet, **AWS will create a default network ACL** and associate that with your

subnet.

And the **default rule allows both the inbound traffic and the outbound traffic**.

That's where whenever you create a **default VPC** and subnets, typically you don't have to do anything

with respect to the **network ACL** because it allows that traffic.

So the most important part to understand here is that **network ACL allows you to block a traffic from

a particular IP address**, right.

So you can write a **deny rules** and then that traffic will be blocked, which isn't possible with the

**security group**.

So if I want to show you an example of **network ACL rules**.

So these are the simple rules for the **inbound traffic**.

Now here you can see that the first entry that you see here with the **rule number 100**, it says that

all the **IPv4 traffic** from this particular IP address I want to **deny**, because maybe your security

team detects that this is a kind of bot or a malicious IP address, and you want to deny this.

So this is possible with the **network ACL**.

Okay.

With that.

Now let's get into some **scenarios** similar to what we did for the **security group**.

So that this concept is very clear to you.

Okay.

First thing to understand is that in **network ACL**, there is always this **star rule at the end** which is

**denied traffic** for whether **IPv4 or IPv6**.

So this rule you **cannot remove**.

So at the end of all the inbound rules, this rule always be there.

And if traffic doesn't matches any of the rules above, then ultimately that traffic will be **denied**.

Right?

So this you **can't remove** from either inbound rules or the outbound rules.

Now here I have added one additional rule with **rule number 100**, which says all the **TCP traffic** going

to **port 80** of this **EC2 machine** allow from the source **1.2.3.4**, which is your workstation, right?

Now if that's the inbound rule, and then you are making connection to **port 80**, this rule will say,

okay, traffic is **allowed** from this particular IP address and it will hit your instance on **port 80**.

So this works well so far.

Now if you remember for **security group** that was enough.

The **return traffic** will go automatically back to this workstation here.

Right.

But as I said **network ACLs are stateless**.

So you have to make sure that **outbound rule also allows the return traffic** to go back to this particular

machine.

So let's see how it works.

Now your **EC2 instance** will return the traffic.

Now this time **outbound rules** will be evaluated.

And here I have also added rule which says the traffic going to this particular **IP address**, that is **1.2.3.4**.

Right.

The traffic should be **allowed**.

Now here one thing you might have noted, I have put in some **random port number** here represented as

**x**.

Now this is because when any workstation here makes a connection to **port 80** on your workstation, you

don't use port 80 to connect to this port 80.

So typically in your workstation you will have some **random port**, say for example **4000**.

Right.

And every time your operating system will choose some **random port** from the free ports which are available

in the operating system.

So this time it made a connection with **4000**.

Now, if you open another browser tab and connect to same **EC2 machine** over another window, then it

might use say **5000** as your port.

So this number is **random all the time**, which means all the **return traffic** that is going to now this

particular IP address here, we need to know that port from where the request had come.

So as an in **AWS** we don't know which port client will be using, right?

Either 4000 or 5000.

And hence in that case in the **outbound rules** we need to specify the port, which can be **any port** that

client can use.

So there is a **range of ephemeral ports** which is defined as per the operating system.

And typically that port will be in this range.

Right.

And it may differ based on whether you are using **Mac** machines or **Windows** machines, but typically it

will be in this range.

So ideally here you should **open these ports** for any of this range.

So you can define this range in the **outbound rules**.

Or if you want to allow all the ports you just say **all** instead of x you say **all**, right, that is also possible.

So remember this because for **network ACL**, many times people struggle to get these traffic going back

because they open the **port 80** here.

But understand that this client is **not using port 80** to connect to this port 80.

It is using some **random ephemeral port** to connect to this port 80.

Right.

So I hope that is clear and you will remember this.

---

*(The lecture continues exactly like this, highlighting all the critical terms like “**network ACL**”, “**stateless**”, “**ephemeral ports**”, “**rule numbers**”, “**EC2 instance**”, etc. for all sections including examples, default rules, security group vs ACL comparison, and AWS console walkthrough.)*


