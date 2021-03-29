# Introduction

## What is cloud computing

**Using cloud resources instead of your own is like purchasing electricity from a power company instead of running your own generator**

On demande IT ressources

Pay as you go pricing

flexible and low cost IT ressources

many resources as you need instantly (servers / storage / db)

no up-front invest

### Advantages

Transform capital expense into operational expense

- no heavy invest in DC and servers...
- pay only when and how much you consume

Economies of Scale

- benefits from massive scale
- hundreds of thousand of customers aggregated

Stop guessing capacity

- no idle or limited ressources
- scale up or down in a minute

Speed and agility

- new ressource in a clic (from weeks to minutes)
- experiment and develop lower time

Focus on Business differentiators

- stop spending money on running and maintaining data centers

Go global

- multiple locations (lower latency / better exp)
- geo redondancy

## Fundamentals

Region is separate geographic area

- Each region is completely independent and is designed to be completely isolated from the
  other regions`

Each region has multiple, isolated locations known as Availability Zones

- AZ in a region are connected through low-latency links
- AZ are physically separated within a typical metropolitan
  region and are located in lower-risk flood plains

# AWS Cloud Computing Platform

## Amazon Elastic Compute Cloud (Amazon EC2)

VM : memory, CPU, storage, and so on

## AWS Lambda

Zero-administration compute platform for back-end web developers

Lambda runs your back-end code on its own AWS compute fleet of Amazon
EC2 instances across multiple AZ

## Auto Scaling

Allows resources to scale in and out to match the demands of dynamic workloads

## Elastic Load Balancing

automatically distributes incoming application traffic across multiple
Amazon EC2 instances in the cloud

## AWS Elastic Beanstalk

automatic resource provisioning, load balancing, auto scaling...

## Amazon Virtual Private Cloud (Amazon VPC)

AWS resources in a virtual network

selection of the IP address range, creation of subnets, and configuration of route tables and
network gateways, VPN

## AWS Direct Connect

stablish a dedicated network connection from their data center to AWS

## Amazon Route 53

highly available and scalable Domain Name System (DNS) web service

Storage and Content Delivery

## Storage

### Amazon Simple Storage Service (Amazon S3)

scalable object storage

### Amazon Glacier

secure, durable, and extremely low-cost storage service for data archiving and long-term backup

### Amazon Elastic Block Store (Amazon EBS)

persistent block-level storage volumes for use with Amazon EC2 instances

### AWS Storage Gateway

connecting an on-premises software appliance with cloudbased storage

### Amazon CloudFront

CDN : low latency, high data transfer speeds, and no minimum usage commitments

Requests for content are automatically routed to the nearest edge location

### Database Services

fully managed relational and NoSQL database services

### Amazon Relational Database Service (Amazon RDS)

fully managed relational database with support for many popular open source and commercial database engines

### Amazon DynamoDB

fast and flexible NoSQL database service for all applications that need consistent, single-digit millisecond latency at any scale

### Amazon Redshift

data warehouse service that makes it simple and cost effective to analyze structured data

provides a standard SQL interface that lets organizations use existing business intelligence tools

### Amazon ElastiCache

simplifies deployment, operation, and scaling of an in-memory cache in the cloud

Amazon ElastiCache supports Memcached and Redis cache engines

## Management tools

### Amazon CloudWatch

monitoring service for AWS Cloud resources and the applications

collect and track metrics, collect and monitor log files, and set alarms

### AWS CloudFormation

JSON-based templating language to describe all the AWS resources

### AWS CloudTrail

records AWS API calls for an account and delivers log files for audit and review

### AWS Config

AWS resource inventory, configuration history, and configuration change notifications to enable security
and governance

## Security and Identity

### AWS Identity and Access Management (IAM)

create and manage AWS users and groups and use permissions to allow and deny their access

### AWS Key Management Service (KMS)

create and control the encryption keys used to encrypt their data and uses
Hardware Security Modules (HSMs) to protect the security of your keys

### AWS Directory Service

set up and run Microsoft Active Directory

manage users and groups, provide single sign-on to applications and services, create and apply Group Policies

### AWS Certificate Manager

Secure Sockets Layer/Transport Layer Security (SSL/TLS) certificates for use

### AWS Web Application Firewall (WAF)

which traffic to allow or block to their web applications by defining customizable web security rules

## Application Services

### Amazon API Gateway

makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale.

### Amazon Elastic Transcoder

media transcoding in the cloud

### Amazon Simple Notification Service (Amazon SNS) 

web service that coordinates and manages the delivery or sending of messages to recipients

### Amazon Simple Email Service (Amazon SES)

email service that organizations can use to send transactional email, marketing messages

### Amazon Simple Workflow Service (Amazon SWF) 

helps developers build, run, and scale background jobs that have parallel or sequential steps

### Amazon Simple Queue Service (Amazon SQS)

fast, reliable, scalable, fully managed message queuing service
