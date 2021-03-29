# Virtual Private Cloud (VPC) Basics

## Networking Refresher

Classful (Fixed length subnet masking)

Class A : 0.0.0.0 -> 127.255.255.255/8

- 16,777,216 IPs Per Network
- private network : 10.0.0.0 - 10.255.255.255

Class B : 128.0.0.0 -> 191.255.255.255

- ​	65,536 IPs per Network
- ​	private network : 172.16.0.0 - 172.31.255.255

Class C : 192.0.0.0 -> 223.255.255.255

- ​	256 IPs per Network
- ​	private network : 192.168.0.0 - 192.168.255.255

CIDR : Classless inter-domain routing (Variable length subnet masking)

Ability to split 10.0.0.0/16 into 2 10.0.0.0/17 and 10.0.128.0/17

- ​	or four 10.0.0.0/18 - 10.0.128.0/18 - 10.0.64.0/18 - 10.0.192.0/18

IPv6 : 8 Hextet (Two octets = Hextet)

- :0: = 0000 / :0:0: = ::
- 2001:db8:1235::/48 -> 2001:db8:1235:0000:0000:0000:0000:0000

## VPC Sizing and Structure

What size should be the VPC

Are there any Networks we can't use ? (Cloud, on prem, partenrs & vendors)

Try to think about the future... size & Resiliency (AZ)

Attention : 10.128.0.0 (reserved to google VPC)

VPC minimum /28 (16 IP), maximum /16 (65456)

| VPC Size    | Netmask | Subnet Size | Hosts/Subnet | Subnet/VPC | Total IPs |
| ----------- | ------- | ----------- | ------------ | ---------- | --------- |
| Micro       | /24     | /27         | 27           | 8          | 216       |
| Small       | /21     | /24         | 251          | 8          | 2008      |
| Medium      | /19     | /22         | 1019         | 8          | 8152      |
| Large       | /18     | /21         | 2043         | 8          | 16344     |
| Extra Large | /16     | /20         | 4091         | 8          | 65456     |

### Exemple Sizing

6 Regions : US x3, EU, Australia x2

4 accounts : General, Prod, Dev, Reserved (for spare)

4 tiers :  WEb / App / DB and 1 Spare

4 AZ : A - B - C and 1 Spare

=> 16 subnets per VPC

- /16 per VPC split into 16 subnets = /20 per subnet (4091 IPs)
- each account has 4 VPC
- US Region 1 : 10.16.0.0/12
- US Region 2 : 10.32.0.0/12

## Custom VPC

Regional Service - All AZs in the region

Isolated Network

Nothing IN or OUT without explicit config

Default or Dedicated Tenancy (cost premium)

Hybrid Networking - other cloud & on prem

1 Primary Private IPv4 Block (optional secondary)

Min /28 (16 IP), Max /16 (65,536)

Optional single IPv6 /56 (256 /64)

### DNS

Provided by R53

PVC 'Base IP +2' Address

enableDnsHostname : give DNS Hosname for public service

enableDnsSupport : enable DNS resolution in VPC

## VPC Subnets

Subnet assign to an AZ (cannot be in more than 1 AZ)

Multiple subnet in AZ

Cannot overlap with other subnet

Subnets can communicate with other subnets in the VPC

### 5 Reserved IP adresses

- exemple : 10.16.16.0/20 (10.16.16.0 - 10.16.31.255)
  - Network Address (10.16.16.0)
  - VPC Router : Network +1 (10.16.16.1)
  - Reserved (DNS) : Network +2 (10.16.16.2)
  - Future Use : Network +3 (10.16.16.3)
  - Broadcast Adress : Last IP in subnet 10.16.31.255

Options :

1. Auto Assign Public IPv4
2. Auto Assign IPv6
3. DHCP Option Set

## VPC Routing and Internet Gateway

### Router

Every VPC has a VPC Router - Highly available (network +1 address)

Routes traffic between subnets in a VPC

Main route table - subnet default

Higher prefix (more specific) = higher priority

### Internet Gateway

1 IGW per VPC

Runs from AWS Public Zone

Region resilient

Gateways traffic between the VPC and Internet or AWS Public Zone (S3, SQS, SNS...)

Workflow :

1. Create IGW
2. Attach IGW to VPC
3. Create custom route table (RT)
4. Associate RT 
5. Default Routes => IGW
6. Subnet allocate IPv4

Public IPv4 are not directly assigned in the EC2 OS but attached to IGW (NAT priv:pub)

Public IPv6 are directly assigned in an EC2 OS

### Demo

1. Convert subnets public ip assigned
2. Create Internet GW and associate to VPC
3. Add route table and add subnets to the route table
4. adding default route to Internet gateway

## Network Acess Control List

Processed in order

Low Rule # First

Stops if matched

Rule # / Type / Protocol / Port Range / Source / Action

Implicit DENY by default, * rule cannot be deleted or edited

Inboud or Outboud

Stateless FW, flow have to be opened both ways (Inbound and Outbound)

no logical resources only subnets

One subnet = One NACL at a time (default or custom)

## Security Group

Statefull (Response automatically allowed)

Hidden Implicit DENY

Can only allow / No Explicit DENY

can filter based on AWS resouces

can reference other SGs

### SGs vs NACLs

NACLs on on subnet for any products which don't work with SG'S (ex : NAT GW)

ANCLs when adding explicit DENY (bad IP's, bad actors)

SG as the default **almost everywhere**

## Network Address Translation (NAT) and NAT Gateways

mapping SRC or DST IP

IP masquerading (hiding CIDR blocks behind one IP)

Gives Private CIDR range outgoing internet acess

IP blocks -> NAT GW -> Internet GW

- Runs from a public subnet
- Uses Elastic IPs (static IPv4 Public)
- AZ resilient service (HA in that AZ)
- For region resilience - NATGW in each AZ
- RT in for each AZ with that NATGW as target
- Charging $ on Duration & Data volume
- Can scale up to 45 Gbps

**NAT GW don't support security group only NACLs**

NAT GW don't work with IPv6

All IPv6 addresses in AWS are publicly routable

- add route ::/0 route + IGW for bi-dir connectivity
- add route ::/0 route + Egress-Only Internet GW (outbound only)

## Quiz

VPC Provide Isolated Network

Regions can only have 1 Default VPC and many custom VPCs

Custom VPC allow flexible network conf, default VPC has fixed scheme

Some services can behave oddly if the default VPC doesn't exist

Default VPCs can be recreated

Max /16 Min /28

An AZ can have many subnets, a subnet is in one AZ

5 IP Addresses reserved in each VPC Subnet

IGW is HA by default - attached to a VPC

A subnet can have one Route table attached

A route table can be associated with multiple subnets