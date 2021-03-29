Cloud NIST definition

1. On-Demand Self-Service: can provision capabilities as needed without requiring human interaction (compute / storage / network / db)
2. Broad Network Access : capabilities are available over the network and accessed through standard mechanisms (HTTP over internet) 
3. Resource pooling : location independence (no control or knowledge of location of resources), resource pooled to serve multiple consumers using multi tenant model
4. Rapid Elasticity : capabilities can be elastically provisioned and released to scale rapidly, for consumer ressource should appear as unlimited
5. Measured service : resource usage can be monitored, controlled, reported and bill



# AWS Fundamentals      

## AWS Global Infrastructure

Region is separate geographic area

- Each region is completely independent and is designed to be completely isolated from the
  other regions`

Each region has multiple, isolated locations known as Availability Zones

- AZ in a region are connected through low-latency links
- AZ are physically separated within a typical metropolitan
  region and are located in lower-risk flood plains

## AWS Virtual Private Cloud

VPC : Virtual Network inside AWS

- within 1 account & region
- private and isolated
- default and custom

VPC CIDR : IP Range (default 172.31.0.0/16)

stretch over all AZ  of region /20 (ex : 172.31.0.0 - 172.31.15.255)

Internet Gateway (IGW), Security group (SG) & NACL

## Elastic Compute Cloud (EC2) Basics

IAAS - Provides VM (diffe=> Instances

Private service by default (VPC)

AZ resilient

On demand Billing

Local on host storage or EBS

## Simple Storage Service

Object (zero bytes to 5TB)

- Key : koala.jpg (in a bucket)
- value : content being stored (image for exemple)
- Metadata / Version / Acess Control / Subresources

S3 Buckets

Flat / Store unlimited objects

Blast Radius = Region

Globally unique (region and aws account)

100 soft limit / 1000 hard limit per by account

## High-Availability vs Fault-Tolerance vs Disaster Recovery

HA : minimise any outage (can support a reboot and restart somewhere else for exemple)

Fault tolerance : operate through fault (should be transparent)

Disaster Recovery : use when these doesn't work

## Quiz

AMI Permissions : Public Access, Owner only, Specific AWS Accounts

NOT Stored in AMI : Instance settings, Network settings

- Stored : Boot volume, data volume, AMI Permissions, Block device mapping

EC2 is an exemple of IAAS model

AWS Public Service are :

- Located in AWS Public zone
- Anyone can connect but permissions are required

AWS Private Service are :

- Located in a VPC
- Accessible from the VPC it is located in
- Accessible from other VPC or on prem networks if network is configured

S3 is :

- AWS Public Service
- An object storage system
- Buckets can store an unlimited amount of data

CloudFormation Logical Resource is a resource defined in a template

CloudFormation Physical Resource is a resource created by a CF stack

Definition of High Availability : system which maximise uptime

Definition of Fault tolerant : system which allow failure and can continue operating without disruption

DNS :

- 13 DNS Root servers
- Manage by 12 Large Organisations
- DNS root zone manage by IANA
- DNS record type A : converts host into an IPv4 addr
- DNS record type NS : delegates control of .org to a registry
- Registry maintains the zone for a TLD (ex : .org)
- Registrar allow domain registration to a TLD registry

Number of subnets in a default VPC  =  number of AZs in the region the VPC is located in

- default VPC IP CIDR : 172.31.0.0/16