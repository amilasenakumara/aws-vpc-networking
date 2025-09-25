So let's start with very first lecture that is Amazon VPC.

☁️ VPC is virtual private cloud. ☁️

And it means that you get your own private networking space inside AWS where you can launch machines.

So before we really get into the VPC world, let's first understand how networking looks in general

in your on premises world, and then how you can translate that to the VPC so that you understand this

concept really well.

So before getting into the VPC, let's first understand the typical network setup that you see in your

offices or in your home. So how does that look right.

So this is the network topology that probably you are familiar with.

You know that if you are logged into your office network you are connected to a particular LAN.

LAN is a local area network. So on a single LAN there will be multiple computers or hosts connected over a physical link.

And then there is a device called switch which operates at second layer of the networking, which makes

sure that packets are delivered to the right host within that local area network.

So here, if you see if host A wants to send a packet to host B, it will be done using the Mac addresses.

And then switch can do this job for you. So packet will flow to host B.

So host A and other hosts including host B are part of the same LAN network.

Now if host A wants to communicate with host C here then it has to cross the land boundary.

And that's where router comes into the play.

So router connects multiple networks together.

So in this case this switches will ultimately send this packets to the router.

And then the router will deliver it to the target network.

And then ultimately it will be delivered to the target host.

So the packet will flow like this.

So in any company it is typically a mesh of the architecture where there are multiple Lans.

And then all these lands are connected using the router.

And if you want to access the internet, typically there will be one router which allows that communication

across your company to the internet.

And this is a top router you can see here okay.

So this is a traditional networking.


🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩

But now over the time you know that offices has a global presence.

There are teams who are distributed across the countries or cities or offices.

And they still want to be able to communicate with each other.

And maybe some application team developers are sitting in one office and some developers are working

remotely, and they want to make sure that they still have access to the same network.

So in that case, if you see this, there are three machines which are part of one network, and the

host D should also be a part of the same network.

Now if you see at a physical layer, they are part of different LAN.

But with the virtual networking, it is possible that all these hosts are part of the same LAN and that

is essentially called virtual LAN or Vlan.

So it is very well possible that they are part of the same Vlan network.

🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩🚩

So in this course in particular, we will be focusing more on the AWS side of the networking and how

[we translate this network to AWS VPC network.](https://github.com/amilasenakumara/aws-vpc-networking/blob/18fc92f133bf3801dd14dbb8cebc97638a24364c/images/physical-cloud.png)

But if you are interested in understanding very basics of computer networking, then I will highly recommend

that you just visit, uh, this particular YouTube channel.

But in this course, as I said, we will be focusing more on AWS side of the networking.

So if you have this kind of physical network and if you want to translate it to AWS VPC network, then

things are not very different.

Ultimately, whatever you see on the left is represented as a VPC in AWS world.

So you can have similar topology that you have in your physical network into the AWS in the form of VPC.

And once you have this VPC as your own private network, you can then connect to the resources inside VPC.

When I say resources, I mean EC2 machines or databases.

Then you can connect to them over the internet.

Of course, you have to make sure that your VPC has a router which connects to the internet.

Otherwise VPC is a closed private network.

Okay, now let's look at a little more closely into the VPC okay.

So here I'm showing the same physical network but just represented a little differently because I want

to show you how it exactly looks into the AWS world.

So in AWS.

The same network can be represented as a VPC, and VPC is created in a particular AWS region, and then

you can have multiple subnets in the VPC which represents this individual LAN networks.

And then you launch your workloads inside the subnets in the form of say EC2 instances.

Something like this.

And to enable the network communication within the VPC.

That means within that private network there is a local router, and if you want to connect your VPC

to the outside world, that means to the internet.

Then there are some gateway devices like Internet Gateway.

Now we are going to deep dive into all these components in the following lecture.

But from this lecture, I just want to let you know that your physical network can be more or less mapped

to the virtual network in AWS in the form of VPC.

So here onwards, we will be focusing all our discussions around the VPC and VPC internal networking.

So that's it for this lecture.

And let's now deep dive into the following topics.

Thank you.


