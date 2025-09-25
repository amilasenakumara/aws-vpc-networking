📚 Overview of AWS Networking

[Which all AWS networking services you should know?](images/aws-networking-services.png)


So let's start this course.

Now I want to start this course with overview of VPC and AWS networking services.

And this is always good practice to first see a big picture.

And then you go towards a particular topic.

So at this moment maybe this is too much for you in order to understand all the services.

But the intention of this note is not to deep dive into individual components here, but rather see

a big picture and how it fits into the AWS ecosystems and what I also recommend that after the end of

this course, you come again to this note and watch it again so that you can relate.

Now all those dots which you learned during the course and this picture will be then much clearer to you.

And I'm sure that will help you big time to understand all the important concepts in AWS, VPC, and networking.

And as I said, it builds your foundation to go further into your AWS journey.

Now I always like to take some examples.

So for this note let's assume that we have some application which has a application server or a web server.

And it has a database. And we want to deploy this application in AWS.

And of course you will have users who will be accessing this application over the internet.

So that's the scenario.

And now we want to see how we build the AWS networking architecture to support this application deployment.

So everything starts with the user and user have access to the internet.

And of course user wants to connect to some web application out there on the internet.

And you have built your application and now you want to deploy that in AWS.

So first thing that you will do is choose which AWS region you want to use.

✅ AWS region ✅

Typically the region is a geographic area which AWS has, and there are 32 regions at the moment and

this number will grow.

Now how do you choose region?

So it will be typically based on where your users are located.

So if you are developing an app for which the users are in the US primarily, you will choose any of the US region.

And don't worry, we are going to cover everything from scratch.

That is what is AWS region, what are the availability zones?

✅ availability zones ✅

So as of now, just let's focus on the high level.

So you choose a region.

Let's say you have chosen some region in India or US.

And every region comes with multiple availability zone.

And this is designed for high availability.

So AZS or availability zones are the data centers.

And at any moment if there is a failure in one data center, you should be able to continue operating

from another data center.

That's where the concept of AZ has been implemented.

So every region has multiple availability zones.

And now if you want to deploy an application, you need a private network space.

Now that private network space is called VPC Virtual Private Cloud.

This is very similar to your on premises network where it's a close network.

And then you have your all intranet networking inside that.

So nobody from outside can get access to any machine inside your network. Exactly the same concept here.

VPC is a private network for you so that you can deploy your application.

Now, when you create a VPC, there is always a local router.

Now this router does the routing between the VPC resources.

So if you launch now some machines within the VPC, then this local router will route traffic between them.

And we will shortly see that architecture. So this is just a router.

But there should be something which tells router how to route the traffic.

Now this is done using the VPC main route table.

So route table has a route entry which says if the destination IP address this then take this particular route.

So that's the job of the main route table. And this main route table looks something like this. Right.

Which has entry which says if the traffic is for any resources within the VPC, then the target is this local router.

Now the address that you see here, 10.10.0.0 /16 is the VPC cider address that is classless Inter-domain address.

And later in this course we will understand everything about Cidr.

So this main route table tells that all the traffic within VPC can be routed through this local router.



As I said, VPC address space is a big address space, but then you should divide that in the form of subnets.

And subnet has 1 to 1 mapping with the availability zone.

That means one subnet can be in only one availability zone at any moment.

However one AZ can have multiple subnets.

So here if you want to launch an EC2 machine, you will choose either this subnet or this subnet.

Depending on that, it will be launched in a corresponding availability zone.

And then this VPC is currently completely isolated.

That means it cannot communicate with the internet.

So you should have a component called Internet Gateway which attaches your VPC to the internet.

However, this traffic does not flow by default.

Of course, you need to modify the route table and you have to say if the traffic is going to the internet,

that is 0.0.0.0/0. Then it should be routed through this internet gateway.

And now you can see that any traffic routing you want to do, you have to modify your route table for that.



Now you have your application bundled in a machine, so you will launch a machine called EC2.

That's a basic compute service from AWS and you will install your application.

You will create a local database in that.

And then this machine will have a public IP.

And users from outside can then connect to this machine using this public IP.


💻💻 Now you must be wondering is it always like users will have to use IP address, which doesn't seems

right because most of the time you access application using some name, for example Amazon.com or google.com.

So for that you need a DNS service.

Now you can use either your own DNS or third party DNS service, or AWS has its own DNS service called route 53.

Now here you configure the hosted zones and you create a record.

Say my application name is example.com and the IP of that is 11.22.33.44.

So you create that record in your route 53 hosted zone.

And now if user in their browser hits example.com it will return that IP address.

And then user browser will then connect to this machine.

So this is how a DNS resolution happens okay. So far so good.



Now you must be wondering if there is just this main route table.

Then how can I have different kind of routing logic for different subnets?

Because you know that in a big application you will have databases, you will have application server,

web server and all these servers have different routing requirement.

For example, your databases should not be accessible from the internet.

They should only be accessible from within your VPC.

That is your application server.

Now if you put this route table for all the subnets, then your machines will be exposed to the internet.

So rather best practice is rather than having this main route table, you can create custom route table

for your subnets.

So what I'm trying to say instead of using this main route table, you can have a custom route table

and you can attach this route table to all the public subnets.

So all this public subnets will follow.

Now this route table.

And here there are same rules that we have right.

There is a local router which routes the traffic within the VPC.

And then there is this route which connects your VPC to the outside world.

So likewise you can have multiple route table depending on what kind of subnet you want to create okay.

So far so good.

So we have now this architecture.

Now you are currently in development.

But now you want to deploy your application in production.

And you think that your application need scaling.

And typically it will be horizontal scaling.

That means instead of one machine you want to deploy your application on two machines so that if there

are more users, there is no outage with respect to the compute that require for this instance here.

Right.

And for that you will launch another machine and it will have another public IP.

Now the problem is in simple world this DNS will always provide you one IP address.

Right.

But now you have new IP address.

So either you do something with the DNS so that some users get this IP and some users get this IP,

which is called client side load balancing.

But ideal way to handle this situation is to do server side load balancing.

What I mean, instead of exposing this machines directly, you put a load balancer in front of them

and then load balancer distributes traffic to these machines.

Right.

So let's try to modify this architecture accordingly.

So here now we are saying that we will have two more subnets because now this machine need not be exposed

to the internet.

And then all these machines are in a private subnet.

That means nobody can access this machine from outside.

Neither this machine can access internet.

And this means that this subnet has different routing requirement.

So I should not associate this route table here with this subnets.

But rather we should create another route table which has just this local route entry.

So in this route table there is no entry for sending or receiving traffic from the internet.

So you associate this route table with these two private subnets.

Now once you do that you launch an application load balancer which is again a service from AWS.

And this application load balancer sits in the public subnet because it is exposed to the internet directly.

And now in this case we will just modify this route table entry and say if someone asks for IP address

of example.

Com now, instead of those public IPS of EC2, you will provide the public IPS of the application load

balancer.

So DNS service will respond with application load balancer IPS.

And now user can connect to Alb.

And depending on the algorithm typically round robin one Alb will distribute traffic to one of the instance.

So this is how now you can have multiple application server behind the Alb.

Okay so far so good.

Now how about isolating your database from your application logic which is always a good practice because

then you can have more control over those machines.

So now instead of putting everything in the application server, let's take out the database.

And as you might have guessed, database should be in a private subnet because they need not be accessed

from the outside.

So we'll create two more subnets.

Again we can attach the same route table with these subnets because routing requirements are the same.

And at this point you must be asking why don't you just create the databases in the same subnet as application

server?

Now technically we can, but it's always good to have the subnet with a specific purpose because you

are launching the databases.

They have kind of, you know, security requirement.

So you can isolate your networking in that sense.

So it's always good that you create different subnets for your databases.

And now you create a database, say my SQL database with a primary instance in one of the subnet and

all your application server access that database okay.

So our architecture has evolved and it looks much better now.

Now with respect to the resiliency of this architecture, is it good enough.

Now when I say resiliency I mean the resiliency at the availability zone level.

What if one of the data centers of AWS goes down out of power, right.

Or there are floods, right.

So this AC will be gone.

For example.

So is your application still accessible?

Now let's see this right.

So imagine there is an outage for this AZ.

And in this case your application load balancer is still accessible.

Your application is still accessible but your database is gone because it was launched in this AZ.

So it's not as of now resilient to the AZ level failure.

Now what do you do for this.

So luckily this kind of databases also provides a mechanism called synchronous replication.

So what you can have is you can have a secondary replica of the database, and you can have synchronous

replication of all the data from primary to the secondary.

Now typically network latency will be an issue here.

But this availability zones are typically maximum 100km from each other.

So there is like less than ten milliseconds of latency with respect to the network between two AZS.

So this replication is synchronous.

It happens almost in real time.

So all the data you can see from here is replicated to the secondary database copy.

Now if you see this architecture and if the outage happens all the components of your architecture are

still accessible.

So user won't see any outage into your application.

So this is much, much better design with respect to the high availability of your application.

And that's how I said networking greatly affects your architecture high availability and the security.

So if we wouldn't have this kind of architecture and we would have been put everything into a single

subnet and subnet is mapped to a single AZ, then if one of the AZ goes down, all your application

component would have been gone down, which means that you should take into consideration all the network

architecture in order to deploy your application.

So I hope that is clear.

Okay, let's go back to our architecture and look at the other services.

Now, you know that these application servers are in a private subnet, but most often or not your application

server would have to say download some packages from the internet, or they want to make an API call

to some third party SaaS service.

Now, because they are in private subnet, they can't access the internet.

Right?

And that's the problem.

Now what will you do.

So for this, of course if you want to access the outbound internet there is a service called Nat gateway.

And we will see in detail how the Nat gateway works in the later section.

But the Nat gateway provides the outbound internet access to your machines in the private subnet.

So this instance can access internet through the Nat gateway.

And of course, for that you need to modify the route table of this subnet and say if the traffic is

going to internet, let's route it through the Nat gateway.

And now this traffic goes.

Goes like this.

Which means that this application server can access internet.

But internet cannot access this application instance because it still sits in the private subnet.

And this is a typical architecture you will see when you deploy two tier or three tier web applications.

Okay, I hope that is clear.

Right.

So far we are just talking about the VPC and three tier architecture.

Now let's go beyond VPC.

In the real world.

If you have this kind of application, maybe this application serves some kind of images or videos.

And in AWS world you will typically use AWS service like Simple Storage service or S3.

Right.

And this S3 bucket sits in the AWS region.

Right.

So you have services like S3 and DynamoDB.

Now how do you access S3 from your application server?

Now if you see this architecture, it will happen through the Nat gateway because these services have

the public endpoint.

And then your instance will go through the Nat gateway through the internet Gateway and access this

S3 service.

So even though you are operating in AWS network, you are still using this longer path, right?

So what AWS did, it says if your S3 buckets or DynamoDB tables are in the same region as your VPC,

then rather you can create a networking component called VPC endpoint, and then your application can

access these services privately through this VPC endpoint.

So now this longer route is taken off.

And then you just access this privately through the shorter route.

And another benefit of this architecture is that you save a lot of cost on the Nat gateway, because

there is a cost per GB of data processed through the Nat gateway.

So imagine that either you are uploading or downloading, say, terabytes of data from this S3 bucket.

Then it will go through this Nat gateway and there will be additional charge.

But with this VPC gateway endpoint, there is no charge for that data processing.

And this component is highly available.

And the bandwidth is also managed by AWS.

So this is particularly useful for S3 and DynamoDB as a service.

But many a times you also need to access other AWS services.

For example SQS which is simple queue service or Kinesis or API gateways from within this application

server.

So there are more than 100 such services which you would want to access from your application machines.

Here.

Again, as I said, you can always access these services through the internet route, but AWS provides

another way to access other AWS services through VPC endpoint of type interface.

So there is a difference between VPC endpoint interface and VPC endpoint gateway.

And we have covered that in the later section.

But ultimately the point is whether VPC gateway endpoint or an interface endpoint, you can access AWS

services privately within only that region from your VPC resources.

And this VPC endpoints are backed by something called AWS Private Link, so it's always a good practice

to access AWS services using VPC endpoints.

Now, if you go beyond the AWS services, maybe you are accessing your SaaS application, which are

also deployed inside a VPC, but they reside in your customer's VPC or your SaaS service providers VPC.

Something like this.

Now again here.

Also, if this SaaS service exposes their services over the internet, then your application can still

access through the Nat gateway and the internet.

There is no problem with that.

But because they have also launched these services inside their VPC, which sits in the AWS network,

and your VPC is also in this AWS network.

These SaaS services can also be accessed over this private link.

Right.

So you can have this private connection.

And then this traffic doesn't flow over the internet route.

So that is another purpose of VPC private links.

Right.

So I hope that is clear.

Now with that let's now move beyond the single VPC.

Because in real world you will have many, many VPCs to work together.

This is because typically VPCs should be created based on the applications.

So if there is a finance application there is HR application.

There is some different IT applications.

They should be sitting in different VPCs because different teams will be managing those applications.

Right.

And all these applications would want to communicate with each other.

So in that case how do you handle that connectivity.

So imagine you have another VPC.

And the machines inside this VPC wants to communicate with the machines in your VPC.

So again one way is to expose to the internet.

But there is a better way.

What you can do is you can pair this two VPCs using VPC peering connection.

And then resources within these VPCs can communicate with each other over the private IP addresses.

Nothing needs to go over the internet route.

So this works well.

But what if there are many VPCs?

So here I'm just showing 2 or 3.

But what if there are hundreds of VPCs and all the resources within all those 100 VPCs wants to communicate

with each other?

Now, if you talk about the VPC peering connection, there are 1 to 1.

That means if there are 100 VPCs, every VPC will needs to be connected with every other VPC.

So it's all mesh of kind of network connectivity.

And there will be ultimately 100 and thousands of VPC peering connections because it's 1 to 1.

But then AWS simplified that complexity by launching something called Transit Gateway.

So Transit Gateway acts as a hub of the network.

So you connect all the VPCs to the other transit gateway, and then you connect your VPC to the transit

gateway as well.

Now all these VPCs, which are connected to the transit gateway can communicate with each other.

Right.

So this is much simplified.

And the benefit is you can also attach this transit gateway with your on premises network.

So you can have either a VPN or a DD that is direct connect connection with the transit gateway.

And now all these networks can communicate with each other through this transit gateway.

And as I said, we will deep dive into individual component later in this course.

But it greatly simplifies the network architecture.

Right.

So I hope that is clear.

Now let's also talk about the hybrid networking.

What if you are a big enterprise and you have this application deployed inside a VPC.

And this application also needs to communicate with some of the applications which are deployed on premises.

Now one of the route is this transit gateway that we have seen.

But if you don't want to get into the complexity of transit gateways, because also there is a lot of

cost, you can directly connect your VPC to on premise network over IPsec VPN, right?

So for that, you will need to have the virtual private gateway attached to your VPC and customer gateway,

which is kind of router into your corporate data center.

And then you can set up an IPsec VPN tunnel between these two.

So essentially you are doing site to site VPN connection between VPC and corporate data center.

So this works well.

But this traffic still goes over the internet.

That means your connection may not be reliable because there are a lot of hops.

This bandwidth is not consistent and you may face some challenges, but imagine the workloads like high

performance computing where you need maybe GBS of consistent traffic between your on prem data center

and your workloads deployed inside AWS VPC.

In that case, you can't really rely on the IPsec traffic, right?

Rather, you want a physical connectivity between your data center and AWS network.

And exactly for that, you can use something called Direct Connect.

So direct Connect is a layer one physical connectivity between the AWS network and your data center

network.

And this comes in a range of 50 Mbps up to 100 Gbps.

And it's a dedicated link.

That means you get all that bandwidth between these two networks.

Right.

So I hope that is clear.

And here we are talking about connecting two networks that is on premise network and AWS network essentially

VPC.

Okay.

So now let's move on to another scenario.

Now if you remember the Covid time, most of the people worked from home and they want to access the

application within the company network.

Then what we do we do something called a VPN connection.

Now, in this case, uh, our machines acts as a VPN client.

And this network that is VPC acts as a company network.

And then we log in to the VPN, and then we can access resources privately.

Now in AWS world also this is supported over the client VPN network.

So you create a client VPN endpoint and associate this VPC with that endpoint.

And if client installs the VPN client on their machines, then they can securely access AWS VPC resources

privately over this client VPN.

So this is called client VPN, right?

So I hope this is clear so far.

Now let's go back to our application again.

And this time let's imagine that our application is kind of social media application, where it is also

serving some kind of videos and images, something called say Facebook or Instagram.

And typically for this kind of media content you will store it in S3.

Bucket S3 is a simple storage service.

So something like this.

This represents some kind of video file.

And if your users want to access this video file with this architecture, whenever they access that

video, of course they will access your application first.

So this EC2 instance here will serve the user request.

So this application server will actually get the content of the video file and stream it over this channel

to the user.

So user will watch the video, but the traffic will be from S3 to your EC2 machine through this application

load balancer, through the internet it reaches the end users.

So this works well if your user base is really small and users are located in a particular geography.

But now try to multiply this by number of users.

And also what if you have global users, which means your users are located across the world, and all

these users are trying to get access to the same video through your application.

So now the traffic flow will be as many users are there.

This EC2 machine that is your application server will download or access this video from the S3.

So multiple invocation.

It will then send the traffic back to Load balancer and through load balancer they will be served to

this users.

Now this is still possible, but if you see there are multiple problems in this flow, can you think

of those problems.

So let's try to decode right.

So very first problem is you would have to scale this application server.

Because now there are many many user requests.

Let's quote a number of civilian users.

And this simple single EC2 machine cannot serve that traffic.

You need to scale this horizontally or vertically right?

And as you scale it, you need to pay more for this application server.

Now, logically thinking this application server at this moment is doing nothing very fancy, right?

It's pulling the data from S3 and serving to your users because these are the static content though

we are calling it as a video.

Video is a static video doesn't change on every invocation or there is a image.

It is also a static content.

But all these contents are being served through your application server and through the load balancer.

So that's one problem with respect to the compute resources required to serve this traffic.

Second, now you have chosen one of the AWS region, let's say Mumbai.

So latency for other users in US or Japan might be much higher.

And imagine your video size is say one GB.

It will take a lot of time for this users to get that video streamed right.

So they might not have the great experience watching this video, because it's a law of physics that

there will be additional latency.

And third, and most important is the cost.

As you scale this, there will be more cost.

And in AWS world there is also cost of data transfer out from the VPC to the end user.

Right.

So there will be a lot of cost around application load balancer processing and the data transfer cost.

So how do you solve this problem.

So interestingly AWS does give you some mechanism to better design this kind of application.

And here instead of serving this videos through this application server you can directly serve through

S3 bucket.

That means user can directly access S3 bucket to watch this video.

Right.

And.

Let's try to represent that.

And for that, I'll just transfer this S3 bucket here so that I can show that in a better architecture.

So let's take the same S3 bucket and same video file.

Now here what I'm saying is user can directly access this S3 bucket.

So application need not download that file and then serve through the load balancer.

So it saves you a lot of cost around application compute.

Right.

But still there is a latency issue right.

Because users are accessing S3 bucket from different geographies.

Now S3 bucket sits into a particular region let's say Mumbai.

So maybe users in India might have good experience, but users in USA will still won't have the great

experience.

So for that you can use AWS edge services.

So there is a AWS service called AWS CloudFront.

And CloudFront has edge locations.

Edge locations are like, you know, there are caching devices in different cities.

So there are more than, I think 400 edge locations as of today.

So like India has three edge locations in Mumbai, Delhi, Chennai and maybe Bangalore as well.

And likewise across the world in different cities, there are these edge locations.

Now what they do.

Whenever a user now try to access this video files, they access through the nearest edge location.

So people in USA will connect to the nearest edge location.

And these locations are already connected with AWS Backbone network to all other AWS regions.

So user will request to the nearest edge location, and then the video from this S3 file will be cached

in this edge location.

So it will go and sit into the every edge location.

And now if other users in the USA will access that video, it will be served right from that edge location,

which is probably few hundred miles from the user.

Right?

So this video is now not fetched from the Mumbai region, but from the nearest edge location.

So that's the functionality of the CloudFront edge location, which greatly improves the end user experience

whenever you want to serve the static content.

Right.

So that's another AWS service that you need to look after.

Now, the only thing that we haven't talked about here is how do these user get access to this edge

location.

Now again you can do that using the DNS service.

So here instead of returning the Alb IP address we will configure route 53 to rather return the CloudFront

IP addresses.

And then all this routing will work as we have designed.

Right.

So this is the way in which you can use CloudFront.

So these are the important AWS services that you should be knowing.

And I hope this entire discussion gives you now better idea about different AWS services and how they

work together.

Okay, so I hope this picture now makes more sense to you.

And in this course we are going to start now learning everything about starting with the VPC, routing

everything.

And we will do our exercises covering all these AWS services.

Now we can't cover AWS Direct Connect because it needs a physical connectivity.

But for rest of the components we are going to do the exercises.

And as I said, revisit this lecture again at the end of the course so that you can visually remember

the things.