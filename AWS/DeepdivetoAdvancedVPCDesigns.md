# Deepdive to VPC and Connection to VPC

Amazon VPC (Virtual Private Cloud ) enables to have complete control over AWS virtual networking environment.  How new Amazon VPC features might affect the way to design AWS networking infrastructure, or even change existing architectures? Let's explore the new design and capabilities of VPC and how to use them.

## Learning Objectives

-   Understanding of all networking services and when to use each
-   Understand the latest updates and how each is used
-   How to take existing architecture to the next level

![VPC](https://raw.githubusercontent.com/ealtili/Blog/master/AWS/vpcdeepdive/VPC.png)

Diagram above gives an idea about Amazon VPC Architecture. We will have a look at these components and carrier gateway which is not in the diagram.

**VPC** is a region level service which you assign a cidr address range. There are multiple availability zones, public subnets, private subnets. We can deploy ec2 instances inside these subnets in the availability zones of the VPC in the region.

AWS services has two different classes or types of services which are public services and private services. 

Anything inside the VPC is **private** and it's a domain that you manage and you control.  

Anything outside of the VPC is generally **public** unless you build a private connection to a service such as s3, DynamoDB, Lambda, SQS, SNS etc. all of these services are all public services.

![Internet VPC](https://docs.aws.amazon.com/vpc/latest/userguide/images/default-vpc-diagram.png)

When we're communicating inside the VPC we will actually use **route tables** that are assigned to subnets. The route tables basically give you control over everything that's going on inside the VPC. Where traffic can communicate.  Routing is the key on on how we communicate with everything else.

**Internet gateway** (IGW) gives us access to public services and also the public Internet. Just build a default route to the IGW. When you have a Public IP or Elastic IP assigned to an instance we can communicate with all of these public services and also the public internet. 

![NAT Instance](https://docs.aws.amazon.com/vpc/latest/userguide/images/nat-instance-diagram.png)

For a private subnet we can send traffic via **NAT instance** or **NAT Gateway** instead of directly to the public Internet.  Then the NAT instance or NAT Gateway (is a managed scalable service) has the ability to communicate with the public internet. 

![NAT Gateway](![](https://docs.aws.amazon.com/vpc/latest/userguide/images/nat-gateway-diagram.png)

A **VPC endpoints** enables private connections between your VPC and supported AWS services. VPC endpoint services powered by AWS PrivateLink. A VPC endpoint does not require an internet gateway, NAT device, VPN connection or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

![VPC Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-s3-diagram.png)

A **VPC gateway endpoint** is a gateway that you specify as a target for a route in your route table for traffic destined to a supported AWS service. We can connect to s3 and Dynamodb privately using VPC gateway endpoints at the moment. We' use prefix lists in the route tables to communicate privately. VPC gateway endpoint is like a bridge between s3, dynamodb and VPC. VPC Gateway endpoints are horizontally scaled, redundant, and highly available VPC components. They allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic.

![Interface Endpoints](https://raw.githubusercontent.com/ealtili/Blog/master/AWS/vpcdeepdive/interfaceendpoints.png)

An **interface endpoint** is an elastic network interface with a private IP address from the IP address range of your subnet that serves as an entry point for traffic destined to a supported service. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access services by using private IP addresses. AWS PrivateLink restricts all network traffic between your VPC and services to the Amazon network. You do not need an internet gateway, a NAT device, or a virtual private gateway. 

When you create an endpoint, you can attach an endpoint policy to it that controls access to the service to which you are connecting. A VPC endpoint policy is an IAM resource policy that you attach to an endpoint when you create or modify the endpoint. If you do not attach a policy when you create an endpoint, a default policy is attached for you that allows full access to the service. If a service does not support endpoint policies, the endpoint allows full access to the service. An endpoint policy does not override or replace IAM user policies or service-specific policies (such as S3 bucket policies). It is a separate policy for controlling access from the endpoint to the specified service. You cannot attach more than one policy to an endpoint. However, you can modify the policy at any time. If you do modify a policy, it can take a few minutes for the changes to take effect.

In the example shown in the following diagram, there is an interface endpoint for Amazon Kinesis Data Streams and an endpoint network interface in subnet 2. Private DNS for the interface endpoint is not enabled. Instances in either subnet can send requests to Amazon Kinesis Data Streams through the interface endpoint using an endpoint-specific DNS hostname. Instances in subnet 1 can communicate with Amazon Kinesis Data Streams over public IP address space in the AWS Region using its default DNS name.

![Interface Endpoint no Private DNS](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-kinesis-diagram.png)

In the next diagram, private DNS for the endpoint has been enabled. Instances in either subnet can send requests to Amazon Kinesis Data Streams through the interface endpoint using either the default DNS hostname or the endpoint-specific DNS hostname.

![Interface Endpoint Private DNS Enabled](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-kinesis-private-dns-diagram.png)

**Private link** is going to enable you to configure SaaS services. You can create your own application in your VPC and configure it as an AWS PrivateLink-powered service (Service Provider VPC).  Other AWS principals can create a connection from their VPC to your endpoint service using an interface VPC endpoint. You are the service provider, and the AWS principals that create connections to your service are service consumers. Tagging is available for endpoint services and they are reachable over intra and inter-Region VPC peering or transit gateways, Direct Connect.

![VPC Privatelink](https://docs.aws.amazon.com/vpc/latest/userguide/images/vpc-endpoint-service.png)

In the diagram above, the account owner of VPC B is a service provider, and has a service running on instances in subnet B. The owner of VPC B has a service endpoint (vpce-svc-1234) with an associated Network Load Balancer that points to the instances in subnet B as targets. Instances in subnet A of VPC A use an interface endpoint to access the services in subnet B.

We might want to connect to our own premises so we'll use Direct Connect or VPN.  

You can connect your Amazon VPC to remote networks and users using the following VPN connectivity options.

- **AWS Site-to-Site VPN**: You can create an IPsec VPN connection between your VPC and your remote network. On the AWS side of the Site-to-Site VPN connection, a virtual private gateway or transit gateway provides two VPN endpoints (tunnels) for automatic failover. You configure your customer gateway device on the remote side of the Site-to-Site VPN connection.

![site2sitevpn](https://github.com/ealtili/Blog/blob/master/AWS/vpcdeepdive/site2sitevpn.png?raw=true)

You can migrate Site to Site VPN to AWS transit gateway. 
https://aws.amazon.com/about-aws/whats-new/2019/04/migrate-your-aws-site-to-site-vpn-connections-from-a-virtual-private-gateway-to-an-aws-transit-gateway/
 
- **AWS Client VPN**: This is a managed client-based VPN service that enables you to securely access your AWS resources or your on-premises network. With AWS Client VPN, you configure an endpoint to which your users can connect to establish a secure TLS VPN session. This enables clients to access resources in AWS or an on-premises from any location using an OpenVPN-based VPN client.

![ClientVPN](https://github.com/ealtili/Blog/blob/master/AWS/vpcdeepdive/clientvpn.png?raw=true)

- **AWS VPN CloudHub**	If you have multiple AWS Site-to-Site VPN connections, you can provide secure communication between sites using the AWS VPN CloudHub. This enables your remote sites to communicate with each other, and not just with the VPC. The VPN CloudHub operates on a simple hub-and-spoke model that you can use with or without a VPC. This design is suitable if you have multiple branch offices and existing internet connections and would like to implement a convenient, potentially low-cost hub-and-spoke model for primary or backup connectivity between these remote offices.

The sites must not have overlapping IP ranges.

![AWS VPN Cloud HUB](https://docs.aws.amazon.com/vpn/latest/s2svpn/images/AWS_VPN_CloudHub-diagram.png)

- **Third party software VPN appliance** You can create a VPN connection to your remote network by using an Amazon EC2 instance in your VPC that's running a third party software VPN appliance. AWS does not provide or maintain third party software VPN appliances; however, you can choose from a range of products provided by partners and open source communities

AWS **Direct Connect** links your internal network to an AWS Direct Connect location over a standard Ethernet fiber-optic cable. One end of the cable is connected to your router, the other to an AWS Direct Connect router. With this connection, you can create virtual interfaces directly to public AWS services (for example, to Amazon S3) or private virtual interfaces to Amazon VPC, bypassing internet service providers in your network path. An AWS Direct Connect location provides access to AWS in the Region with which it is associated. 

![AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/images/direct_connect_overview.png)

AWS Direct Connect + VPN, you can combine AWS Direct Connect dedicated network connections with the Amazon VPC VPN. AWS Direct Connect public VIF established a dedicated network connection between your network to public AWS resources, such as an Amazon virtual private gateway IPsec endpoint. The following figure illustrates this option.

![AWS Direct Connect and VPN](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/images/image10.png)

By default direct connect traffic is not encrypted with this solution combines the benefits of the end-to-end secure IPSec connection with low latency and increased bandwidth of the AWS Direct Connect to provide a more consistent network experience than internet-based VPN connections. 

**Transit gateway** is a service that was launched in about November 2018. A transit gateway is a network transit hub that you can use to interconnect your virtual private clouds (VPC) and on-premises networks. A transit gateway acts as a Regional virtual router for traffic flowing between your virtual private clouds (VPC), VPN connections and on premises. 

With AWS Direct Connect + AWS Transit Gateway + VPN, using public VIF on AWS Direct Connect, enables end-to-end IPSec-encrypted connections between your networks and a regional centralized router which is Transit Gateway for Amazon VPCs over a private dedicated connection, as shown in the following figure

![AWS Direct Connect + AWS Transit Gateway + VPN](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/images/image11.png)

Consider taking this approach when you want to simplify management and minimize the cost of IPSec VPN connections to multiple Amazon VPCs in the same region, with the low latency and consistent network experience benefits of a private dedicated connection over an internet-based VPN

**Transit Gateway Network Manager** (Network Manager) enables you to centrally manage your networks that are built around transit gateways. You can visualize and monitor your global network across AWS Regions and on-premises locations.

You can create a global network that includes transit gateways in multiple AWS Regions. This enables you to monitor the global health of your AWS network. In the following diagram, the global network includes a transit gateway in the us-east-2 Region and a transit gateway in the us-west-2 Region. Each transit gateway has VPC and VPN attachments. You can use the Network Manager console to view and monitor both of the transit gateways and their attachments

![Transit Gateway Manager](https://docs.aws.amazon.com/vpc/latest/tgw/images/nm-multi-region-tgw.png)

**AWS Direct Connect gateway** is a grouping of virtual private gateways and private virtual interfaces that belong to the same AWS account. A Direct Connect gateway is a globally available resource. You can create the Direct Connect gateway in any Region and access it from all other Regions. 

AWS Direct Connect + AWS Transit Gateway, using transit VIF attachment to Direct Connect gateway, enables your network to connect up to three regional centralized routers over a private dedicated connection, as shown in the following diagram.

![](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/images/image9.png)

***A VPC peering** is a networking connection between two or more VPCs. This enables you to route traffic between them using private IPv4 addresses or IPv6 addresses. Instances in either VPC can communicate with each other as if they are within the same network. You can create a VPC peering connection between your own VPCs, or with a VPC in another AWS account. The VPCs can be in different regions (also known as an inter-region VPC peering connection).

![VPC](https://docs.aws.amazon.com/vpc/latest/peering/images/three-vpcs-peered-diagram.png)

You cannot create a VPC peering connection between VPCs with matching or overlapping IPv4 CIDR blocks. 

![overlapping ip](https://docs.aws.amazon.com/vpc/latest/peering/images/overlapping-cidrs-diagram.png)

You can't have transitive peering. You have a VPC peering connection between VPC A and VPC B (pcx-aaaabbbb), and between VPC A and VPC C (pcx-aaaacccc). There is no VPC peering connection between VPC B and VPC C. You cannot route packets directly from VPC B to VPC C through VPC A.

![](https://docs.aws.amazon.com/vpc/latest/peering/images/transitive-peering-diagram.png)

To route packets directly between VPC B and VPC C, you can create a separate VPC peering connection between them (provided they do not have overlapping CIDR blocks).

You cant have Edge to edge routing through a gateway or private connection. 
![](https://docs.aws.amazon.com/vpc/latest/peering/images/edge-to-edge-vpn-diagram.png)

For example, if VPC A and VPC B are peered, and VPC A has any of these connections, then instances in VPC B cannot use the connection to access resources on the other side of the connection. Similarly, resources on the other side of a connection cannot use the connection to access VPC B.

A **carrier gateway** serves two purposes. It allows inbound traffic from a carrier network in a specific location, and it allows outbound traffic to the carrier network and the internet. There is no inbound connection configuration from the internet to a Wavelength Zone through the carrier gateway. A carrier gateway supports IPv4 traffic.
![Carrier Gateway](https://docs.aws.amazon.com/wavelength/latest/developerguide/images/aws-wz.png)

Carrier gateways are only available for VPCs that contain subnets in a Wavelength Zone. The carrier gateway provides connectivity between your Wavelength Zone and the telecommunication carrier, and devices on the telecommunication carrier network. The carrier gateway performs NAT of the Wavelength instances' IP addresses to the Carrier IP addresses from a pool that is assigned to the network border group. The carrier gateway NAT function is similar to how an internet gateway functions in a Region. The diagram above demonstrates how you can create a subnet that uses resources in a telecommunication carrier network at a specific location. You create a VPC in the Region. For resources that need to be within the telecommunication carrier network, you opt in to the Wavelength Zone, and then create resources in the Wavelength Zone. AWS Wavelength enables developers to build applications that deliver ultra-low latencies to mobile devices and end users.

![VPC](https://raw.githubusercontent.com/ealtili/Blog/master/AWS/vpcdeepdive/VPC.png)

**VPC Flow Logs** is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC.  You can create a flow log for a VPC, subnet, or individual network interface.  You can Determine the direction of the traffic to and from the network interfaces. Flow log data is published to CloudWatch Logs or Amazon S3, and can help you Monitoring the traffic, diagnose overly restrictive or overly permissive security group and network ACL rules. Flow log data is collected outside of the path of your network traffic, and therefore does not affect network throughput or latency. You can create or delete flow logs without any risk of impact to network performance.

![VPC Flow log](https://docs.aws.amazon.com/vpc/latest/userguide/images/flow-logs-diagram.png)

In the example above, you create a flow log (fl-aaa) that captures accepted traffic for the network interface for instance A1 and publishes the flow log records to an Amazon S3 bucket. You create a second flow log that captures all traffic for subnet B and publishes the flow log records to Amazon CloudWatch Logs. The flow log (fl-bbb) captures traffic for all network interfaces in subnet B. There are no flow logs that capture traffic for instance A2's network interface.

**Traffic Mirroring** is an Amazon VPC feature that you can use to copy network traffic from an elastic network interface of Amazon EC2 instances.  You can then send the traffic to out-of-band security and monitoring appliances for:

- Content inspection
- Threat monitoring
- Troubleshooting

![Traffic Mirroring](https://docs.aws.amazon.com/vpc/latest/mirroring/images/traffic-mirroring.png)

The security and monitoring appliances can be deployed as individual instances, or as a fleet of instances behind a Network Load Balancer with a UDP listener. Traffic Mirroring supports filters and packet truncation, so that you only extract the traffic of interest to monitor by using monitoring tools of your choice. The following are the key concepts for Traffic Mirroring:

- Target — The destination for mirrored traffic.
- Filter — A set of rules that defines the traffic that is copied in a traffic mirror session.
- Session — An entity that describes Traffic Mirroring from a source to a target using filters.

Lastly there is global accelerator. It basically allows you to front things in the VPC at edge locations. **AWS Global Accelerator** is a service in which you create accelerators to improve availability and performance of your applications for local and global users. Global Accelerator directs traffic to optimal endpoints over the AWS global network. This improves the availability and performance of your internet applications that are used by a global audience. 

![AWS Global Accelerator](https://d1.awsstatic.com/Networking/AGA-Multi-Region-Usecase-2000px.39194f2f7dd49fca26217836d10d522298c1abcc.png)

Global Accelerator is a global service that supports endpoints in multiple AWS Regions. By default, Global Accelerator provides you with two static IP addresses that you associate with your accelerator. (Or, instead of using the IP addresses that Global Accelerator provides, you can configure these entry points to be IPv4 addresses from your own IP address ranges that you bring to Global Accelerator.) The static IP addresses are anycast from the AWS edge network and distribute incoming application traffic across multiple endpoint resources in multiple AWS Regions, which increases the availability of your applications. Endpoints can be Network Load Balancers, Application Load Balancers, Amazon EC2 instances, or Elastic IP addresses that are located in one AWS Region or multiple Regions. Global Accelerator uses the AWS global network to route traffic to the optimal regional endpoint based on health, client location, and policies that you configure. The service reacts instantly to changes in health or configuration to ensure that internet traffic from clients is always directed to healthy endpoints.

![Accelerated S2S VPN](https://docs.aws.amazon.com/whitepapers/latest/hybrid-connectivity/images/image7.png)

You can optionally enable acceleration for your Site-to-Site VPN connection. An accelerated Site-to-Site VPN connection (accelerated VPN connection) uses AWS Global Accelerator to route traffic from your on-premises network to an AWS edge location that is closest to your customer gateway device. AWS Global Accelerator optimizes the network path, using the congestion-free AWS global network to route traffic to the endpoint that provides the best application performance.
You can use an accelerated VPN connection to avoid network disruptions that might occur when traffic is routed over the public internet.

When you create an accelerated VPN connection, AWS create and manage two accelerators on your behalf, one for each VPN tunnel. You cannot view or manage these accelerators yourself by using the AWS Global Accelerator console or APIs.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDExNTI2ODU0LDEyMDEyMjYyMTgsMTQyND
Y3NDA0MCwtMjkzMDQ5Nzg2LDEzMTgxNTE1NjEsMTY5MDM3OTg5
MiwtODE2Mzg5NDEsMTA0OTU5NjY3LDEwNjc1NDE4MzQsLTEyOD
I5MDg3OTksLTc3NDI4ODQ3NSwtNjM4Mzg1MTUsMTIwNDg1NTQy
MiwtMzAxMTQwOTEwLDExOTM5Mzg0NDIsLTIzODQ3NTgwLDc1MD
Q5ODY2OCwtMTk2NjU4ODk3NCwxMzY4NzU1ODM4LC0zNDQxNTkx
NjBdfQ==
-->