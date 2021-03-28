# IAM

## IAM Identity Policies

IAM policy document : JSON file
1+ Statement (SID : statement ID)
1 : Explicit deny (take priority)
2 : Explicit allow  
deny allow deny (default implicit)
Inline ( 3 isolated policies, use for special or exceptional allow or deny)
Managed policy (reusable and low management overhead)
	manage by AWS or Customer manage

## IAM Users

Humans, Applications or service accounts
User / Password or Access Keys
Authenticated Identity is something identified to IAM
ARN : Amazon Resource Name
	uniquely identify resources within any AWS accounts
arn:aws:s3:::catgifs (Bucket)
arn:aws:s3:::catgifs/* (Objects in the Bucket)
5,000 IAM Users per account
IAM User can be a member of 10 groups
This has systems design impacts
Internet-scale applications / large orgs
IAM Roles & Identity federation

## IAM Groups

Groups are containers for Users
Group policies + User permissions
Default 300 groups per account can be increase by support
No groups in groups
groups are not identity, ressources policy can't grant permission to groups

## IAM Roles

Role is a level of access for a short time
Trust policy (who can assume that role)
	a user, an app, something outside the ccount
permissions policy (what they can do)
Secure Token Service : sts:AssumeRole manage Temporary Security Credentials
	for short time only
When to use IAM Roles
	Lamba needs access for exemple
	Lambda Execution Role 
	Code run in a rutime environnement and assume the role
	the runtime env get sts:AssumeRole

External accounts can't be used in AWS directly

## AWS Organizations

Management Account (previously Master)
Organization Root -> Organizational Unit (OU)
Member Account pass biling through Management Account
Can be use for a login account (from on prem) then use roles switch to other aws account

## Service Control Policies

Management can't be restricted by SCP
SCP are account permissions boundaries (including account root user)
define limit for exemple size of EC2 instance
SCPs can be applied to the organization, to OU's or to individual accounts.
Allow list and Deny list
SCPs DON'T GIVE permission - they just control what an account CAN and CANNOT grant via identity policies.
Overlap of Identity policies in accounts + SCP

## CloudWatch Logs

Public service (usable from aws or on prem)
Store, monitor, access logs
log stream (any source) => log group
log group : retention & permissions for log streams
	=> metric filter => metric => alarm

## CloudTrail

Logs API calls/activies as Event
90 days by default (cost free)
Customisable
Management event by default, can be activate for data
one region or all regions trail
stored in S3 (compressed JSON file)
not realtime - there is delay
