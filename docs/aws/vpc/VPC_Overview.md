# [Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html)
An Amazon Virtual Private Cloud (VPC) enables you to launch AWS resources into a virtual network that you've defined. It closely-resembles traditional networks that would normally be operated in your own data center, with the benefits of using the scalable infrastructure of AWS.

## VPC Concepts
Amazon VPC **is the networking layer for Amazon EC2**.

### VPCs and Subnets
- A VPC is a virtual network _dedicated to your AWS account_
  - It is logically-separated from other virtual networks in the AWS cloud
- You can _launch your AWS Resources_ into your VPC
  - Such as EC2
- You can configure your VPC by:
  - Modifying its IP address range
  - Create subnets
  - Configure
    - Route tables
    - Network Gateways
    - Security Settings

**_Subnet_**: _a range of IP addresses in your VPC_
- You can launch AWS resources _into a specified subnet_
  - Use a public subnet for resources that must be connected to the internet
  - Use a private subnet for resources that won't be connected to the internet
- For more information on Public v. Private Subnets, see [VPC and Subnet Basics](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#vpc-subnet-basics)

### Supported Platforms
The original release of EC2 supported a single, flat network that's shared with other customers called the EC2-Classic platform. Accounts created after 2013-12-04 support EC2-VPC only. By launching your instances into a VPC instead of EC2-Classic, you gain the ability to:
1. Assign static private IPv4 addresses to your instances that persist across starts and stops
2. Optionally associate an IPv6 CIDR block to your VPC and assign IPv6 addresses to your instances
3. Assign multiple IP addresses to your instances
4. Define network interfaces and attack one or more network interfaces to your instances
5. Change security group membership for your instances while they're running
6. Control the outbound traffic from your instancess ("egress filtering) in addition to controlling the inbound traffic to them ("ingress filtering")
7. Add an additional layer of access control to your instances in the form of network access control lists (ACL)
8. Run your instance on single-tenant hardware

### Default and Nondefault VPCs
If your account supports the EC2-VPC platform only (e.g. all accounts created after 2013-12-04), it comes with a _default VPC_ that has a _default subnet_ in each Availability Zone. A default VPC has the benefits of the advanced features provided by EC2-VPC, and is ready for use out-of-the-box. _If you have a default VPC and don't specify a subnet when you launch an instance, the instance is launched into your default VPC_. You can launch instances into your default VPC without needing to know anything about Amazon VPC.

### Accessing the Internet
You control how the instances that you launch into a VPC access resources outside the VPC.
- Each instance that you launch into a default subnet has a private IPv4 address and a public IPv4 address
  - The instances can communicate with the internet through the internet gateway
  - An internet gateway enables your instances to connect to the internet through the Amazon EC2 network edge
- By default, each instance you launch into a nondefault subnet has a private IPv4 address, but no public IPv4 address (unless you specifically assign one at launch, or you modify the subnet's public IP address attribute)
  - These instances can communicate with each other, but can't access the internet

#### NAT (Network Address Translation)
To allow an instnace in your VPC to initiate outbound connections to the internet but prevent unsolicited inbound connections from the internet, you can use a NAT device for IPv4 traffic.
- NAT maps multiple private IPv4 addresses to a single public IPv4 address
- A NAT device has an elastic IP address and is connected to the internet through an internet gateway
- You can connect an instance in a private subnet to the internet through the NAT device

For more information, see [NAT](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat.html)

### Accesing a Corporate or Home Network
You can optionally connect your VPC to your own corporate data center using an IPsec AWS manged VPN connection, making the AWS Cloud an extension of your data center.

For more information, see [AWS Managed VPN Connections](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html)

### Accessing Services Through AWS PrivateLink
AWS PrivateLink is a highly-available, scalable technology that enables you to privately connecct your VPC to supported AWS services, services hosted by other AWS accounts (VPC endpoint services), and supported AWS Marketplace partner services. _You do not require an internet gateway, NAT device, public IP address, AWS Direct Connect connection, or VPN connection to communicate with the service._ Traffic between your VPC and the service does not leave the Amazon network.

To use AWS PrivateLink, create an interface VPC endpoint for a service in your VPC. This creates an elastic network interface in your subnet with a private IP address that serves as an entry point for traffic destined to the service. For more information, see [VPC Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html).

You can create your own AWS PrivateLink-powered service (endpoint service) and enable other AWS customers to access your servicce. For more information, see [VPC Endpoint Services (AWS PrivateLink)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/endpoint-service.html)