# Simple Storage Service (S3)

## S3 Security

S3 is private by default

resource policy (attached to a bucket, who can access)

allow/deny same or different accounts

"Principal" in resource policies

Identity + Resource policies

Block Pubic Access

## Static Website Hosting

- allows access via http
- index and error html file
- allow public access
- add HTML files

## Object Versioning & MFA Delete

Versioning can't be disabled

- Can be suspended then enabled

Versioning enable multiple versions

- modify id=null (vers disable) with version

Consumed space by all versions

### Multi Factor Authentication

MFA Delete : enabled in versioning config

MFA required to change bucket versioning state

MFA required to delete versions

## S3 Performance Optimization

Single data stream to S3 s3:PutObject

Up to 5GB file

- Multipart Upload (min 100MB file)
- 5MB parts minimum 10000 max parts

S3 Transfer Acceleration use AWS edge location

- use aws internal network

## Encryption 101

Encryption At Rest

- Protect agains theft
- Data are protect in shared storage

Encryption In transit

- Tunnel encryption

Symetric Encryption

- Plaintext message use Algorith and Key to encrypt a Ciphertext

Asymmetric Encryption

- Public key use to encrypt
- Private key use to decrypt

## Key Management Service (KMS)

Regional & Public Service
Create, Store and manage keys
Symmetric and Asymmetric
encrypt and decrypt
Keys never leave KMS (FIPS 140-2 L2)
CMK (Customer Master Key)
CMK is logical - ID, date, policy, desc & state
backed by physical key material
Generated or Imported
CMK up to 4KB of data
CMK stays inside KMS (even for encrypt / decrypt...)
Data Encryption Keys 
	GenerateDataKey - works on > 4KB
	Paintext Version
	Ciphertext version
CMK are isolated to a region & never leave
AWS managed or Customer managed keys
CMK are more configurable
CMKs support rotation
Backing Key
Aliases
Key Policies (every CMK has one) + IAM Policies

## S3 Encryption           

Buckets aren't encrypted, objects are
Encryption at rest 
	Client-Side Encryption
	Server-Side Encryption 
Server-Side Encryption
	SSE-C (Customer Provided Keys)
		Key used to encrypt
		Hash generated to identify the key
		Key discarded after encryption
		Hash used to validate key for decryption
		User have to manage keys
		Less CPU required Server Side
		Specific security requirements
	SSE-S3 (S3 Managed keys AES256)
		S3 generate the Master Key
			generate a key to encrypt object
			keeps encrypt key & encrypt object
		User can't manage key rotation
		Can't manage Role separation
			Full S3 admin will have full acess to data
	SSE-KMS (Customer Master Keys stored in KMS)
		Role separation
		Manage key rotation
		external to S3

## S3 Storage Classes

### S3 Standard 

replicated at least 3 AZs, 9 9's durability

Content MD5 and CRC to detect data corruption

GB/month fee, $ per GB charge for transfer OUT (IN is free), price per 1000 request

1 ms latency

### S3 Standard IA (Infrequent Access)

Same as Standard, but cheaper

cost increases with frequent data access

minimum duration charge of 30 days

minimum capacity of 128KB per object

### S3 One Zone IA

Same as Standard IA but only one AZ

9 9's durability unless AZ fails

used for long lived data, non critical, replaceable and access infrequent

### S3 Glacier

Cold storage

Same availability and durability as S3 Standard

Objects cannot be made publicly accessible

Retrieve to S3

- Expedited (1-5 minutes)
- Standard (3-5 hours)
- bulk (5-12 hours)
- Faster = More expensive

### S3 Glacier Deep Archive

Same as Glacier but

40KB min size and 180 Day min duration

Retrieve time

- Standard (12 hours)
- Bulk (up to 48 hours)
- 4/ of the price of Glacier

### S3 Intelligent-Tiering

1utomatically mives any objects not accessed for 30 days to low cost IA tier then archives and deep archive
Accessed object move back to frequent access tier (no retrieval fees for min 30 days)
Cost per 1000 objects
Should be used for long lived data with changing or unknown patterns

## S3 Filecycle Configuration

Set of rules (rules consist of actions)

- on a bucket or groups of objects

Transition and Expiration Actions

Minimum of 30 days before new transition between IA and other in the same rule

## S3 Replication

Cross Region Replication (CRR)

- Use bucket policy to allow IAM role for replication

Same Region Replication (SRR)

- IAM role has access to source and destination bucket

All objects or a subset (tags, filtre)

Storage Class is to maintain

Ownership - default is the source account 

Replication Time Control (RTC), SLA 15 minutes 

Source to Destination (one way only)

Versioning needs to be ON

Unencrypted, SSE-S3 & SSE-KMS (with extra config)

Source bucket owner needs permissions to objects

Use case :

- SRR - Log Aggregation
- SRR - Prod and Test Sync
- SRR - Resilience with strict soverignty
- CRR - Global Resilience Imporvements
- CRR - Latency Reduction

## S3 Presigned URL

Admin generate presignedURL

send presigned URL to Unauthenticated user to Upload (PUT) or DL (GET)

You can create a URL for an object you have no access too

When using the URL, the permissions match the identity which generated it

Access denied could mean the generating ID never had access or doesn't now

Don't generate with a role.. URL stops working when temporary credentials expire

## S3 Select and Glacier Select

This provides a ways to retrieve parts of objects and not the entire object.

If you retrieve a 5TB object, it takes time and consumes 5TB of data. Filtering at the client side doesn't reduce this cost.

S3 and Glacier select lets you use SQL-like statement.

The filtering happens at the S3 bucket source

## S3 Event Notification

Notification generated when events occur in a bucket

can be delivered to SNS,SQS and Lambda Functions

Object Created / Delete / Restore and Replication

## S3 Access Logs

S3 Log Delivery Group

- Source bucket to Target Bucket
- use ACL to allow access to Target Bucket
  	