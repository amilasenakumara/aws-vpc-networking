🖼️Regions and AZs 🖼️

[AWS services scope with respect to VPC](https://github.com/amilasenakumara/aws-vpc-networking/blob/82ffd6c730f872ca69f674d9984a7cfa86c4712f/images/regoins-az.png)

Hi, this is very short but a special lecture and I created this because many a times I get questions

something like can VPC host resources across AWS regions or across AWS account.

And hence it is very important that you see the VPC scope from the bird's eye view so that it is very

clear from the very beginning.

So in this lecture, let's try to relate how AWS account regions AZS and VPC relate to each other.

Okay, so a quick refresher about the regions and availability zone.

Now in AWS everything starts with AWS region.

And as of today there are 32 regions with 102 availability zone.

And this number will grow in the future.

Now why there are so many regions?

So depending on your user base or you know, the data residency requirements, you can choose in which

region you want to deploy your workloads.

Now further, every region is made up of multiple availability zones or also called as AZS, and these

AZS provides the high availability for your workloads.

The way AZS are built, they are separated from each other and they have redundant power.

Their location is separate so that the natural disaster doesn't typically bring down two AZS at the

same time, and then you can deploy your application into multiple AZS so that even if one AC goes down

for some reason, your application is still up and running into another AZ.

And all this really depends on your network architecture.

That means your VPC architecture and we will talk more about that shortly.

So here in this lecture we are going to talk about the VPC.

So if you ask me what's the scope of the VPC.

So the scope of the VPC is at region level.

That means when you create a VPC you create that in a particular AWS region.

Now consider that you are a global company and hence you use multiple AWS regions.

Now in that case you would have to create a different VPC in another AWS region.

And then you can have your applications deployed in both these VPCs in different AWS regions.

Now many a times these applications needs to communicate with each other.

And in that case you can connect these two VPCs over different network connectivity options that AWS

provides to you.

So if you heard about VPC peering or transit gateways, these are the ways in which you can connect

these VPC privately.

But more about that we will talk in the following section.

But you can have that private connectivity okay.

So from this slide I want you to take that VPC scope is at region level okay.

Now let's understand the VPC scope with respect to AWS account region and availability zones.

So everything is part of your AWS account.

So once you get your AWS account you can deploy your workload in any of the available AWS regions.

Right.

So there are all those regions for you.

And as you know, every region has multiple AZS.

Now some region can have like three AZS, some region can have 4 or 6.

That really depends on the size of the region.

So you can go to AWS documentation and check how many AZS are there in a particular region.

So if you talk about Mumbai region, there are three AZS.

And if we talk about the North Virginia region, it has six availability zones.

Now, as I said, when you create a VPC, you create that in a particular region.

So in different region there will be different VPC.

Now you can create as many VPCs that you want in a given region.

Typically AWS will put some default limit generally five VPCs, but you can definitely increase that

limit.

Now one question is why would you need multiple VPCs?

Logically speaking, you can deploy all your application into the single VPC, but with respect to the

best practices, it makes sense to isolate your network based on your application or your projects so

that maybe a finance application is within its own VPC and then air application is in its own VPC.

So there are these kind of best practices to isolate your application at the network layer.

So if you talk about the Mumbai region, you can see here that there is a VPC and it is spanning across

multiple availability zones.

Now if you heard about the AWS service called EC2, which is a virtual machine, then EC2 sits into

a particular AZ.

That means one EC2 machine cannot be in two AZS at the same time.

While launching the EC2 instance, you decide whether it has to be launched in AZ one or AZ two.

Now, if that's the case, then you know that EC2 is launched inside a VPC.

Then how do you make that decision in which AZ, the EC2 will be launched?

Now that is decided using the subnets.

So after you create the.

VPC.

You can create multiple subnets inside the VPC, and while creating this subnet you decide in which

AZ this subnet will be right.

So here if you want to create EC2 instances in three different AZS, then you will need at least three

subnets.

And then you can likewise create as many subnets as you want.

So subnet is mapped to the AZ and VPC is mapped to the AWS region.

So I hope that is clear.

So if you talk about the AWS services like EC2 or RDS, that is database service.

The scope of those services is AZ level, which means you have to carefully design your subnets for

those edges, and then you can deploy these services into respective subnets.

Okay, I hope that is clear.

Now if you talk about other AWS services, for example, Elastic Load Balancer, it distributes the

incoming traffic across multiple machines inside your VPC.

And these machines can be in different availability zones.

So the ELB has a scope of VPC level.

That means it can distribute traffic across any of the AC inside that VPC.

So some of the AWS services will have the scope of the VPC.

So that's about some of the services which works at AZ level and the VPC level.

However, if you talk about the services for example S3 bucket, when you create a S3 bucket, you decide

the region for that bucket.

You never put S3 bucket inside your VPC.

That's because it's fully managed AWS service and AWS manages the network for S3.

So the scope of S3 is a regional right.

So you create a bucket, you decide in which region it should be, and then it can be accessed over

the internet.

Right.

So some of the AWS services will have the regional scope, which means they are not inside your VPC.

And finally, if you talk about the AWS services like route 53, which is a DNS service, it has a global

scope.

It doesn't sit into a particular AWS region they operate at globally.

Similarly other services, for example IAM, which is identity and access management.

Through which you create multiple users into your AWS account.

It also has the global scope, which means IAM users can access any resources across any AWS region.

And similarly, billing service is like global, which means that you get a single bill at the end of

the day, irrespective of how many AWS regions you use.

Right.



So from this slide, uh, what I want you to take is that AWS account is a top level entity.

And then under that you get access to multiple AWS regions.

And then in every region you can create one or more VPCs.

And then in order to use multiple availability zone within the region, you have to create subnets inside

that VPC.

