# Elastic Compute Cloud (EC2) Basics 

## Virtualization 101

### Emulated virtualization

Hypervisor emulate virtual CPU / Memory / Disks

Binary Translation : call from guest OS to hypervisor but slow

### Para-Virtualization

Modified Linux OS, 

Hypercalls call to Hypervisor for privileged

### Hardware Assisted Virtualization

Knowledge of Virtualization

Virtual device which point to physical device

### SR-IOV

Awareness of Virtualization

Hardware device are aware of virtualization

- Can be present as single mini card
- Presented to Guest OS as dedicated car for it's use
- Can use the hardware device when ever it wants

In EC2 - This is **Enhanced Networking**

## EC2 Architecture

EC2 instance are virtual machines

EC2 instances run on EC2 Hosts

- Shared hosts (share with other customers)
- Dedicated hosts (dedicated to you)
  - Hosts run in AZ - AZ fails, hosts fails, Instance fails

EC2 Instance and EBS are locked to an AZ

if an Hosts fails or stop and start (not restart) Instance is moved to an other host

EC2 good for :

- Traditional OS + Application
- Long-running Compute
- Server style applications 
- Monolith application stacks
- burst or steady-state load
- Migrating application workloads or DR

## EC2 Instance Types

5 categories :

- General Purpose - Default - diverse workloads, equal resource ratio
- Compute Optimized - Media Processing, HPC, Scientific Modelling, gaming, ML
- Memory Optimized - Processing large in memory datasets, some db workloads
- Accelerated Computing - Hardware GPU, FPGA (field programmable gate array)
- Storage Optimized - Sequential & Random IO - Scale out DB, data warehouse, ElasticSearch, analytics

R5bn.8xlarge : instance type

- R : Instance Family
- 5 : Instance generation
- dn : Additional Capabilities
- 8xlarge : Instance Size

![](.\EC2-Instance_type.png)

## Storage Refresher

### Type of storage of EC2

Direct (local) attached Storage - Storage on EC2 host

Network attached Storage - EBS

Ephemeral Storage - Temporary Storage

Persistent Storage - Permanent storage - lives on past the lifetime of the instance

### Type of storage

Block storage - Volume presented to the OS as collection of blocks... no structure provied. Mountable Bootable. OS deploy an Filesystem on top of RAW volume (ext.fs, ntfs ...)

File Storage - Presented as a file share... has structure (folder / file). Mountable, not bootable

Object Storage - flat collection of objects (binary data, image, movie...) & metadata, no structure. not mountable, not bootable. Super scalable

### Storage Performance

IO (block) size x IOPS = Throughput

## Elastic block Store (EBS)

Block storage - raw disk allocations (volume) - can be encrypted using KMS

instances see block device and create file system on (ext3/4, xfs)

Storage is provisioned in ONE AZ (Resilient in that AZ)

Attached / detached and reattached to one EC2 (persistent volume)

Snapshot (backup) into S3, create volume from snapshot

Billed based on GB-month (some case performance)

![](.\EBS_Architecture.png)

## EBS Volume Types : General Purpose SSD

### GP2

Volumes can be 1GB to 16TB

IO Credit is 16KB

- IOPS assume 16KB
- 1 IOPS is 1 IO in 1 second

IO Credit Bucket capacity : 5,4 Million Credits

- Bucket fills with min 100 IO Credits per second
- Burst up to 3000 IOPS
- 30 minutes @3000 IOPS = 5,4 Million IO Credits

Volumes larger than 1000GB - Up to 16000 IO credits per second (base line performance)

- baseline is above burst - credit system isn't used 

Use cases : boot volumes, low-latency interactive apps, dev&test

### GP3

remove bucket architecture

3000 IOPS & 125 Mbits/s - Standard

20% cheaper than GP2

Extra cost for up to 16000 IOPS or 1Gbit/s

### io1/2

Up to 256k IOPS per volume (Block express)

Up to 4Gbit/s  (block express)

io1 50IOPS/GB (Max)

io2 500IOPS/GB (Max)

BlockExpress 1000IOPS/GB

4GB-16TB io1/2

4GB-64TB BlockExpress

Per instance Performance

- io1 - 260K IOPS & 7,5Gbit/s
- io2 - 160K IOPS & 4,75Gbit/s
- io2 Block Express - 260k IOPS & 7,5Gbit/s

### HDD based

st1 Throughput Optimized

- 125GB - 16TB
- Max 500 IOPS (1MB)
- Max 500 MB/s
- 40MB/s/TB base
- 250MB/s/TB

use case : Big data, data warehouse, log processing

sc1 cold HDD

- Max 250 IOPS (1MB)
- Max 250 MB/s
- 12MB/s/TB Base
- 80MB/s/TB Burst

use case :  cold storage few scan per days

### Memo

| **Volume Name**               | **General Purpose SSD**                                      | **General Purpose SSD**                                      | **Provisioned IOPS SSD**                                     | **Provisioned IOPS SSD**                                     |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Volume type**               | gp3                                                          | gp2                                                          | io2                                                          | io1                                                          |
| **Description**               | General Purpose SSD volume that balances price performance for a wide variety of transactional workloads | General Purpose SSD volume that balances price performance for a wide variety of transactional workloads | High performance SSD volume designed for **business-critical** latency-sensitive applications | High performance SSD volume designed for latency-sensitive transactional workloads |
| **Use Cases**                 | virtual desktops, medium sized single instance databases such as MSFT SQL  Server and Oracle DB, low-latency interactive apps, dev & test, boot volumes | Boot volumes, low-latency interactive apps, dev & test       | Workloads that require sub-millisecond latency, and sustained IOPS performance or more than 64,000 IOPS or 1,000 MiB/s of throughput | Workloads that require sustained IOPS performance or more than 16,000 IOPS and I/O-intensive database workloads |
| **Volume Size**               | 1 GB – 16 TB                                                 | 1 GB – 16 TB                                                 | 4 GB – 16 TB                                                 | 4 GB – 16 TB                                                 |
| **Durability**                | 99.8% – 99.9% durability                                     | 99.8% – 99.9% durability                                     | 99.999%                                                      | 99.8% – 99.9%                                                |
| **Max IOPS / Volume**         | 16,000                                                       | 16,000                                                       | 64,000                                                       | 64,000                                                       |
| **Max Throughput / Volume**   | 1000 MB/s                                                    | 250 MB/s                                                     | 1,000 MB/s                                                   | 1,000 MB/s                                                   |
| **Max IOPS / Instance**       | 260,000                                                      | 260,000                                                      | 160,000                                                      | 260,000                                                      |
| **Max IOPS / GB**             | N/A                                                          | N/A                                                          | 500 IOPS/GB                                                  | 50 IOPS/GB                                                   |
| **Max Throughput / Instance** | 7,500 MB/s                                                   | 7,500 MB/s                                                   | 4,750 MB/s                                                   | 7,500 MB/s                                                   |
| **Latency**                   | single digit millisecond                                     | single digit millisecond                                     | single digit millisecond                                     | single digit millisecond                                     |
| **Multi-Attach**              | No                                                           | No                                                           | Yes                                                          | Yes                                                          |

| **Volume Name**               | **Throughput Optimized HDD**                                 | **Cold HDD**                                                 |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Volume type**               | st1                                                          | sc1                                                          |
| **Description**               | Low cost HDD volume designed for frequently accessed, throughput-intensive workloads | Throughput-oriented storage for data that is infrequently accessedScenarios where the lowest storage cost is important |
| **Use Cases**                 | Big data, data warehouses, log processing                    | Colder data requiring fewer scans per day                    |
| **Volume Size**               | 125 GB – 16 TB                                               | 125 GB – 16 TB                                               |
| **Durability**                | 99.8% – 99.9% durability                                     | 99.8% – 99.9% durability                                     |
| **Max IOPS / Volume**         | 500                                                          | 250                                                          |
| **Max Throughput / Volume**   | 500 MB/s                                                     | 250 MB/s                                                     |
| **Max IOPS / Instance**       | 260,000                                                      | 260,000                                                      |
| **Max IOPS / GB**             | N/A                                                          | N/A                                                          |
| **Max Throughput / Instance** | 7,500 MB/s                                                   | 7,500 MB/s                                                   |
| **Multi-Attach**              | No                                                           | No                                                           |

## Instance Store Volumes - Architecture

Block Storage Devices - ephemeral storage (temporary)

Physically connected to one EC2 hosts (local)

Instances on that host can access them

Highest storage perf in AWS

Included in instance price

ATTACHED AT LAUNCH ONLY (can't be attached later)

if an instance move to an other host, rezise or hardware failure data are lost

## Choosing between Instance Store vs EBS

Persistence / resilience : EBS

Super High perf / Cost : Instance store

**Cheap : ST1 or SC1**

**Throughput or Streaming : ST1**

**Boot : not ST1 or SC1**

GP2/3 - Up to 16k IOPS

IO1/2 - Up to 64k IOPS

**RAID0 + EBS** up to 260,000 IOPS (io1/2-BE/GP2/3)

More than 260k IOPS - Instance Store

## Snapshots, Restore & Fast Snapshot Restore (FSR)

Snapshots are incremental volume copies to S3

Snapshots are Region Resilient (increase resilience of EBS volume)

First is a full copy of **'data'** on the volume, future snaps are incremental (only diff)

Volumes can be created (restored) from snapshots

snapshots can be used to move EBS volume to other AZ or Region

- for DR or migration

New EBS volume = full perf immediately

snaps restore lazily 

Fast Snopshot Restore (FSR) - Immediate restore (expensive)

up to 50 snaps per region (set on the snap & AZ)

Billed on GB/month only data store (not allocated)

## EBS Encryption

Customer Master Key stored in KMS

DEK (Data Encryption Key) stored in EC2 Host

in case of migration DEK needs to be loaded from KMS to an other EC2 Host

Snapshot are encrypted with DEK, same for Volume created from snapshots

Each volume uses 1 unique DEK

Can't change a volume to NOT be encrypted

OS isn't aware of the encryption... no performance loss

## Network Interfaces, Instance IPs and DNS

EC2 starts with 1 interface Primary ENI (Elastic Network Interface)

- can have a Secondary ENI (in same AZ)
- MAC Address
- Primary IPv4 Private IP 
  - fixe DNS local name
    - for exemple 10.16.0.10 -> ip-10-16-0-10.ec2.internal
- 0 or more secondary IPs
- 0 or 1 Public IPv4 Address 
  - change at start and stop
  - don't change at restart
  - public DNS name
    - for exemple 3.89.7.136 -> ec2-3-89-7-136.compute-1.amazonaws.com
      - resolve internally in the VPC return Private IP
      - resolve elsewhere return Public IP
- 1 Elastic IP per private IPv4
  - removes the public IPv4 and replaces with Elastic IP (no rollback to previous pub IP)
- 0 or more IPv6 addresses
- Security Groups

### Exam Power UP

Secondary ENI + MAC = Licensing

- can be used to swap ENI to an other instance and move license too

Multi-homed (subnets) Management & Data

- Security group are attached to ENI not IPs

OS never see public IPv4

IPv4 Public IPs are Dynamic .. Stop & Start = Change

Public DNS = private IP in VPC, public IP everywhere else

## Amazon Machine Image

Regional unique ID

Permissions (Public, Your Account, Specific Accounts)

Can create an AMI from an EC2 Instance

1. Launch : 
2. Configure
3. Create Image
4. Launch

AMI is an object stored in a region, The Snapshot is stored in S3

AMI is container which reference a snapshot from an original EBS volume

### Exam Power UP

AMI = One Region, only works in that one region

AMI Baking... creating an AMI from a configured instance + application

AMI can't be edited.. launch instance, update configuration and make a new AMI

Can be copied between regions (includes its snapshots)

Permissions default = your account

AMI cost : billed for capacity of the snapshot

## Instance Pricing Models

On-Demand Instances

- hourly rate
- Billed in seconds (60s minimum) or Hourly
- On-demand is default pricing model
- No long term commitment or upfront payment
- unpredictable workloads which can't tolerate any disruption

Spot Instances

- 90% off vs On Demand
- Spot price is set by EC2 based on spare capacity
- can specify maximum price you'll pay
- if goes above price - instance terminate
- great for application can support downtime or can be terminated (can tolerate failure)
- low cost

Reserved Instances

- Up to 75% off vs On-demand for a commitment
- 1 or 3 years, all upfront, partial upfront, no upfront
- Reserved in region, or AZ with capacity reservation
- Known steady state usage (Mail server, AD, website)

Dedicated Hosts (other lesson)

## Instance Status Check

System reachability check

- Loss of system power
- Loss of network Connectivity
- Host software issues
- Host hardware issues

Instance Check

- Corrupted file system
- Incorrect instant Networking
- OS Kernel Issues

Auto-recovery move instance to another host

## Horizontal & Vertical Scaling

Vertical : add more RAM & CPU (change EC2 type)

- Each resize requires a reboot - disruption
- Larger instances often carry a $ premium
- There is an upper cap on performance - instance size
- No application modification required
- Works for ALL applications - even Monolitics

 Horizontal : add more instance

- Sessions, sessions, sessions
- requires application support or off-host sessions (sessions externaly hosted)
- no disruption when scaling
- Often less expensive - no large instance premium
- More granular

## Instance Metadata

http://169.254.169.254/latest/meta-data/

Accessible inside ALL instances

EC2 Service provides data to Instances

- Environment
- Networking
- Authentication

## Quiz

3 States of EC2 Instance :

- Running
- Stopped
- Terminated

Instance store volumes :

- are temporary
- data can be lost when EC2 instance stops and starts
- Data stored can be lost on hardware failure

if AZ fails, EC2 instance fail until AZ recovers

EC2 instance can't be migrated between AZ, you need to create and AMI and use it to clone to an other AZ

IO1 EBS use-case : maximum consistent IOPS is a priority and data is important

Volume can be attached to 1 instance at a time in the same AZ

Instance store volumes use for :

- replaceable / temporary data
- max IOPS

Cant tolerate interruption : On-demand