# aws-solutions-architect-associate
This is my notes for AWS Solutions Architect Associate Exam


## 1. IAM

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

### Access Tiers:

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

### Encryption:

- SSL/TLS - IN TRANSIT

- AT REST (server side)
    - S3 managed keys
    - AWS KMS
    - Server Side Envryption with customer SSE-C

- CLient Side encryption

### Best Practices:

- MF auth on root account
- strong and complex password
- paying account should only be used for billing
- Enable/disable AWS services with Service control policies

### 3 different ways to share S3 buckets across accounts:

- using Bucket policies and IAM (Applied accross entire bucket): programmatic access only
- using Bucket ACLS and IAM (individual objects): programmatic access only
- Cross-account IAM roles: programmatic and console acces

### Cross Region Replication

- versioning must be enabled
- files in an existing bucket are not replicated automatically
- dele markers are not replicated

### Lifecycle Policies

- Automates moving your objects between different storage tiers
- can be use in conjunction with versioning

### S3 Transfer Acceleration

- if you want your users to impove speed and performance to uploading files
- uses edge location

### CloudFront:

- Edge Location: content will be cached
    - not just Read Only (can write as well)
    - objects are cached for the lif of the TTL
    - can clear cached objects , but will be charged
- Origin: S3, EC2, Elastic Load Balancer,Route53
- Distribution: Name vien the CDN consists of a collection of Edge locations
- Web Distribution -for websites
- RTMP - Adobe media or media streaming

### Snowball

- big disk
- import export S3

### Storage Gateway:

- File Gateway: flat files for S3 NFS
- Volume gateway
    - stored volumes: full dataset stored on site and async backed up to s3
    - cached volumes: full datased is stored in S3 and most frequently accessed data on site
- Gateway Virtual Tape library: used for backup and uses NetBackUp,Backup Exec, Veeam etc.


### Athena Exam tips

- interactive query service
- allows SQL to query S3
- Serverless
- Commonly used to analyse log data stored in S3

### Macie Exam Tips

- uses AI to analyze data in S3 and helps identify PII
    - address
    - name
    - personal information
- analyse CloudTrail logs for suspicious API activity
- have dashboards,reports and aletring
- PCI-DSS compliance and preventing ID theft

S3 FAQs


## 2. EC2

Amazon Elastic Compute Cloud. Resizebale compute capacity in the cloud. Reduces time required to obtain and boot new server instaces to minutes, quickly scale up or down as your computing req change.


### TIERS:

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

### EC2 Instance Types
FIGHT DR MCPXZ AU

### EBS

- Termination Protection is turned off by default
- EBS-backed instance, the default action is for the root EBS volume to be deleted when the insance is terminated
- EBS Root Volumes of your default ami can be encrypted. Can use 3rd party tool (bit locker), can be done when creating AMI's in the AWS console or API
- additional volumes can be encrypted

### Security Groups (SG)

- All inbound traffics is blocked by default
- All Outbound traffic is allowed
- Changes ot security groups take effect immediately
-  any number of Ec2 instances within a SG
- multiple security groups attached to Ec2 instaqnces
- stateful
- if you create an inbound rule allowing traffic in, that traffic is auto allowing back out again
- cannot block specific IP addresses using SG's
- can specify allow rules but not deny rules

- Network Access Control Lists (NACL) is stateless and can block specific IP addresses


### EBS types

- General Purpose SSD:
    - balance price and performance
    - most work loads
    - AKA for API: gp2
    - 1gb - 16tb
    - IOPs Max: 16000
- Provisioned IOPS SSD
    - Highest performance ssd, designed for mission criticla applciations
    - Databases
    - io1
    - 4gb - 16tb
    - 64000 IOPS
- Throughput Optimized HDD
    - low cost HDD, frequently accessed, throughput intensive workloads
    - Big data and Data warehouses
    - st1
    - 500gb - 16tb
    - 500
- Cold HDD
    - low cost HDD, less frequent accessed workloads
    - file servers
    - sc1
    - 500gb - 16tb
    - 250 IOPS
- EBS Magnetic
    - previos gen HDD
    - workloads where data is infrequently accessed
    - standard
    - 1gb - 1tb
    - 40-200 IOPS

### EBS Snapshots

- Volumes exist on EBS = virtual hard disk
- snapshots exist on S3 = photograph of the disk
- snapshots are point in time copies of volumes
- snapshots are incremental - blocks that have changed since your last snapshot are moved to S3
- first time snapshot - takes time to create

- stop instance before creating a snapshot
- can still snapshot while instance are running
- can create AMI from both the volumes and snapshots
- change  EBS volume sizes on the fly, including size and storage type
- volumes will always be in the same AZ as the ec2 isntance

### migrating EBS

- to move ec2 from AZ to AZ 
    - take snapshot 
    - create AMI
    - use AMI to launch ec2
- to move ec2 from region to region
    - take snapshot 
    - create AMI
    - copy AMI from region to region
    - use AMI to launch ec2

### EBS encryption

- snapshots of encrypted volumes are encrypted automatically
- volumes restored from encrypted snapshots are encrypted automatically
- share snapshots, only if not encrypted
- share to other aws or make it public

### Exam tips

- Root device vilumes can now be encrypted
- to encrypt if its not encrypted:
    - create snapshot of unencrypted root device volume
    - create copy of snapshot 
    - select encrypt option
    - create ami from encrypted snapshot
    - use that AMI for the ec2 isntance

### EBS vs Instance store

- instance store volumes = ephemeral storage
- instance store volumes cannot be stopped. if it fails, can lose data
- EBS backed instances can be sotpped. You will note lose data on this instance if it is stopped.
- can reboot both, you will not lose data
- both root volumes will be deleted on termination by default. EBS vol, can tell aws to keep root device vol

### encrypting root device volumes

- create snapshot of unencrypted root device volume
- create copy  and select encrypt option
- create ami form encrypted snapshot
- use AMI to launch encrypted instances

## CloudWatch

- CloudWatch is used for monitoring performances
- monitor most of aws and apps that run on aws
- CW with ec2 monitor events every 5 minutes by default
- can do 1 minute intervals by turning monitoring on
- can create CW alarms which trigger notifications

- can create dashboards to see aws environment
- set alarm to notify you when thresholds are hit
- CW events helps you respond to state changes in aws resources
- logs helps you to aggregate and monitor and store logs

CloudWatch monitors performance and CloudTrail monitors API calss in the AWS platform

## CLI

- interact with aws using command line
- set up in IAM

## Roles in EC2

- roles are more secure than access ket and secret access key on ec2 instances
- roles are easier to manage
- can be assigned to an ec2 instance after it is created using both console and command line
- roles are universal

## bootstrap scipts
- run when instance first boots
- can be powerful to automate

## instance metadata and user data

- used to get information about an instance (such as public ip)
- curl http://ip/latest/meta-data/
- curl http://ip/latest/user-data/

## EFS

- supports NFSv4
- only pay for storage use
- can scale up to petabytes
- can support thousands of concurrent NFS connections
- data is stored accross multiple AZ within a region
- read after write consistency

## EC2 Placement groups

- Clustered PG
    - low latency/ high network throughput
    - can't span multiple AZ's
- Spread PG
    - individaul critical ec2 instances
    - can span multiple AZ's
- partitioned 
    - multiple ec2 instance, HDFS HBase and cassandra
        - can span multiple AZ's

- name you specify for a PG must be unique within your aws account
- only certain type of instances cna be launched in a PG (Compute optimize, gpu, memory optimized and torage optimized)
- aws recommend homogenous instances within clustered placement groups
- can't merge PG
- can't move instance into a PG. can create AMI from existing instance, then launched from the ami into a pg

## 3. Databases

- RDS (OLTP)
    - SQL
    - MySQL
    - PostgreSQL
    - Oracle
    - Aurora
    - MariaDB
- DynamoDB (No SQL)
- Red Shift OLAP
- Elasticache
    - memcached
    - redis

Relational Databases:

- RDS runs on VMs
- cannot loging the OS
- patching of the RDS OS and DB is Amazons responsibility
- RDS is not serverless
- Aurora serverless is serverless

 Two types of Backups for RDS:

- Automated Backups
- Database Snapshots

 Read Replicas

- can be multi-AZ
- used to increase performance
- must have backups turned on
- can be different regions
- can be MySQL, PostfreSQL, MariaDB, Oracle, Aurora
- can be promoted ot master, this will break the read replica

- SQL server is not supported
- add read replicas and point ec2 instances to read replicas

MUltiAZ

- usded for DR
- you can force a failover from one AZ to another by rebooting the rds instance

### Ecryptiong in DB's

- Ecryption at rest:
    - MySql oracle sql server postgresql mariadb and aurora

- encryption is done by AWS KMS
- once encrypted , data stored at rest in the underlying storage is encrypted, as it is for the auto backups, read replicas and snapshots

### DynamoDB

- stored in SSD storage
- spread across 3 geographically distinct data
- eventual consistent reads (default), n > 1 second
- strongly consistent reads, n < 1 second

## Redshift Backups

- Enabled by default (1 day retention period)
- maximum retention period is 35 days
- always attempt to maintain at least three copies of your data  (org and replica on the compute nodes and a backup in s3)
- async replciate your snapshots to s3 in another region for disaster recovery

## Aurora

- 2 copies of your data are contained in each AZ, with minimum 3 AZ.
- 6 copies of you data
- share Aurora Snapshots with other AWS accounts
- 3 types of replicas available
    - Aurora replica
    - MySQL replicas
    - PostgreSQL replcias
- automated failover is only available with Aurora replicas
- automated backups by defaul and take snapshots with other aWS acocunts
- Aurora Serverless:
    - simple
    - cost effective option for infrequent, intermittent or unpredectable workloads

## Elasticache

- increase database and web application performance
- redis is multi AZ
- backups and restore of redis
- scale horizontally, use memcached

## 4. Route53 (DNS)

- ELBs do not have predefined IPv4 addresses, you resolve them using a DNS name
- Alias Record = telephone book with names and numbers
- Cname = can't support naked/root domain name
- chosoe alias record over a cname

### common DNS types

- SOA records
- NS records
- A records
- Cnames
- mx record
- PTR records

### Routing policies for route53

- simple routing
    - only one record with multi IP address
    - specify multi values in a record
    - returns all values to the user in a random order
- weighted routing
    - add weiths in percent (20% go to and 80% go to)
- latency based routing
    - based on user location and latency
- failover routing
    - health checks with active and passive 
- geolocation routing
    - allows customers in a region to have their region as the route
- geoproximity routing (traffic flow only)
    - can create traffic based on geographic location
- multivalue answer routing
    - same as simple except you get healthchecks

Health checks

- set health checks on individual record sets
- if record fails helth check, it will remove from route53 unitl health checks passes
- set SNS notifications to alert you if a health check failed

## 5. VPC

- VPC = logical datacenter in AWS
- consists of IGW's (or vitual private gateways (VPG)), Route tables, Network Access Control Lists, Subnets and Security Groupds
- 1 subnet = 1 availablity zone
- security groups are stateful, NACL are stateless
- no transitive peering

- when you create a VPC 
    - default route table
    - NACL
    - default Security Groups
- it won't create any subnets, nor create default internet gateway
- us-east-1a in you aws account can be completely different availability zone to us-east-1a in another aws account. AZ's are randomised
- amazon reserve 5 IP addresses within your subnets
- SG can't span VPC's

### NAT Instances

- creating a nat instance
    - disable source/destination check on the instance
- nat instances must be in a public subnet
- route out of the private subnet to the NAT instance, in order for this to work
- the ammount of traffic that NAT instances can support depends on the instance size. bottlenecking? increase the instance size
- can create high availability using autosacling groups, multiple different AZ's and a script to automate failover
- always behind a security group

### NAT Gateways

- redundant inside the AZ
- prefered by enterprise
- starts at 5gb/s and scales to 45gb/s
- no patch 
- not associated with security groups
- automate assign a public ip address
- update your route tables
- no need to disable source/destination checks

- if you have multiple AZ's and share one NAT Gateway, and when it is down, you lose access to the internet
- to create an AZ-independent architecture, create a NAT gateway in each AZ and configure your routing to ensure that resources use the NAT gateway in the same AZ


preferably use NAT gateways instead of NAT instance


### NEtwork Access Control Lists (NACL)

- VPC automatically comes with a default NACL, which allows all outbound and inbound traffic 
- can create custom NACL, by default it denies all outbound and inbound traffic, until you add rules
- each subnet in your VPC must be associated with a NACL.
    - if you don't explicitly associate a subnet, automatically associate it to the default NACL
- Block IP addresses using NACL's not SG

- associate a NACL with multiple subnets, however subnet can only associate with one NACL at a time.
    - when you associate a NACL with a subnet, the previous NACL is removed
- NACL contains numbered list of rules that is evaluated in order with lowest numbered rule
- NACL have seperate inbound and outbound rules and each rule can either allow or deny traffic
- NACL are stateless; responses to allowed inbound traffic are subjet to rules for outbound traffic and vice versa

### ELB and VPC

need a minimum of two public subnets to deploy an internet facing loadbalancer

### VPC flow logs

- cannot enable flow logs for vpc that are pered with your vpc unless the peer vpc is in your account
- you can tag flow logs
- created a flow log, cannot change its config. eg can't associate a different IAM role with the flow log

- not all IP traffic is monitored
    - traffic generated by instances when they contact the AMzon dns server
    - traffic generated by a windows instance for amazon windows licence activation
    - traffic to and from 169.254.169.254 for instance metadata
    - DHCP traffic
    - traffic to the reserved IP address for the default vpc router
- use your own DNS serve then all trafic to that DNS server is logged

### BAstion

bastion is for SSH or RDP instead of NAT gateways or NAt instances

### Bastions vs NAT Gateways/Instances

- NAT is used fto provide internet traffic to EC2 instances in a private subnets
- bastion used to securely administer ec2 instances (using ssh or rdp)
- cannot use a nat gateway as a bastion host

### Direct Connect

- directly connects your data center to aws
- useful for highthroughput workloads (ie lots of network traffic)
- if you need a stable and relatioble connection

- create virtual interface in the DC consol. this is public VI
- got to vpc console and then to vpn conncetions. create customer gateway
- create VPG
- attach VPG to the desired VPC
- select vpn connections and create new vpn connections
- slect the VPG and customer gateway
- once vpn is available, setup the vpn on the customer gateway or firewall

### Global Accelerator

- a service which you create accelerators to improve availability and performance of you apps for local and global users
- assigned two static IP addresses (or bring your own)
- can control traffic using dials. this is done within the endpoint group
- can control weighting to individual endpoints using weigths

### VPC endpoints

- enables you to privately connect your vpc to supported aws ervices and vpc endpoint services powered by privatelink without internet gateway, nat device, vpn connection, or aws DC connection.
- instances in your vpc do not reuire public ip addresses to communicate with resources in the service.
- traffic between your vpc and other service does not leave the amazon network

- endpoints are virtual devices. they are horizontally scaled, redundant and highly aiavlable vpc components that allow communication between isntances in your vpc and services without imposing availability risks or bandwidth constraints on your network traffic

two types of vpc endpoitns

- interface endpoints
- gateway endpoints
    - s3
    - dynamodb

## 6. Elastic Load Balancer

physical or virtual device that balance the load

### Types of ELB

- Applciation Load Balancers 
    - suited for HTTP and HTTPS traffic
    - Operate in Layer 7
    - Application aware = can see aplications inside
    - intelligent and can create advanced request routing, sending specified requests to specific web servers
    - Tesla Model X
- Network Load Balancers
    - suited for TCP traffic where extreme performance is required
    - Operate in Layer 4 (connection lecel)
    - Capable of handling millions of requests per second, while maintaining ultra-low latencies
    - use for extreme performance
    - Tesla Roadster
- Classic Load Balancers
    - legacy ELB
    - use for HTTP/HTTPS apps
    - level 7
    - features:
        - X-Forwarded
        - sticky sessions
    - can use strict layer 4 for apps that rely on the TCP protocol
    - not intelligent
    - dont care how its routed
    - 504 (Gateway timeout) error if apps stops responding
        - meaning app have issues
        - web server or database layer
        - identify where app is failing and scale it up or out where possible

- X-fowarded-For HEader
    - uses for forwarding the ip address of the user 

- LAB:
    - create 2 ec2 instances in two different AZ
        - defaukt vpc
        - add bootstrap script to both: with yum update and creates an html file
        - add storage 
        - tags
    - Create calssic ELB
        - can enable advanced vpc
            - should have minimum of 2 
        - use vpc
        - configure health check
        - have DNS name and no ip address
    - create app elb
        - can create routing, can view and edit rules

ELB EXAM TIPS:
- INstaqnces monitored by ELB are reported as:
    - Inservice
    - OutofService
- Health Checks check the instance health by talking to it
- have their own dns name. Never given an IP address

### Advanced ELB

#### Sticky Sessions

- allow binding a user's session to a specific ec2 instance. Ensures all requests are sent tot he same instance
- can enbale for app load balancers, but traffic are sent at Target Group Level

EXAM TIP: 
enbale or disable sticky session depending on scenario

#### Cross Zone Load Balancing

enables load balancing between AZ to AZ

#### Path Patterns

- can create listener with rules to forward requests based on the url path.
- AKA path based routing
- microservices? route traffic to multiple back end service using PBR
- eg: route general requests to one target group and requests to render images to another target group

### Autoscaling theory

#### 3 Components

1. Groups: logical component
    - webserver group
    - application  group
    - database group
2. Configuration Templates
    - have launch template or configuration for it's ec2 instances
    - can specify information:
        - AMI ID
        - INsurance type
        - key pair
        - security groups
        - block device mapping for your instances
3. Scaling Options
    - provides several ways to scale the Auto Scaling Groups
    - eg: configure a group to scale based on the occurrence of specified conditions (dynamic scaling) or schedule
    - scaling options:
        - maintain current instance levels at all times
        - scale manually
            - specify max, min or desired capacity
        - scale based on a schedule
            - time and date
        - based on demands
            - popular
            - more advanced
            - scaling policies
        - use predictive scaling
            - combination predictive and dynamic scaling 
                - proactive
                - reactive

#### Autoscaling Groups LAB

provision an auto scaling group in ec2:

- need a launch config
- advanced details with bottstrap script
- select security group

- create auto scaling group
- group size
- can put behind ELB

- can keep group as this size or scaling policies to adjust the capacity of this group
    - eg for scale policies:
        - scale between min 3 and  max 6 ec2 instances
        - target value: 80% of cpu utilization (metric type)
        - can warm up 

- can add SNS notifications

### HA architecture: word press

- Everyhting fail
- planb for failure
- use multi az and multi regions wherever you can
- diff between multi az (disaster recovery) and read replicas (performance) for rds
- scaling out ( increase instance) and sacling up ( increase insdie ec2 resources t2 -> t6) 
- cost element


### Cleaning up

- simulated failover with rds, can force to failover by reboot

### CloudFormation LAB:

Automate creation 

- create a stack or use a template
- specify details
- options
- review

- can take time
- did not create rds (database)

- a way to completely scripting your cloud environment
- quick start is a bunch of cloudformation templates already build by AWS solutions architects - allows complex environments created quikcly

### ElasticBeanstalk LAB

- create cloud environment by UI (not scripting)
 - can upload code

- quickly deploy and manage apps in aws cloud (don't have to worry about infrastructure that run thos apps)
- simply upload your app, and it automatically handles"
    - details of capacity provisioning
    - load balancing
    - scaling
    - app health monitoring


## Applications

### SQS

- access to a message queue that can be used to store messages while waiting for a computer to process them

- distributed queue sytem that enables web service applications to qquickly and reliably queue messages that one component in the application generates to be consumed by another component

- a queue is a temporary repo for messages that are awaiting processing

- can decoubple the components of an application so they run independetly, easing managedment between components

- any acomponent of a distributed application can store messages in a faill-safe queue

- can contain up to 256 kb of text in any format
- any component can later retrieve the messages programmatically using the amazon SQS API


- acts as a buffer between component producing, saving data and the component recieving the data for processing
    - this means the queue resolves issues that arise if the producer is producing work fast than the consumer can process it, or if the producer or consumer are only intermittently connected to the network

Two types queues
- standard queues
    - default
    - nearly-unlimited number of transactions/s. 
    - gurantee that a message is delivered at least once
    - more than one copy of a message might be delivered out fo order
    - provide best-effort ordering, ensures that it is delivered in the same order as they are snet (not guranteed)

- FIFO
    - no duplicates
    - exactly-once processing
    - support message groups that allow multi ordered message groups within a single queue
    - limited transaction 300 TPS, but have all the capabilities of standard queues

 - exam tips
    - sqs is pull based, not pushed-based
    - 256 kb size = message
    - kept in queue from 1 minute - 14 days; default = 4 days

    - visibility timeout (max 12 hours); make it longer if process is taking longer
    - long polling - retrieve messages from sqs queues
        - doesn't return a response until there is something in the queue or a time out
    - short polling returns immediately (even if queue is empty)
    
### SWF - work flow

- SWF, workflow executions can last up to 1 year (retention period)
- task-orient API, sqs message-oriented API
- swf - assigned only once and is never duplicated
- swf keeps track of all tawsks and events i application
- swf actors
    - workflow starters
    - deciders
    - activity workers

### SNS

- instantaneous, push based delivery (no polling)
- simple apis and easy integration with apps
- flexible message delivery over multi transpoiort protocols
- inexpensive, pay as you go with no up front cost
- web based aws management console wioffers the simplicity of a point and click interface
- 

### Elastic Transcoder

- media transcoder in the cloud
- converts files from original source format to diff formats that will play on smartphones, tablets, pcs etc

### API gateway

- gate wat to aws resouces
- caching to increase pergformance
- low cost and scales auto
- throttle to prevent attacks
- log results to cloudwatch
- enable CORS for javascript / ajax

### kinesis

- streams
    - data persistance: store 24 hours can go up for longer
- firehose
    - real time analytics: analyse data in real time and find somewhere to store it eg s3 ec2.
- analytics
    - uses both streams and firehose

### Cognito

- Federation allows users to authenticate with the Web Identity Provider
- user authenticates first with WIP and recieves auth token, which is exchanged for a temp aws credentials allowing the, to assume an IAM role
- Identitty broker which handles interaction between your apps and WIP

- user pool
    - user based
    - handles registration
    - authentication and account recovery
- identity pool
    - authorise access to your aws resources

## Serverless

## traditional vs serverless

traditional
user -> elb -> ec2 instances -> rds

bottlenecking at rds, unless if you are using aurora

serverless
api gateway -> lambda -> dynamodb

best way to architect and sot saving -> serverless 

### Lambda

- lambda scales out (not up) automatically
- independent 1event = 1funciton
- serverless
- can trigger other lambda functions, 1 event can trigger x functions
- aws x ray can debug what is happening
- can do thing globally, back uo s3 buckets 
- know your triggers





