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

| Rule | Type           | Protocol | Port | Source           | Allow/Deny |
|------|----------------|---------|------|-----------------|------------|
| 100  | All IPv4 traffic | All     | All  | 180.151.138.43/32 | DENY       |
| 101  | HTTPS          | TCP     | 443  | 0.0.0.0/0       | ALLOW      |
| *    | All IPv4 traffic | All     | All  | 0.0.0.0/0       | DENY       |



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

# AWS Network ACLs: Detailed Examples and Concepts

With that, let's go forward.

Now here is another example.

Instead of now this 1.2.3.4, I have another host with the IP address of 9.10.11.12.

Now, if it tries to reach to the same EC2 instance and we haven't modified our inbound and outbound rules, then the traffic will reach here.

And of course it try to match any of the rule numbers starting with the one it says 100. **Rule is for this IP which is not this IP.**

So this rule is not evaluated.

And then it will ultimately be **denied because this rule matches which says all the IPv4 traffic should be denied unless the source IP is this.**

Right.

So this is an **implicit deny**.  

This **star rule** matches and the traffic is dropped.

Now this is one way.

Other way is if you want to **explicitly deny traffic from this machine** what you can do, you can just add one more rule and say traffic from this particular IP.  

I want to **deny**.

Now again, if this machine tries to reach to your EC2 machine, this time, this rule number 101 will be evaluated and the traffic will be **denied**.  

So either **explicit deny or implicit deny** with the star.  

In both the cases, this machine cannot reach to your EC2 instance, right?

---

Okay, now let's take another example.

Now I have this host 9.10.11.12.

And I have also this inbound rule which says **traffic from this host should be allowed**.  

Right? This this particular rule.

Now if this hits the **Network ACL rule**, then this **rule number 200** will be matched.  

**Traffic will reach to port 80 of the EC2 machine, and now the return traffic will go back to the outbound rule.**

Now here, if you see there is **no outbound rule which says the traffic should go to this particular IP address**, right.

So in this case **return traffic is not allowed**.  

So the **packet is dropped**.  

So this **network flow doesn't work** like that.  

Obviously the option for you is now to **open also the outbound traffic** here using outbound rule.  

So for that you will just add another rule.  

Here, you say the **traffic to this particular IP address should be allowed**.  

And as I said you need to provide a **port range which is ephemeral port range**.  

Or you can say **all**. Right, now once you do that, you are now able to get the **inbound traffic and also the return traffic going back to your machine.**  

So remember that you have to add this kind of rules for the **network ACL**.

---

Now let's look at some more scenarios and then we'll compare both **network ACL** and **security groups**.

So in this case let's now understand about the **outbound traffic**.  

That means **traffic which is originated by your EC2 instance**.

So as I said whenever EC2 instance now originates the traffic, again it doesn't use port 80 of this machine.  

It uses some random port that is **ephemeral port for outbound traffic**.

So this is a scenario where from EC2 you are trying to download something, right?  

So it is not a workstation which is connecting to your machine and you are returning the traffic.  

It is like your machine is **initiating that outbound traffic**.

So again, if you do that, of course the traffic will first reach the **network ACL**.  

It will check the **outbound rules**.  

Now in this particular case I don't have any allow rules in the outbound rules.  

That means the traffic will be **simply dropped**, right. This rule will be matched.  

But if I want to go to this particular IP address, which is some IP on the internet, of course I need to **allow the outbound rule** in this, right?

So what I'll do is I'll add one more rule which says I want to go to this internet and this is my target port of this machine, which is **port 80**, because where I'm trying to connect probably run on port 80 or 443 depending on HTTP or HTTPS.  

And I need to explicitly define that port.  

So in this case the **outbound traffic will be allowed** to go to this.

Now again, because of the **stateless nature of network ACL**, you need to think **will return traffic be allowed?**  

Now just looking at this inbound rules, you know that there is **no inbound rule to allow traffic coming back from internet** to your EC2 machine, right?  

So that means **return traffic will be blocked again**.  

And if you want to allow this return traffic, you know what to do, right?  

You need to add another rule which says the **return traffic from this particular instance should be allowed**.  

Right.  

And you know that this port here should be the port here, right?  

You are not reaching to the port 80 of the EC2 machine.  

You are reaching back to the port which was used to **initiate that outbound connection**.  

And that's where you need to have some **ephemeral port range here**, which will allow this traffic to go back.  

So again, this kind of port range, or for simplicity, you can just use **all**, and the traffic will reach back to your EC2 machine.  

So this is a flow for the **outbound traffic initiated from your EC2 instance**, right?  

I know that it is a little complicated, but if you go through this video again, if there is a doubt, I'm sure it will be clarified.

---

Now let's look at the **default network ACL rules**.  

So there are **inbound rules and the outbound rules**.  

So you can see that by default **network ACL** will have these rules which **allow the inbound and the outbound rules**.  

So **all the traffic can come in and go back from where it is coming**.  

Right.  

So this is **allowed**.  

Similarly **all the outbound traffic is allowed** because this rule is there.  

And again the **return traffic will be matched with this**.  

Right.  

So that's where for **network ACL** you don't have to do anything when you create your subnet.  

Because **default network ACL will allow both inbound and outbound traffic**.  

So this is for **IPv4**.  

Similarly, for **IPv6** there will be additional rules just that.  

Instead of 0.0.0.0, there will be this notation to represent IPv6 addresses.  

So if your VPC has enabled both **IPv4 and IPv6 addresses**, then automatically these rules will be added to the **network ACL**.

---

Okay. So that's it about the **network ACL**.

Now quickly let's look at the difference between the **security groups and network ACL**.

- Security group works at the **EC2 instance level**, which actually means any level that is elastic network interface.  
- Network ACL operates at the **subnet level**.  
- Security Group only has **allow rules**, and network ACL has **both allow and deny rules** with respect to the traffic.  
- **Statefulness:** Security group has **stateful traffic**, which means **return traffic is automatically allowed**, but network ACL has **stateless traffic**, which means you have to **explicitly allow the return traffic** through the outbound rules.  
- **Rules evaluation:** Security group evaluates **all rules before making a decision**, but in case of network ACL, rules are **numbered**, and the **smallest number rules are evaluated first**. As soon as a rule is matched, the **rest of the rules are not evaluated**.  

**Major difference:**  
- Security group = stateful, allow only.  
- Network ACL = stateless, allow + deny, numbered evaluation.  

---

Now there are certain limits, and these limits always keep changing:  

- How many rules you can have in **security groups**.  
- How many **security groups** you can attach to your machine.  
- How many rules you can have in **network ACL**.  

I don't want to put those numbers here because they are always changing.  

If you want to know those numbers, you can visit the **VPC quota page in AWS documentation** to verify.  

---

With that, I hope the difference is clear between **security groups and network ACL**, and you are very well able to **troubleshoot** if the network traffic is **not reaching to EC2 machine** or it is **not going out to the destination**.

---

Now, before we wrap up this lecture, I just wanted to quickly show you in **AWS console** how **network ACL looks**, and then we'll move to the next topic.  

- Go to **VPC**.  
- Every AWS region will have **default VPCs**.  
- Additionally, you might have created your own.  
- Select a **default VPC** and note down the **VPC ID**.  
- Go to **subnets** and filter by this **VPC ID**.  
- Select a subnet and go to the **network ACL** tab.  
- You will see the **default network ACL rules**:  
  - Default rule **allows all inbound traffic**.  
  - Default rule **allows all outbound traffic**.  
  - There is also a **default deny rule at the end**.  

So this is essentially the **network ACL**, and it will become clearer with exercises.  

---




*(The lecture continues exactly like this, highlighting all the critical terms like “**network ACL**”, “**stateless**”, “**ephemeral ports**”, “**rule numbers**”, “**EC2 instance**”, etc. for all sections including examples, default rules, security group vs ACL comparison, and AWS console walkthrough.)*


