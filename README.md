# aws-solutions-architect-associate
This is my notes for AWS Solutions Architect Associate Exam


## IAM

Consist of:

- Users
- Groups 
- Roles
- Policies

Policies are in JSON syntax

- IAM is universal
- root account has Admin Acces
- new users have no permissions when created
- have access key and secret access key
- can only be viewed once

So far:

- alway MF
- rolling password policy

## S3

S3 is object-based. Files can be from 0 bytes to 5tb. Unlimited storage. Stored in buckets. S3 is a universal namespace.

Not suitable to install OS and dbs.

Bucket have policies and Access control lists.

S3 fundamentals:

- key
- value
- version ID
- metadata
- subresources
    - ACL AND TORRENTS

### S3 Exam TIPS:

- Read after Write consistency for PUTS of new Objects
- Eventual Consistency for overwrite Puts and Deletes

Access Tiers:

- S3 Standard 
    - most expensive
    - 99.99 availability and durability
    - designed to sustain the loss of 2 facilities concurrently
- S3 - IA
    - Infrequently accessed
    - lower fee, charged a retrieval fee
- S3 - One Zone - IA
    - for on availability zone only
    - if data needs to be secured, don't use this
- S3 - intelligent tiering
    - optimize cost by automatically moving data to a cost effective tier
- S3 Glacier
    - secure durable and for archiving
    - retrival times can be configured from minutes -> hours
- S3 Glacier Deep Archive
    - lowest cost tier
    - retrival times of 12 hours or more is acceptable

Encryption:

- SSL/TLS - IN TRANSIT

- AT REST (server side)
    - S3 managed keys
    - AWS KMS
    - Server Side Envryption with customer SSE-C

- CLient Side encryption

Best Practices:

- MF auth on root account
- strong and complex password
- paying account should only be used for billing
- Enable/disable AWS services with Service control policies

3 different ways to share S3 buckets across accounts:

- using Bucket policies and IAM (Applied accross entire bucket): programmatic access only
- using Bucket ACLS and IAM (individual objects): programmatic access only
- Cross-account IAM roles: programmatic and console acces

Cross Region Replication

- versioning must be enabled
- files in an existing bucket are not replicated automatically
- dele markers are not replicated

Lifecycle Policies

- Automates moving your objects between different storage tiers
- can be use in conjunction with versioning

S3 Transfer Acceleration

- if you want your users to impove speed and performance to uploading files
- uses edge location

CloudFront:

- Edge Location: content will be cached
    - not just Read Only (can write as well)
    - objects are cached for the lif of the TTL
    - can clear cached objects , but will be charged
- Origin: S3, EC2, Elastic Load Balancer,Route53
- Distribution: Name vien the CDN consists of a collection of Edge locations
- Web Distribution -for websites
- RTMP - Adobe media or media streaming

Snowball

- big disk
- import export S3

Sotrage Gateway:

- File Gateway: flat files for S3 NFS
- Volume gateway
    - stored volumes: full dataset stored on site and async backed up to s3
    - cached volumes: full datased is stored in S3 and most frequently accessed data on site
- Gateway Virtual Tape library: used for backup and uses NetBackUp,Backup Exec, Veeam etc.


Athena Exam tips

- interactive query service
- allows SQL to query S3
- Serverless
- Commonly used to analyse log data stored in S3

Macie Exam Tips

- uses AI to analyze data in S3 and helps identify PII
    - address
    - name
    - personal information
- analyse CloudTrail logs for suspicious API activity
- have dashboards,reports and aletring
- PCI-DSS compliance and preventing ID theft

S3 FAQs


## EC2

Amazon Elastic Compute Cloud. Resizebale compute capacity in the cloud. Reduces time required to obtain and boot new server instaces to minutes, quickly scale up or down as your computing req change.


TIERS:

- On Demand: 
    - pay fixed rate by hour/seconds with no commitment
- Reserved:
    - capacity resrvation
    - significount discount on hourly charge per instance 
    - 1 or 3 year contract terms
- Spot:
    - bid price for instance capacity
    -  greater savings if your applications have flexible start and end times
    - if terminated by aws, you will not be charged for a partial hour usage
    - if terminated by you, charged for any hour in whcih the instance ran
- dedicated host:
    - dedicated for you
    - reduce costs by allowing you to use your existing server bound
    software licenses or regulations

EC2 Instance Types
------------------