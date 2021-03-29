# Virtual Private Cloud (VPC) Basics

## Networking Refresher

Classful (Fixed length subnet masking)

Class A : 0.0.0.0 -> 127.255.255.255/8

​	16,777,216 IPs Per Network

​	private network : 10.0.0.0 - 10.255.255.255

Class B : 128.0.0.0 -> 191.255.255.255

​	65,536 IPs per Network

​	private network : 172.16.0.0 - 172.31.255.255

Class C : 192.0.0.0 -> 223.255.255.255

​	256 IPs per Network

​	private network : 192.168.0.0 - 192.168.255.255

CIDR : Classless inter-domain routing (Variable length subnet masking)

Ability to split 10.0.0.0/16 into 2 10.0.0.0/17 and 10.0.128.0/17

​	or four 10.0.0.0/18 - 10.0.128.0/18 - 10.0.64.0/18 - 10.0.192.0/18

IPv6 : 8 Hextet (Two octets = Hextet)

:0: = 0000 / :0:0: = ::

2001:db8:1235::/48 -> 2001:db8:1235:0000:0000:0000:0000:0000

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

Planning :



3+1 AZ A-B-C and Future

16 subnets in total

### Exemple Sizing

6 Regions : US x3, EU, Australia x2

4 accounts : General, Prod, Dev, Reserved (for spare)

4 tiers :  WEb / App / DB and 1 Spare

4 AZ : A - B - C and 1 Spare

=> 16 subnets per VPC

/16 per VPC split into 16 subnets = /20 per subnet (4091 IPs)

each account has 4 VPC

US Region 1 : 10.16.0.0/12

US Region 2 : 10.32.0.0/12

## Custom VPC

Regional Service - All AZs in the region

Isolated Network

Nothing IN or OUT without explicit config

Default or Dedicated Tenancy (cost premium)

Hybrid Networking - other cloud & on prem

