# Deepdive VPC and Advanced VPC Designs

Amazon VPC (Virtual Private Cloud ) enables to have complete control over AWS virtual networking environment.  How new Amazon VPC features might affect the way to design AWS networking infrastructure, or even change existing architectures? Let's explore the new design and capabilities of VPC and how to use them.

## Learning Objectives

-   Understanding of all networking services and when to use each
-   Understand the latest updates and how each is used
-   How to take existing architecture to the next level

![VPC Picture november 2018](https://raw.githubusercontent.com/ealtili/Blog/master/AWS/vpcdeepdive/november2018vpc.png)

VPC is a region level service which you assign a cidr address range. There are multiple availability zones, public subnets, private subnets. We can deploy ec2 instances inside these subnets in the availability zones of the VPC in the region.

So we can use ec2 as a base level service here to use as an example for connectivity. 

AWS services has two different classes or types of services which are public services and private services. 

Anything inside the VPC is private and it's a domain that you manage and you control.  

Anything outside of the VPC is generally public unless you build a private connection to a service such as s3, DynamoDB, Lambda, SQS, SNS etc. all of these services are all public services.

![Internet VPC](https://docs.aws.amazon.com/vpc/latest/userguide/images/default-vpc-diagram.png)

When we're communicating inside the VPC we will actually use route tables that are assigned to subnets.
The route tables basically give you control over everything that's going on inside the VPC. Where traffic can communicate to and that sort of thing.  Routing is the key here on on how we communicate with everything else.

Internet gateway (IGW) gives us access to public services and also the public Internet. We just build a default route to the IGW. When you have a Public IP or Elastic IP assigned to an instance we can then communicate with all of these public services and also the public internet. 

![NAT Instance](https://docs.aws.amazon.com/vpc/latest/userguide/images/nat-instance-diagram.png)

For a private subnet we can send traffic via NAT instance or NAT Gateway instead of directly to the public Internet.  Then the NAT instance or NAT Gateway (is a managed scalable service) has the ability to communicate with the public internet. 

![NAT Gateway](![](https://docs.aws.amazon.com/vpc/latest/userguide/images/nat-gateway-diagram.png)

We can also connect to s3 and Dynamodb privately using VPC gateway endpoints.  A gateway endpoint is a gateway that you specify as a target for a route in your route table for traffic destined to a supported AWS service. We'll use prefix lists in the route tables to communicate privately. VPC gateway endpoint is like a bridge between s3, dynamodb and VPC.  VPC Gateway endpoints are horizontally scaled, redundant, and highly available VPC components. They allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic.

![VPC Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-s3-diagram.png)

We might want to connect to our own premises so we'll use Direct Connect or VPN. 

![VPC](https://raw.githubusercontent.com/ealtili/Blog/master/AWS/vpcdeepdive/VPC.png)

Transit gateway is a service that was launched in about November 2018. Transit gateway allows you to connect to many VPCs. Up to 5,000 Vpcs can communicate to each other over a transit gateway. We can also use VPN from an on-premises to a transit gateway. We can use a direct connect gateway.  Use AWS Direct Connect gateway to connect your VPCs. Associate an AWS Direct Connect gateway with either of the following gateways:

- A transit gateway when you have multiple VPCs in the same Region
- A virtual private gateway

The diagram illustrates how the Direct Connect gateway enables you to create a single connection to your Direct Connect connection that all of your VPCs can use.

![Direct Connect Gateway](https://docs.aws.amazon.com/directconnect/latest/UserGuide/images/direct-connect-tgw.png)

Attach a transit gateway to a Direct Connect gateway using a transit virtual interface. This configuration offers the following benefits. You can:

- Manage a single connection for multiple VPCs or VPNs that are in the same Region.
- Advertise prefixes from on-premises to AWS and from AWS to on-premises.

![VPC](https://docs.aws.amazon.com/vpc/latest/peering/images/three-vpcs-peered-diagram.png)

We can peer our VPC with other VPCs directly in a point-to-point fashion either within a region or across to another region. 

A carrier gateway serves two purposes. It allows inbound traffic from a carrier network in a specific location, and it allows outbound traffic to the carrier network and the internet. There is no inbound connection configuration from the internet to a Wavelength Zone through the carrier gateway.

A carrier gateway supports IPv4 traffic.

Carrier gateways are only available for VPCs that contain subnets in a Wavelength Zone. The carrier gateway provides connectivity between your Wavelength Zone and the telecommunication carrier, and devices on the telecommunication carrier network. The carrier gateway performs NAT of the Wavelength instances' IP addresses to the Carrier IP addresses from a pool that is assigned to the network border group. The carrier gateway NAT function is similar to how an internet gateway functions in a Region.

The following diagram demonstrates how you can create a subnet that uses resources in a telecommunication carrier network at a specific location. You create a VPC in the Region. For resources that need to be within the telecommunication carrier network, you opt in to the Wavelength Zone, and then create resources in the Wavelength Zone. AWS Wavelength enables developers to build applications that deliver ultra-low latencies to mobile devices and end users.

![Carrier Gateway](https://docs.aws.amazon.com/wavelength/latest/developerguide/images/aws-wz.png)

A VPC endpoint enables private connections between your VPC and supported AWS services and VPC endpoint services powered by AWS PrivateLink. A VPC endpoint does not require an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.



![VPC](https://raw.githubusercontent.com/ealtili/Blog/master/AWS/vpcdeepdive/VPC.png)



Private link is going to enable you to configure SaaS services and that sort of thing inside of EPC that we

call a service provider V PC or you can

connect to a bunch of services by a

private link and so any other services

outside of s3 and dynamodb when you can

communicate to those privately within

the V PC it's generally using a private

link enabled service or an interface

endpoint

for me to data of traffic going in and

out of EPC we can use VPC flow logs and

then lastly we've got global accelerator

up here on the left hand side and global

accelerator is like a and any car

service it's much much more than that

but it basically allows you to front

things in the V PC at our edge locations

using global accelerator a global

accelerator now what you see here is

really what is a combination of a whole

bunch of different networking components

but customers are basically building

this architecture that you see in front

of you or a combination of these

services depending on what they want to

build some customers use Direct Connect

some use VPN some use IG W's and some

use flow logs some don't so it really

does depend but this gives you a bit of

an idea of water and Amazon V PC

architecture looks like so what we're

actually going to do now is take this

architecture and then take a step

forward in time and talk about all of

the new features that that have been

released over the last 18 - to 12 months

so firstly what's changed

all right let's start with into

BPC endpoints

so an interface VPC endpoint is where

we've basically got a whole bunch of

different services that exist in the

public realm so I'll take one for

example maybe the AWS ec2 API for the

longest time the AWS ec2 API was

actually resided within the public realm

so one of you wanted to do an API call

from him ec2 instance within the V PC

you'd have to go over an Internet

gateway um out into the to the public

realm now

interface end points or endpoints that

are powered by private and Lincoln they

were you to have these services dropped

into a V PC privately so each of these

services can have an interface endpoint

and we just drop a set of end points

into that V PC and then we can reach

those endpoints natively in the V PC so

what basically happens is we have a

whole bunch of elastic network

interfaces that we create and front

these services with inside the V PC and

each of these lasting network interfaces

will take an address from the V PC range

so you are noticed here on the left week

we actually don't need any routes to

each of these services you can reach

them natively via these elastic network

interfaces so previously we had about 18

services that were supported over over

private link which is quite a lot but in

the last 12 to 18 months we have

actually been created easy with be PC

endpoints and we now have over 52

services that you can drop into a V PC

privately using private link and so this

is actually awesome you've seen this

kind of shift away from all of these

public services and having to have an

Internet gateway to privately connecting

to all of these services from within

your V PC using private link enabled

endpoints

we also released endpoint policies as

well so you can add a little bit of

policy control around the endpoints and

and who can talk to services and that

sort of thing

okay so we may also made some updates to

V PC endpoint services so a little bit

different from interface V PC endpoints

so V PC endpoint services is where you

can use private link and take a SAS V PC

here on the right hand side or what we

call a service provider V PC so you

might have like a SAS application

running in that V PC and then you can

use private link or a V PC endpoint

service to connect to a network load

balancer and your SAS application for

example is the target of the NLB so

traffic flow goes from the customer V PC

over the private link enabled endpoint

through to a network load balancer and

through to your service so it's a pretty

easy way to drop this service into

multiple V pcs if you wanted to

privately

so we now have tagging for endpoint

services seems like a no-brainer so

pretty happy that one that one's there

endpoint services are also reachable

over intra and Inter region peering as

well now so I'm we've got a V PC and

that V PC is paired to your customer V

PC that has the endpoint in in it you

can actually reach from that V PC all

the way through to the end point and all

the way through to your service provider

V PC now this is actually applicable for

the other interface endpoints that we

had for all of those other services so

you could effectively had a bit have a V

PC that has all these endpoints that

exists within the V PC and then connect

through over peering or transit gateways

or maybe even over Direct Connect or

something similar and reach out to any

of those services pretty cool

okay let's move on to global accelerator

so if we look at global accelerator

basically we've got your Amazon edge

locations and global accelerator operate

at the edge other that's where it where

it's it so we've got an anycast IP

address here you think about it like any

caste but it's a lot more than that

though really we've got a three 10.3 dot

125 and we've got our users that come in

and access that anycast IP

global accelerator is actually going to

direct you down to your elastic IPS of

your service in this case it could be

network load balancer that's fronting

some applications inside that region and

so these users when they use that single

IP to access the service will actually

get routed to the closest region so now

with global accelerator we've got a

whole bunch of additional application

endpoints in additional regions so you

can use global accelerator to connect to

additional regions

we've got source IP preservation of the

client IP so previously when you

connected from your clients down through

to your application the application

couldn't see what the source IP was now

we have source IP preservation and we

now have support for ec2 endpoints where

we can basically drop global accelerator

into a vbc privately using an interface

endpoint allowing access privately from

global accelerator down into your V PC

okay let's talk about either your

site-to-site VPN so site-to-site VPN is

basically where we've got a virtual

private gateway or a vgw

which is attached to a VP see we've got

our own premises or what we call the CGW

which is basically a another term for a

CPA customer premise equipment or a

router that sits at the on-premises site

we've then got the public Internet and

we can connect via IPSec tunnels through

to the vgw and this is what we call

site-to-site VPN for an amazon VPC

so now we support certificate

authentication for site-to-site VPN so

previously you would have a pre-shaped

key which you would have to share with

your cgw when you're configuring the CGW

for the IPSec tunnel to come up or to be

established but now we can actually use

a double your certification manager we

create a certificate we can figure the

CGW and then we use the certificate on

the CGW

to connect through to 3gw or establish

the IPSec tunnel so in this case we're

creating a customer gateway and we've

got this option now to select the

certificate AR n that we've created

using a degree a certificate manager we

now support ike v2 which that's been a

long time coming so I'm pretty happy

with that one

and we now allow you to configure the

security algorithms and timer settings

of the IPSec connection which is not

something you could previously do so you

can see here there's a whole bunch of

options that we can do here we can

choose AES 128 or 256 sha Wan char to

256 etc etc

part of the transit gateway launch

folks actually build a tool to migrate

from site to site VPN to AWS transit

gateway where transit gateway actually

supports VPN termination or VPN

connectivity so this tool can actually

help with migrating from site to site

VPN if you started to use a transit

gateway okay let's talk about AWS client

VPN so client VPN is where we built a

client VPN service to allow you to have

a client on our laptop or similar which

can connect through this service and

then we can drop that connectivity

through to an Amazon V PC so we've got a

user here using an open VPN client we've

then got a TLS based tunnel over the

internet connected to the client VPN

service and then we attached that client

VPN service through to an Amazon V PC

and so what this also allows us to do is

connect from your client that's running

the open VPN client through the the

client VPN service into the V piece and

down through to other V pcs the internet

over things like an IG W to s3 or

dynamodb or others over a vgw back to an

on-premises if you wanted to and so the

client VPN is really used as a jump

point or an access point into a V PC

then that then could be used to access

other things in AWS or elsewhere as well

so some of the updates for client

can we

our support CloudFormation for client

VPN that one's definitely a no-brainer

so I'm pretty happy that that one's

there now

we now support split tunneling so not

all traffic is going to go over the

client VPN service to AWS you can split

other traffic out to other destinations

as well so maybe traffic that's not

destined towards AWS you can send it via

the split tunnel and we also support

multi-factor authentication for Active

Directory now and we have a new desktop

client for client VPN which is quite

recent

all right let's talk about a two beers

transit gateway one of my favorite

services and we're going to throw in

some Direct Connect and some Amazon VPC

in here as well so transit gateway what

is it

well transit gateway is really a logical

route identity which allows us to

connect to many VPC so in this case

we've got three V pcs V PC 1 V PC 2 and

V PC n now n could be up to 5,000 V PC

so we can have up to 5,000 V pcs

attached to a transit gateway we can

also then take our on-premises and

connect via VPN to the transit gateway

and in a lot of cases which is

interesting we're actually seeing the

transit gateway be used as a

connectivity point for transit VPC so we

actually built transit gateway to help

with customers that were building

transit VPC so they wouldn't have to

build transit V pcs anymore but what we

actually saw was folks that wanted to

deploy some firewalls in line or maybe a

wife or something similar inside of EPC

I've actually started using transit

gateway with IPSec

with BGP and ecmp or equal cost multi

path across multiple appliances within a

transit VPC so ec and peter was

definitely the big game there where you

can now actually Auto scale a transit V

PC edge and use ecmp to load balanced

across multiple VPN connections across

multiple instances in a transit V PC

pretty awesome stuff and so some of the

updates to transit gateway we now

support Direct Connect connectivity so

in this case we've got a nativist

transit gateway that's connected to a

direct connect gateway and then we take

care on premises and we use something

like a Direct Connect connection and we

use what's called a transit virtual

interface or a transit Biff now the

transit VF is something that each Direct

Connect whether it's if it's greater

than 500 Meg so or one gig a 2 Giga 10

gig etc gets a single transit virtual

interface so where where as you might

get 50 virtual interfaces either public

or private you can now have a single

transit virtual interface which can

connect through to a direct connect

gateway and connect to your

Adria's transit gateway

all right so let's take a quick detour

on AWS Direct Connect Now if transit

gateway is one of my most favorite

services over here at AWS I would have

to say Direct Connect is my second

favorite and don't tell the direct

connect pro team I said that though so

with Direct Connect and we're just going

to do a really quick overview here we've

got our on-premises on the right hand

side which we might have a 192 168 0 0

slash 16 as our on-premises IP range

we've then got the AWS region on the

left hand side and we've got our public

services that we had before so s3 and

dynamo and lambda and others and then

we've got our V PC in the AWS region and

the idea here is with Direct Connect we

want to actually bridge your on-premises

to your V PC privately

so in this case we

service provider network which a lot of

companies enterprises etc would use

something like an MPLS IP VPN or a DWDM

or a VP LS and basically use that LAN to

connect all of their sites together so

in this case we're using the when or the

service provider network to connect from

the on-premises to an AWS Direct Connect

location so a Direct Connect location is

where we've deployed points of presence

i'mso routers at a physical location

that you can plug a fiber into so we've

got an AWS cage which is in the Direct

Connect location we've got our routers

inside the AWS cage

we've then got either a customer device

that you deploy so something that can be

used to terminate the Direct Connect

from AWS or you could use a partner and

we've got a whole bunch of partners last

time I counted it was in excess of 150

different partners globally and this is

where partners have deployed equipment

at our colocation facilities so that you

can plug into that partner device

without having to deploy a router

yourself so we've got the where network

connected to our customer or partner

device then we build a cross connect and

the cross connect is essentially just a

physical fibre it's a fiber that goes

from your AWS cage to your customer

device or partner device and then once

we've got that cross connect and a cross

connect is essentially connected to a

Direct Connect port and Direct Connect

in itself the service itself is really

just a port on a router that you can

then create VLANs on so virtual

interfaces so if we think about the

private connectivity piece we build

what's called a private virtual

interface from the vgw and it's the same

vgw we were using for VP and termination

before we now connect a VLAN or a

private v to that vgw

on the left-hand side and then on the

right-hand side it's just a VLAN or sub

interface on your customer or partner

device

now if you wanted to connect natively to

some of the public services we can build

what's called a public virtual interface

so public virtual interface is basically

going to give us access to s3 and dynamo

and those services public and our public

virtual interface is a little bit

special in that it advertises all of the

public region routes for every region in

AWS globally so you'll get about last

time I counted about 1,900 BGP prefixes

advertised over a Direct Connect public

virtual interface and we actually do use

BGP as the routing mechanism to

advertise routes from AWS to your

on-premises via the virtual interfaces

okay so the cross connects all the

Direct Connect service also allows you

to create multiple virtual interfaces so

we can have up to 50 virtual interfaces

on a direct connect and in this case

we've actually got one public v and

we've got two virtual interface private

virtual interfaces now so we're

connecting to two different V pcs so we

can have up to 50 private or public

virtual interfaces and what we generally

sees customers having one or two public

virtual interfaces and then many other

up to you know 48 49 private virtual

interface is connected to 48 or 49 v pcs

now each one of these private virtual

interfaces actually has a bgp session

back to the customer or partner device

so what we actually realized was

customers were building out a whole

bunch of epcs and every time they built

a VPC they had to then go on to their

customer device on the partner would

have to go into the partner device and

configure a new virtual interface and

new bgp session etc so we built this

service called Direct Connect gateway

and what Direct Connect gateway allows

us to do is basically take a private

virtual interface connect that to the

direct connect gateway instead of the VG

W's and then the Direct Connect gateway

can attach directly to VG w's natively

so in this case we've just got one

private virtual interface and we can

have up to 10 v pcs on the left hand

side here connected to that Direct

Connect gateway so it's giving you a

little bit more scale beyond the 50

private virtual interfaces you can now

get up to about 500 V PC is connected

through one Direct Connect connection

now one of the important features around

Direct Connect a where was actually the

ability to have direct connect gateway

connect across multiple regions so

Direct Connect gateways one of those

unique global services so here we've got

a private virtual interface the Direct

Connect is home to two region one we're

using Direct Connect gateway and we can

also attach be pcs in other regions so

um you could be connected in say our San

Francisco region but then have another V

PC in our Dublin region connected to

that same Direct Connect gateway and the

communication between the direct connect

gateway and the vgw in the other region

is actually going to go via the AWS

backbone so we've got two separate

regions here

okay so for the longest time we actually

couldn't have

VP sees connected to the same direct

connect gateway in different accounts so

up until late last year that was the

case but then we launched multi-account

DX gateway so we can actually take the

DX gateway and we can drop that or

connect it to multiple VP C's in

different regions but those VP C's could

be in separate accounts as well

now also up until late last year our

partner connection speed so this is when

you're using a partner device at the

colocation facility and we could only go

from 50 mega up to 500 Meg now after we

launch this feature you can now go up to

ten gig of partner connectivity so one

gig to Gig five gig or ten gigs of

capacity when using an Amazon partner

our customers are actually really happy

about this because they can use a part

in a four or five gig connection for

example and not have to deploy those

devices at the colocation facility that

can just use a partner's set of devices

there

okay we touched on this a little bit

earlier when we're talking about transit

VPC

we did late last year released the

ability to have a transit gateway

connect to a direct connect so when

transit gateway first launched in

November 2018 we had VPN capability to

connect your own premises

and that was great but a lot of

customers do use Direct Connect so in

this case what we can actually do now

we've released this new type of vith and

for every physical Direct Connect you

can have an additional lift so we could

do 50 public or private vests before we

can now do 51 vs and one of those

virtual interfaces can be a transit

virtual interface and the transit RIF

actually attaches to a direct connect

gateway and so from the direct connect

gateway we can then attach to a transit

gateway which can then attach to up to

5,000 V PC so now instead of having 10 V

pcs connected to a direct connect

gateway you could have 5,000 V pcs

connected to a direct connect gateway

through the transit gateway so pretty

amazing stuff that the scale here that

we've built out for Direct Connect and

transit v is just an immensely huge and

so it gets a little bit better than that

as well in that you can actually have up

to three transit gateways connected to

the same Direct Connect gateway so in

this case we could in theory have up to

15,000 V pcs connected through a single

Direct Connect connection now again the

Direct Connect connection is a physical

means that cross connect is a physical

fiber and when you start working at that

kind of scale you probably want to have

multiple Direct Connect connections and

have some form of redundancy across

those so that if you lose that Direct

Connect connection you don't lose all

that access from your own premises all

the way through to the the 15,000 V B

C's in this case which is you know a

huge amount of scale now one of the key

pieces here and the same with just

direct connect gateway we've actually

decoupled all of these v pcs from the

Direct Connect itself so when you can

you can now spin-up and spin-down these

V pcs as you like and you don't have to

go and reconfigure the customer devices

here as you did with the original

private virtual interface architecture

okay so that was Direct Connect now I

want to talk about Amazon V PC route

limits and and this was actually

something I was really passionate about

for a couple of years here the default

limit within a V PC for routes in a

route table was actually 50 for the

longest time and the hard limit there's

actually a hundred routes and to me when

I'm thinking about you know transit V

pcs and the transit transit gateways and

transit gateway can do up to 10,000

routes and you know you might have a

whole bunch of scale that you're

building with connectivity to a V PC 100

routes inside a rat tail is actually

quite small

so late last year we actually released

the ability to do 1,000 routes in a

route table for a V PC so this is

actually pretty awesome now a thousand

might not sound like much versus the

transit gateways 10,000 but when you

think about a single V PC and a thousand

routes you probably don't want to go

much further than that just because of

the complexity - to manage all of these

routes but a thousand is definitely much

much better than a hundred routes that

we had previously and this is available

on request

so not something that we will just up

your limits to natively you can open a

trouble ticket and we can go in there

and increase your VPC route limits if

you needed more than the 50 default or

100 hard limit default we can go to a

thousand now

and that one's been something I've been

looking forward to for a long time had a

lot of customers that wanted to go

beyond that hundred route limiting of

EPC

okay let's talk about ingress routing

this is a new one late last year and

this is a it's it's a little bit

different on how we're doing routing

within the VPC because when you look at

a V PC the route tables were really just

for routing of traffic as it pertains to

inside the V PC

so before ingress routing we actually

have multiple instances here and we've

got our route tables with all of our

destinations for each of these route

tables for each of the subnets here so

again always a good idea to create

multiple route tables one per subnet and

so when a when traffic comes in via an

internet gateway what's going to happen

is and let's just say our destination

here is instant B or may be instant a

we're actually going to follow the local

route of the VPC so that's that magic

10.10 0/6 teen and this route is

actually created by Amazon when we

create a route table and it's equivalent

to the V PC range that we configured

when we created the V PC so everything's

just going to get routed naturally

within the V PC

so what we can actually do with ingress

routing now is we've released the

ability to or we've enabled the ability

to have gateway route tables now a

gateway route table is where you've got

a route table associated with not a

subnet but an Internet gateway or a

virtual private gateway so in this case

we've got traffic that's destined

destination is B so our instance B that

we had earlier but instead we're going

to say as traffic comes in an Internet

gateway let's direct that traffic over

to a firewall which is actually instance

a so we're doing some routing instead of

just following that that local route in

the VPC wrap table we're actually doing

a little bit of ingress routing at the

internet gateway so instance a can then

forward traffic onto instance B as

needed now if we look at the Virtual

Private Gateway we've also got a

destination of B traffic's coming into

the vgw and we're sending that to

firewall a and firewall a's then sending

traffic onto instance B so

ingress routing really gives you the

ability to have something like a

firewall or a way for some kind of

appliance inline from the outside

inbound to your V PC you could always do

this outbound so you could say okay

instant be going to the internet please

go via an ad instance or something

similar but now we have the ability to

do this inbound on an Internet gateway

and a virtual private gateway pretty

cool stuff

alright that was a little bit of a shift

we've talked about some direct connect

stuff and remember my second favorite

service in AWS and we also talked about

some ingress routing stuff and and our

route table limits let's go back to AWS

transit gateway now

so with AWS transit gateway we also

released accelerated site-to-site VPN

and this was a new one late last year at

reinvent as well

got a transit gateway we've got our VP

C's attached to the transit gateway and

again we can have up to 5,000 VP C's

attached to the transit gateway and then

we've got our on-premises we've got our

VPN connections from our on-premises to

the transit gateway and these VPN

connections actually traverse the public

Internet

so the transit gateway will have a

couple of IP SiC endpoints that are

advertised at the internet and you'll

connect to those using IPSec this is

basically how VPN connectivity works to

a transit gateway

now with accelerated site-to-site VPN

we're taking that same architecture or

actually

adding global accelerator into the mix

so that we can use our edge locations

and so our two IPS that we would connect

to for our VPN connectivity previously

directly on the transit gateways those

IPs are actually advertised like anycast

IPS out of our edge locations and now

when you connect from your own premises

you're actually going to an edge

location that's close to you and so what

happens then is we're using the AWS

global network from the edge location to

the transit gateway and so we capacity

manage our backbone and a whole bunch of

other good stuff as opposed to you just

going over the native Internet so in

this case we're combining AWS global

accelerator with VPN connectivity to a

transit gateway and that's going to give

you maybe some lower latency some

reduced juda and a more consistent

connection because you are going over

that capacity managed piece of the AWS

backbone between the edge location and

the transit gateway and this is really

ideal for if you've got a lot of sites

globally and you want to connect back

through to a transit gate where you can

take advantage of the AWS backbone here

okay another new service here we had a

whole bunch of stuff that we actually

released towards the end of last year

around transit gateway and transit

gateways being an immensely popular

service for us we've got a whole bunch

of customers using it it is amazing and

we've been you can see that by the the

new services that we've been adding on

to transit gateway so now we've got AWS

transit gateway network manager

so when I talk to customers one of the

complaints around networking that I've

heard quite a lot is hey I really want

to get visibility into traffic going

over my network I'd like to get

visibility into how many connections

I've got both Direct Connect and VPN and

what the status is and the visibility

piece traffic mirroring which we'll talk

about I've got it some traffic mirroring

slides coming up shortly and also vbc

flow logs I'm give you visibility in the

traffic going over then your network but

you didn't really have a good mechanism

to have to view all of your connectivity

components when it comes to your

connectivity architecture so a devious

transit gateway Network manager is going

to give you the ability to manage and

monitor your global network that

basically constitutes all of your

connectivity to AWS now we've also

integrated with a whole bunch of SD when

partners and their branch appliances

we'll talk a little bit about that but

the whole idea for transit gateway

network manager is to give you

visibility across AWS and the whole

branch network that's connected back to

AWS

okay so we've got a really quick screen

here we've got our global network on on

the the top left hand side here and so

you can see that made up in this case of

five transit gateways we've got sixteen

sites we've got fifty devices and that's

just going to be a really quick summary

of of what your global network inventory

is then we've got some VPN status to our

transit gateways so um you get some

statistics there on on the state on the

status of your VPN connections and then

we've got our network events summary so

this is when there's been some topology

changes or status updates or routing

updates around our our connectivity to

AWS

and so what I really like about our

network manager is this visualization

here so we've got our global network

topology oops

we've got a global network topology and

it's going to give you an overview of

all of your transit gateways or your VP

sees all of the connectivity DX and VPN

etc that's going to connect through to

your AWS deployment so you get a bit of

an overview here on what everything

looks like pretty awesome stuff

now SD Wang has been one of those

services that a lot of our customers

have used to connect their branches back

to EWS and we did also work pretty

extensively with folks like Cisco silver

peak Aruba aviatrix when we were

building network manager to integrate

their stuff around SD when into the

transit gateway network manager as well

so definitely something that's worth

checking out

alright so that was Network manager now

I've got two last features for transit

gateway and one of those is multicast so

we now can do multicast within a V PC

using transit gateway and I've got a

quick link here if you want to learn

more about how multicast works with

transit gateway definitely worth

checking out if you're interested in

multicast but also into region peering

the inter region peering was one of

those things that customers really

wanted to take care take advantage of

our AWS backbone and you might have

multiple transit gateways in multiple

different regions and inter region

peering really helps with that so if we

dive into inter region peering or cross

region transit gateway peering we've got

two Transit gateways they're sitting in

two different regions and essentially

this is just static peering between the

regions and between the transit gateways

so we've got a transit gateway here on

the left that could be part of a region

and we've got another transit gate we're

here on the right that could be part of

another region and we're just going to

configure static peering between those

two transit gateways now this is a new

attachment type and we're also using Inc

the same encryption that we use for V PC

peering between the regions but it's

basically going to be a mechanism to

connect transit gateways together now a

quick point here we don't actually

support transit gateway peering within

the same region now there might be a

reason to do that but really this

feature is geared towards connecting

transit gateways from one region to

another now this is just static

point-to-point peering

can actually go a lot further than that

with transit gateway peering so in this

case if you've got many regions with

transit gateways in each region we can

actually connect all of these transit

gateways together in a full mesh

topology and now you've basically built

a global backbone connecting all of your

transit gateways on top of the AWS

backbone this is really really cool

now one of the things you do need to

consider here in that when you're

connecting transit gateways together

we're basically using peering as a

static mechanism to connect transit

gateways together so you basically

determine how the routing works and that

sort of thing but we do also ask that

you configure unique AAS ends or

autonomous system numbers for each

transit gateway when you're creating the

transit gateway so they do need to be

unique and and we're actually asking for

that because we might do something with

dynamic routing and that sort of thing

with transit gateway peering in the

future but this mechanism is is actually

really amazing that you can build your

own backbone between all of your regions

and it's all private connectivity from

one transit gateway to another and each

region could have or each transit

gateway in each region could have up to

5,000 V pcs and you can just peer them

across to every other region here pretty

awesome stuff

okay let's take a dive into VPC traffic

mirroring so we've talked about transit

gateway a whole bunch and you remember

one of the questions customers have been

asking us is how do I get visibility

into my traffic inside of EPC now we've

got vbc flow logs but flow logs are

really just metadata for a particular

time period what customers have been

asking for is the actual packets the

actual traffic going to their instances

and traffic mirroring brings this to the

party and we release traffic mirroring

at reinforce last year it was a big

fanfare as part of the the launch so I'm

a little bit excited and biased about

this one

so here we've got an instance and we're

receiving traffic and sending traffic

over an Internet gateway

and so what we can do now is create a

traffic mirroring session

and then what will happen is the packets

from and or to and from that instance

now get mirrored over to a monitoring

instance or to another Eni so now we're

getting a copy or a duplicate of those

packets that are going in and out of

that ec2 instance so in this case we're

sending everything inbound and outbound

when we think about traffic mirroring

there's three different components

there's the target which is really the

destination for the mirrored traffic

there's the filter so you don't have to

receive all the traffic and we'll talk

about how you can filter that down and

then there's the session we can have

multiple sessions per ec2 instance to

send traffic to different destinations

based on a few different attributes

so let's start with targets

so vbc traffic traffic mirroring can be

sent to either an elastic network

interface or a network load balancer so

if we wanted to just send traffic to a

single monitoring instance running say Z

Coursera card or some open source IDs

tools we can do that or if you wanted to

build some scale into the system here we

can actually put a network load balancer

in front of our monitoring instances and

we'll talk more about what that looks

like

so you can also set us on a number of

rules that define the traffic that's to

be copied to the mirrored session so in

this case we've got traffic coming in

and out of the ec2 instance we've set a

filter for traffic mirroring to say

we're only interested in inbound packets

so inbound packets are going to be sent

from that ec2 instance to the monitoring

instance and we're actually not going to

mirror the outbound packets

so when we look at filtering there's a

couple of different options here like

flow direction protocol source IP source

port destination IP destination port and

one of the more important ones here is

truncation so we can take a sampled

truncated packet instead of the full

thing

so when we tie all this back together to

a particular instance we actually build

what's called a session so a session is

going to be a relationship between the

traffic mirroring source and the traffic

mirroring target and associated with the

appropriate filter so there's three

components the source

of the of the packets so what instance

we're mirroring traffic from the target

where we're sending it to and then the

filter how much traffic would we like do

we want to truncate at what source or

destination port etc etc

and so each traffic mirroring source can

support up to three sessions now this

gets a little bit interesting where you

can basically set a priority here with

your session so the priority with the

other session with the lowest priority

will have the hopper with the lowest ID

will have the highest priority and a

packet only gets mirrored once but

basically you can do a little bit of

rule matching here on where you actually

want particular packets to go to so what

may be Zeke and Sarkar the instance for

example you could have multiples of

those depending on the source and

destination

okay so let's talk a little bit more

about scale we track with traffic

mirroring so in this case we've got a

whole bunch of traffic going to an ec2

instance we enable traffic mirroring and

we're sending traffic in this case to a

network load balancer

and so we've got multiple monitoring

instances in an auto scaling group now

that are receiving the packets from the

network load balancer so the right

sizing we talked about before you can do

a little bit more scale than just having

a single instance and right sizing that

you can actually have a fleet of

instances as monitoring instances

receiving traffic from many instances on

the left hand side here and so we could

set an auto scaling rule to say hey

based upon certain metrics let's scale

out this fleet of monitoring instances

as well just like you would with a

normal knit work load balancer and auto

scaling

so what do you actually do with the

packets once what you receive them so

here we've got a monitoring instance and

we actually partnered with a whole bunch

of APN partners that have traffic

inspection applications and the like you

can deploy those as monitoring instances

as the source or the receiver of the

traffic mirroring packets you could also

build your own network analyzer so you

could use open source tools we have seen

customers say hey I actually want to

take the packet and do something

specific with that and write an

application to do things with that or

you could use Zeke and sericata to

deploy open source IDs and IPS tools on

your monitoring instances as well

we've actually got a link here for

working with some open source tools so

just how you could do that and you seek

and sericata etc to do that

okay let's dive into a quick Amazon VPC

traffic mirroring news case and this is

actually kind of cool it's a bit of a

Rube Goldberg machine but pretty awesome

stuff too so we've got an instance here

that it's receiving traffic and there

might be bad traffic coming into the

instance now it's up to you to define

what bad traffic is

but Amazon guard duty might fire an

alert based upon the traffic that's seen

to this instance and so it'll fire an

alert to Amazon CloudWatch events which

will trigger a lambda event so it'll

match the alert and then identify what

the instance and eni is that needs

monitoring and then lambda can go and

spin up a monitoring ami or monitoring

instance and enable VPC traffic

mirroring so now traffic is going to get

forwarded to the monitoring instance

based upon an alert that was fired from

Amazon guard duty

so then we could use Zika sericata or

something similar to create logs of the

traffic and maybe ship those logs off to

s3 now we could then at a later stage

come through with our amazon athena and

inspect the logs and and do something

with that traffic now let's just say the

bad traffic goes away or maybe we've

applied some filters or something

similar

you can then have Amazon or EWS lambda

spin down the monitoring instance and

turn off traffic mirroring and

everything goes back to normal and this

could all be automated which is pretty

amazing

so a couple of quick considerations with

traffic mirroring

traffic mirror traffic isn't subject to

the outbound security group so when you

apply a security group to an instance it

doesn't actually apply to the mirror

traffic

and also flow logs won't capture

mirrored traffic mirror traffic is

considered outside of that normal flow

in and out of the V PC it's part of the

traffic mirroring service so you won't

see that in flow logs

and packets will only get mirrored if

they pass the inbound security group or

ACL so if the packet doesn't make it to

the instance we're not going to mirror a

packet

okay so that was a BBC traffic mirroring

pretty amazing stuff and no one's

actually been a long time coming I've

been looking forward to traffic

mirroring since I first joined a Amazon

all those years ago pretty awesome we

have that now all right so we've got a

little bit of a divergence here we've

got dumb

AWS outpost somewhere specifically I'm

diving on the aid of this outpost

networking piece

our post is really where we're taking

the technology in and aid abuse region

and we're allowing you to then have that

within your own premises in the form of

Iraq or multiple racks is pretty awesome

stuff

so we're basically bringing the

technology that we built inside an

Amazon region and we're allowing you to

have that within your on-premises data

center so we're using the same AWS

designed infrastructure think servers

switches etc all in Iraq that operates

on premises now it is a fully managed

service so we do need always-on

connectivity back to the region and

that's that's an important thing we'll

talk about but it does take away all of

the management of the underlying

hardware that you would normally do in

your data center now because it is a

fully managed service with an extending

and amazon VPC to that service and you

get this single pane of management

through the aid of your CLI or through

the editors console to manage your VP

see that now resides inside the outpost

as well awesome

so talk about AWS outposts networking

and specifically what the networking

components look like

okay so we've got our AWS region here

just like we did before in some of the

earlier slides and we've got our Amazon

VPC with our vbc site arranged and our

instances ABCD are different subnets

that are in different availability zones

and a subnet is an availability zone

construct and this is prior to our post

and so what we do with outpost now is

we're actually extending that VPC all

the way through to the outposts which is

operating on premises and so now we can

create outpost subnets and an outpost

subnet resides or exists within an

outpost but it's in the same V PC that

you were using in the region so now we

can deploy some ec2 instances in our

output subnets that we've just created

and we can use route tables security

groups and Network ACLs the same

networking constructs that you would

have used in the region with your

instances inside your availability zones

you can use those inside the outpost as

well so we've just created or extended

that same capability to the AWS outpost

and so when we think about things from a

traffic flow perspective we've got say

instance Y or instance said

communicating with each other within the

outpost and that seems that to make

Santa mean that's it's the logical

conclusion here instances talking to

each other in the same VP see inside the

outpost communicate natively indirectly

but we can also have something like

instance Y communicating with other

instances inside the region so instance

Y could be talking with instance B or

instance D and so if we have a look at

the route table real quick here we've

again got that 10100 slash 16 local

route in the outpost just like we did in

the V PC in the region so that route is

going to enable everything inside the V

PC to communicate with each other and so

we can actually take it one step further

where we've got the internet out through

the region or an Internet gateway we've

got our public services again your s3z

Dinamo's your lambdas and then we can

build a default route from the outpost

through to an IG w or we could use V PC

endpoints transit gateway V PC peering a

V G W and we basically just build out

our routes within the V PC just like we

would with all of these constructs in

the region now the only difference here

is something like a transit gateway is

an immensely scalable service which we

touched on earlier and the underlying

infrastructure that supports transit

gateway is just massive now that

actually runs in the region so when

we're communicating from instance Y

inside an outpost we're going over the

service link or the underlay here but it

overlays actually the V PC and then we

go out the transit gateway in the region

so the transit gateway in this case is

of region level construct now really

quickly if we had two v pcs we could

then deploy two subnets intercept in

those two separate V pcs in the outpost

and we could actually use peering

locally in the outposts appearing is one

of those exceptions there but transit

gateway and VG WS + IG WS all operate

inside the region but we can communicate

with those now one of the main purposes

of having an outpost is to connect

through to your on-premises workloads

we've now developed this new gateway

called a low

call gateway or an L GW and that's going

to allow us to have a route to the LG W

and then communicate to on-premises

workloads or anywhere else now

one key point here is we use what's

called a customer owned IP range to

assign elastic IPS to instances in the

outpost at the LG W so if you're

familiar with the way the igw works and

we covered that briefly at the start

basically we assign elastic IPS at the

igw just like we do with the LG W and

there's a NAT function that happens

there from the instance out through to

your on-premises workload okay so let's

talk about when connectivity for the

outpost really quickly so we've got a

region on the Left we've got our VP see

we've got our customer premises we've

got our outpost our help post networking

devices which basically like top-of-rack

switches but they're a little bit more

than that because they do BGP and LACP

and a few other things and they

communicate via this blue segment to

your local network which we just

reference from the LG W

or anywhere else

this service linked infrastructure range

which you'll also assign to an outpost

and that subnet or that VLAN for the

service link needs connectivity back to

AWS so here we've got a customer edge

router we've got a weigh-in connection

we've got a Direct Connect and in this

case where we want public access back to

the AWS region so we're using cross

connect with a Direct Connect and we're

using a public virtual interface for

Direct Connect

and whenever we use a public virtual

interface we do need to use network

address translation to public IPS or you

know you could have this service link

range as a public range but it's

probably burning a few more IPs than

necessary because we need a slash 26

here or you could do port address

translation here on a single address as

the outpost will initiate that

communications out over the direct

connect and over the public virtual

interface to what we call the outpost

service or the outpost anchor that

operates in an availability zone that

you choose to home the outpost to

we could also use the internet because

the output service is fronted by Amazon

public IP so you don't actually need to

use direct connect to connect back to

the region when you're using an outpost

you can just use the public internet if

you wanted to as well and so this is

going to then allow us to have AWS

control plane traffic connected through

to the outpost and then also data plane

traffic so from the outpost service

we've got a data plane connection and

what's gonna happen here is traffic is

going to be sourced or destined to an

ec2 instance in the outpost

it'll traverse this infrastructure

subnet or the service link over the

anchor and then get dropped down into

the V PC in the region so this is how we

make the V PC look like it expands from

the region to the outpost

and so that's all the content I have few

folks today so I've thrown a couple of

references in here and we we talked

about updates for V PC endpoints the

things like access over V PC peering

we've got a whole bunch of new services

that support private link interface

endpoints global accelerator now has

source IP preservation multiple or

additional regions instance endpoints

into a V PC

we had a whole bunch of updates ooh site

to site VPN and client to site VPN so

pretty amazing transition there on the

new features that we can use

connected DX and V PC routing there's a

lot of additional features so we really

doubled down on on a whole bunch of

stuff that we can do here with Direct

Connect DX and V PC routing um the

thousand routes per V pcs amazing

transit v really allows you to connect

your own premises in a scalable way to a

whole bunch of e pcs in the region a

network manager a multicast cross region

peering couple of links here for your

folks as well

VPC traffic mirroring which has been a

long time coming I'm excited about that

one and AWS outposts which is a whole

shift in the way we do things here at

AWS and we're thinking about things like

distributed cloud and and really

extending AWS out to your on-premises

using AWS outposts and I'll actually try

to add these links into the offline

recording for this and see if we can

make them clickable but I hope I gave

you guys a bit of a chance to take some

screenshots there if you wanted to as

well

all right thanks very much for listening

folks hope you got this far and there's

a whole bunch of stuff that we covered

and I'm pretty excited to see what's

gonna happen in the next 12 months as

well is gonna be a whole bunch of new

features there too but I hope this gave

you folks a bit of a rundown on

everything that's changed and what some

of the VPC architectures would look like

with these new features thanks for

listening folks catch you next time
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUzOTc3ODU1OSwxMzk0MTA1NTg0LDYzMj
YzMzc3MCwxOTM2ODg2MDcxLC02NTA3MjkzLDc3MzEyODk5Nywx
MjI2NjI0NjE1LC0xOTI5NjY5MTIsODY3MjUyODUxLC04MDM5Mj
kwNDEsLTE3OTg2OTc2MzgsLTE4NjEyODEwMTAsLTExMzMwMDIz
NDZdfQ==
-->