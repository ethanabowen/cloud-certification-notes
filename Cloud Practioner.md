# Domains of the test
Domain 1: Cloud Concepts
  * Define the AWS Cloud and its value proposition
  * Identify aspects of AWS Cloud economics
  * List the different cloud architecture design principles
  
Domain 2: Security and Compliance
  * Define the AWS shared responsibility model
  * Define AWS Cloud security and compliance concepts
  * Identify AWS access management capabilities
  * Identify resources for security support
  
Domain 3: Technology
  * Define methods of deploying and operating in the AWS Cloud
  * Define the AWS global infrastructure
  * Identify the core AWS services
  * Identify resources for technology support
  
Domain 4: Billing and Pricing
  * Compare and contrast the various pricing models for AWS
  * Recognize the various account structures in relation to AWS billing and pricing
  * Identify resources available for billing support

On exam day:
We recommend logging into your account 30 minutes early to start the check-in process and to allow for any troubleshooting. If you are more than 15 minutes late after your scheduled exam time you will be unable to begin your exam and are unlikely to receive a refund.
To log into your account:

Log in to your AWS Certification Account
On the homepage, click ‘Manage Pearson VUE Exams’, you are taken to the Pearson VUE dashboard
Click on your scheduled exam under 'Purchased Online Exams'
Click “Begin Exam” and follow the on-screen prompts to complete the check-in process
Once you have completed the check-in process you will be contacted by a Proctor to begin your exam

# <u>Cloud Foundations</u>
##### As A Service
* **IaaS** - Managed Physical Hardware and Hypervisor (software that runs the OS).  etc EC2
* **PaaS** - Managed Physical Hardware, Hypervisor, and  OS/Runtime.  etc Elastic Beanstalk
* **SaaS** - Everything is managed by the provider, only consuming software

##### Cloud Deployment Models
* Private Cloud - dedicated infrastructure and data center.  Ex.) VMware, Microsoft, Redhat, OpenStack
* Public Cloud - Hosted and delivered from a 3rd-party (typically over the internet).  Ex.) AWS, Microsoft Azure, Google Cloud Platform
    * Variable expense, instead of capital expense
    * Economies of scale - savings on large scale projects
    * Massive elasticity - grow to any size
* Hybrid Cloud - A mix of public and private clouds
  * Keeps critical sensitive applications/data in house while non-critical applications/data can be put in the public cloud
* Multicloud - Two or more clouds 

##### Legacy IT
* Large upfront cost for infrastrucure
* Operations within the organization is a big expensive
* Operations costs don't go down as the company scales down

##### Size advantages of cloud computing
| # | Name | Description
|---|---|---|
| 1 | Trade **capital expense** for **variable expense** | Instead of investing in data centers before you know how you're going to user them, pay only when, and for how much, you consume 
| 2 | Benefit from massive **economies of scale** | Achieve a **lower variable cost** due to AWS' **scale** |
| 3 | Stop guessing about capacity |Eliminate guessing, **scale as demand dictates** |
| 4 | Increate speed and agility | Easily and **quickly scale** your usage |
| 5 | **Stop spending money** running and maintaining data centers | **Focus on business growth** and innovation instead! |
| 6 | Go global in minutes | Easily **deploy applications in multiple regions **around the world |

### Amazon Web Services (History)
* 2002 - Launched internally
* 2003 - Vision casted for public offering
* 2004 - SQS public launch
* 2006 - Re-launch with S3, SQS, EC2
* 2010 - All amazon.com retail sites miratred to AWS
* 2015 - Sales $1.57 billion!
* 2018 - Revenue of $25 billion
* Today - Over 165+ services in Computing, Storage, Networking, Database, Analytics, Media services, ,chine Learning, Management, Mobile, IoT all in **22 Geographical regions** of presense

### AWS Global Infrastructure
|Name|Description|
|---|---|
|Region| A geographical area with **2 or AZs**, isolated from other AWS regions |
|Availability Zone (AZ)| One or more data centers that are **physically separate and isolated from other AZs**.  Each AZ is designed as an independary failure zone and use discrete power sources on seperate grids |
|Edge Location| A location with **cache of content that can be delivered at low latency** to users - used by CloudFront |
|Regional Edge Cache| Also part of the CloudFront network. These are **larger caches that sit between AWS services and Edge Locations** |
|Global Network| **Highly available, low-latency** private global network **interconnecting every data center, AZ, and AWS region** |


### AWS Services in Scope (for exam)
##### Legend: <span style="color:red">Red - Global Service </span> | White - Regional Service
  
* **AWS IAM -** <span style="color:red">Identity and Access Management</span>
* **AWS Compute** - EC2, EC2, Lambda, LightSail
* **AWS Storage** - <span style="color:red">S3</span>, EBS, EFS, Storage Gateway
* **AWS Networking** - VPC, <span style="color:red">Direct Connect</span>
* **Databases** - RDS, DynamoDB, Redshift, ElastiCache
* **Elastic Load Balancing and Auto Scaling** - Elastic Load Balancing, Auto Scaling
* **Content Delivery and DNS Services** - <span style="color:red">Route 53, CloudFront</span>
* **Monitoring and Logging Services** - CloudWatch, CloudTrail
* **Notification Services** - SNS
* **Migration and Transfer** - Database Migration Service, Server Migration Service, Snowball
* **Cloud Governance and Security** - GuardDuty, KMS, <span style="color:red">WAF & Shield</span>, CloudHSM, <span style="color:red">Artifact</span>, Inspector, <span style="color:red">Trusted Advisor</span>, AWS Config, AWS Service Catalog, <span style="color:red">Personal Health Dashboard</span>


## Three Fundamentals of AWS Pricing
* Compute - pay for virtual server + CPU cycles
* Storage - pay for the amount of data you have stored
* Outbound data - pay for the amount of data you pull out (charged at the outbound data transfer rate)

* On-demand
  * Used for Compute and Database capacity
  * No long-term commitments or upfront payments
* Dedicated Instances
  * Available for Amazon Ec2
  * Hardware is dedicated to a single customer
* Spot instances
  * Purchase spare capacity with no commitments
  * **Great discounts for hourly rates**
* Reservations
  * Up to 75% discounts (with 1 or 3 year term), No/Partial/All upfront cost options
  * Availabe for these services: EC2, DynamoDB, ElastiCache, RDS, RedShift

```code
AWS Acceptable User Policy - study it.
```


## <u>IAM</u>
**Authentication**
* Access key and Secret key combined provide programatic Access (CLI, SDK, etc)
  * MFA can be added on
  * These keys can be rotated, but the secret key is only available at the time of creation
  * Users CAN change their own keys **through IAM policy (not from the console)**
  * These keys can be disabled at any time by administrative means
* Tradiational sign in with user/pass in AWS Console
  * MFA can be added on.
  * Users can change their own passwords
  * The ability to give only selected users password changing abilities exists.  
    * First disable the password changing ability for all users via an IAM Policy.
    * Then grant the password changing ability to the selected users you wish to have it.
* Signing Certificate (not very often)
  * SSL/TLS certificate that can be used to authenticate with some AWS services.
  * AWS Certificate Manager (ACM) is recommended for provisioning, managing and deploying your server certificates
  * Use IAM when you must support HTTPS connections in a region that is not supported by ACM (less and less common)
  * 
**MFA (Multi-Factor Authentication)** - A combination of something you know (password, secret), have (phone, usb), or ARE (biometrics)

### Best Practices
* Lock away the AWS root user access keys
* Create individual IAM users, do not share amongst multiple persons
* Use AWS defined policies to assign permissions whenever possible
* User groups to assign permissions to IAM users, will help relieve the clutter of user -> policy assignments
* Least Privilege approach
* User access level to **review** IAM permissions
* Configure strong password policy for users
* Enable MFA for **ALL privileged** users
* **Use roles for applications that run on AWS EC2 instances**
  * Never put User/Pass/Keys into code [on EC2 instances]
* Use Roles over sharing credentials
* Rotate credentials regularly (this is hard to do in practice)
* Remove unnecessary credentials
* Use policy conditions for extra security (ex. certain computer coming from certain ip address)
* Monitor activity in your AWS account (tools to use later in the course)

## <u>STS (Security Token Service)</u>
* Allows for the abilit to request temporary, limit-privilege credentials for IAM users or for users that you authenticate (federated users)
* Global service https://sts.amazonaws.com
* All regions are eabled by default, but can be disabled
* The region in which temporary credentials are requested muist be enabled
* Credentials will **always work globally**

# AWS Compute

## <u>EC2 (Elastic Compute Cloud)</u>
Web Service providing secure, resizable compute capacity in the cloud

* IaaS (Infrastructure)
* Composed of CPU, RAM, Disk, NIC and OS (**Linux**/Windows/etc)
* Instance - Individual Virtual Private Server
  * AMI (Amazon Machine Image) - an **EBS (Elastic Blockstore Volume) Snapshot** (which has an OS installed on it)
  * Type - scale of resources (small, medium, large, xlarge, etc)
* Elasticity - increase or decrease capacity within minutes, commission 1-1000+ instances simultaneously
* Complete control (including root access) to each instance
  * No loss of data on start/stop
  * Ability to communicate to via API
* Flexible Hosting Services - preconfigued images based on configurable options (OS, hardware, storage, etc)
* Scale Vertically (CPU/Mem/HDD)
* Scale Horizontally (automatically) with Auto Scaling
* Used for Traditional Applications that run for long periods of time (month/years)
* Pay for instance run time based on family/type (resources allocated) and data provisioned
* Only 1 IAM Role can be applied per instance
  
Instance data - Configuration data about your instances can be found and queried at http://169.254.169.254/latest/meta-data
User data - 16kb max of non-encrpyted user supplied data, commands run when the instances launchs

## <u>AMI (Amazon Machine Image)</u>
Provides the information required to launch an instance
* One or more EBS snapshots (a copy of an EBS volume)
* Permission settings associated with the launching the AMI
* A device mapping that specifies the volumes to attach to the instance when launched
  
Type:
* Community AMIs - free to use, typically just select the OS you want
* Marketplace AMIs - pay to use, usually somes with additional licensed software
* My AMIs - AMIs that you create yourself

[Instance Types](https://aws.amazon.com/ec2/instance-types/)


## <u>ECS (Elastic Container Service)</u>
* CaaS (Container)
* Provides a **highly scalable, high performance container management service** that **supports Docker container**
* Container - a virtual instance, OS is not apart of it
  * Contains Code, runtime, system tools/libraries/settings
* Eliminates the need for own implementation of cluster management
* There are 2 launch types:
  |EC2|Fargate|
  |---|---|
  |Explicitly Provisioned Instances|(Control plane) **Resources are automatically provisioned**|
  |**Responsible for upgrading and patching**|Provisions compute as needed|
  |**Manually** handle cluster optimization|**Automatically** handles cluster   optimization|
  |**More granular control over infrastructure**|**Limited control, as infrastructure is automated**|
  |Best for microservice and batch use cases where you need containers and have the need to maintain the underlying platform|Same, but best is there is no need for maintaining an underlying platform|
  |Pay for instance run time based on family/type|Pay for container run time based on allocated resources|
* Elastic Container Registry - Docker registry service used by ECS
  
## <u>Lambda</u>
* FaaS (Function)
* Serverless, allowing you to run code without provisioning or managing servers
* Typically executed off of a trigger.  Ex. API call, scheduled run, another lambda?, etc
* Pay only for compute time, this is the big payoff of serverless
* Benefits of Lambda:
  * No servers to manage
  * Continuous scaling
  * Subsecond metering
  * Integrates with almost all other AWS services
* Primary use cases:
  * Data processing
  * Real-time file/stream processing
  * Building a serverless backend for web, mobile, IOT, and 3rd party API requests
* Auto Scaling up to 1000 (default limit) concurrent executions
* Best use cases - ETL, infrastructure automation, data validation, mobile backends
* Pay only for execution time (based on memory allocation, which is related to CPU size)

## <u> LightSail </u>
* Best for users who do not have deep AWS technical expertise
* Makes it very easy to provision computer services
* Basically it bundles compute into simple use-cases for a 'low barrier to entry'
* Provisions
  * Compute - EC2
  * Databases - MySQL, PostgreSQL
  * Storage - High Availability
  * Elasticity - Load Balancing


# AWS Storage

### <u>Storage System types</u>
* Object Storage (S3)
  * Manage data as individual objcts, rather than as blocks and sectors (block-based) or a file hierarchy (file-based)
  * Accessed via REST API (GET, PUT, POST, DELETE, etc)
  * Each object contains the data itself, metadata, and a **globally unique identifier** (this is a constraint of S3)
  * Virtually unlimited scalability to due flat file structure
* Block Storage (EBS - Elastic Block Storage)
  * Manage data in blocks within sectors and tracks and is controller by a server-based operating system
  * Appears as local disks to the OS and can be partitioned and formatted
  * **You can use block storage devices as a boot volume**
  * Use Cases - Structured information such as a file system, databases, transactional logs, SQL databases and virtual machines (VMs)
  * EC2 can be attached to an EBS volume which has the OS.  The OS runs and will connected over a network.
* File Storage
  * Manage data in file structures (hierarchy)
  * Mounted via the network to a client computer where it then becomes accessible for reading and writing data
  * Protocols used for accessing file systems include NFS or (Microsoft) CIFS/SMB


## <u>S3 (Simple Storage Service)</u>
* Object storage system built to store and retrieve any amount of data from anywhere
* Public service - everyone connects to it over the public internet
  * Edge case - S3 Gateway Endpoint can allow for an EC2 instance to connect to S3 without going to through Public Internet 
* Any type of file supported
* S3 is designed to deliver 99.99999999999 (11) durability
* Use Cases
  * Backup and Storage
  * Application Hosting - deploy, install, and manage web applications
  * Media Hosting - upload/download style service of any type of media
  * Software Delivery - customer download portal for your applications
  * Static Website - run static page straight from an S3 bucket!
* Buckets are root level folders
* Files are stored in buckets with a 5TB limit per file
* Unlimited storage - charged for storage
* **S3 is a universal namespace** so bucket names must be unique globally
* However, you **create your buckets within a REGION**
* It is a best practice to create buckets in regions that are physically closest to your user's to reduce latency
* Objects consist of:
  * **Key** - name of the object
  * **Value** - data made up of a sequence of bytes
  * Version ID - changes on each upload?
  * Metadata - information relating to the data that is stored
* Pricing:
  * Storage - pay for what you put in
  * Requests - Retrieve data
  * Storage Management
  * Data transfer
  * Transfer acceleration - way to accelerate the upload of data to S3

Storage Class
* **Standard** - durable, immediately available, **frequency accessed**
* **Intelligent-Tiering** - automatically moves data to the most cost-effective tier
* **Standard-IA** - durable, immediately available, **infrequency accessed**, lower cost than Standard, pay retrival fee
* **One Zone-IA** - lower cost for infrequently accessed data with LESS resilience (99.5%, due to non-replicated data accross multiple availability zones)
* **Glacier** - archieve data, retrieval times in minutes or hours
* **Glacier Deep Archive** - lowest cost storage class for long term retention

|S3 Capability|How it works|
|---|---|
|Transfer Acceleration| Speed up data uploads using CloudFront in reverse|
|Requester Pays|The requester rather than the bucket owner pays for requests and data transfer|
|Tags|Assign taghs to objects to use in costing, billing, security, etc|
|Events|Trigger notifications to SNS, SQS, or Lanbda when certain events happen in your bucket|
|Static Web Hosting|Simple and massively scalable static website hosting|
|BitTorrent|Use the BitTorrent protocol to retrieve any publicly available object by automatically generating a .torrent file|

Versioning - sounds like what it is, if Versioning is suspended, it prevents NEW VERSIONS from being uploaded, all uploads will be under the latest version.  Deleting the object creates a new version.

CRR - Cross-Region Replication
  * Source and destinations must have versioning enabled
  * Can be the same or different account
SRR - Same-Region Replications
  * same as CRR, just in the same region

Why replication?
  * Compliance requirements
  * Backups into another region
  * Copy to another region for closer access for users
  * Copy into a new storage tier


## <u>EBS (Elastic Block Store)</u>
* Provides persistence block storage, even if associated (EC2) instance is detacted and terminated
* **Replicated within its AZ**
* No need for an EBS to be attached an instance, can be zombied with no problem
* **1:M -> EC2 Instance:EBS Volumes**
* * Multiple EBS volumes can be attached to the same instance
  * Can not attach EBS volume to multiple instances (use Elastic File Store instead)
* EBS is AZ contrained, meaning the instance has to be in the same AZ
* **Termination protection is OFF by default** and must be manually enabled (keeps the volume/data when the instance is terminated)
  * Root EBS volumes are deleted on termination by default
  * Extra non-boot volumes are not deleted on termination by default
* **SSD is the default boot type for EC2 instances**
* Root and non-root volumes CAN be encrypted
* **"Virtual hard disk in the cloud"**

### EBS Volume Info:
|Type|Description|Use Cases|Size|MAX IOPS/ Volume|Max Throughout/ Volume|
|---|---|---|---|---|---|
|EBS Provisioned IOPS SSD (io1)|**Highest Performance** designed for latency-sensitive transactional workloads|I/O-Intensive NoSQL and relational databases|4GB-16TB|64,000|1,000MB/s|
|EBS General Purpose SSD (gp2)|General Purpose, balances price performance for a wide variety of transactional worklaods|Boot volumes, low-latency, interactive apps, dev & test|1GB-16TB|16,000|250MB/s|
|Throughput Optimized HDD (st1)|Low cost, designed for frequently accessed, throughput intensive workloads|Big data, data warehouses, log processing|500GB-16TB|500|500MB/s|
|Cold HDD (sc1)|**Lowest Cost** designed for less frequently accessed workloads|Colder data requiring fewer scans per day|500GB-16TB|250|250MB/s

### EBS Snapshot
* Point-In-Time state backup of EBS Volume
* Stored as object in S3
* **Each new snapshot is a delta (incremental) of the previous**
  * Only the latest is needed to restore the volume, which is CRAZY COOL!
* Snapshots can only be access through the EC2 APIs
* EBS volumes are AZ specific, but snapshots are region specific
  * An EBS Snapshot is a way to migrate your EBS Volume to another AZ in the created region.  The flow for that is Existing Volume -> Snapshot (S3) -> Create AMI from Snapshot -> Launch new EC2 Instance pointing to new AMI

### Instance Store Volume
* Physically attached to the Server the EC2 instance is running on
* When the EC2 instance is shutdown/terminated, the Instance Store Volume is not persisted (Ephemeral)
* High Performance local disks
* Good for temp dat - buffer, cache, scratch data, etc.
* ISV root devices are created from AMI templates stored on S3
* ISV cannot be detached/reattached

## <u>EFS (Elastic File System)</u>
* **Network based**, scalable file storage system, mounted as a file system
* NFS Format - Network File System Format (v4.1 protocol)
* **Multiple EC2 Instances can connect to the same File System**
* On-prem solution compliant
* **Linux ONLY**
* Use Cases
  * Big Data and Analytics
  * Media processing Workflows
  * Content Management
  * Web Serving
  * Home Directories
* **Data is stored across multiple AZ's within a region**
* Read after write consistency (Opposite of Eventual Consistency)
* Pay for what you use
* On-premises access can be enabled via Direct Connect or AWS VPN
* After a EFS is attached to an instance, the EFS needs to be mounted via CLI (or attached through console at creation, maybe can be updated in console as well)

## <u>Storage Gateway</u>
* Hybrid cloud storage service that gives you **on-premises access to virtually unlimited cloud storage**
* Seamlessly connects on-premises applications to cloud storage, **caching data locally for low-latency access**
* Types of Gateway:
  * Tape - virtual tape library, can be used with popular backup software.
  * File Gateway - accessed using SMB (Microsoft), NFS or S3 API protocols.
  * Volume Gateway - block device access using iSCSI (look into)
    * iSCSI stands for Internet Small Computer Systems Interface. iSCSI is a transport layer protocol that works on top of the Transport Control Protocol (TCP). It enables block-level SCSI data transport between the iSCSI initiator and the storage target over TCP/IP networks.


## <u>AWS VPC (Virtual Private Cloud)</u>
A VPC is a logically isolated (your AWS account only) portion of the AWS cloud **within a region**.
* A VPC spans all the AZs in the region and exist by default (Subnets as well)
  * 5 per region maximum
* A VPC has a different address block called a different **CIDR (Classless InterDomain Routing) block**, a list of addresses that VPC can use
* A Subnet is a subset of those VPC addresses
  * Has a block of IP addresses from the VPC CIDR block
  * Subnets are created within AZs
* All traffic in and out of the VPC goes through the Internet Gateway, which uses a Router (that references the 'Main Route Table')
* A VPC provides complete control over the virtual networking environment including selection of IP ranges, creation of subnets, and configuration of route tables and gateways
* When you create a VPC, you must specify a range of IPv4 addresses for the VPC in the form of a CIDR block, for examples 10.0.0.0/16
 * You have full control over who has access to the AWS resources inside your VPC

How to securely connect?
## <u>AWS Managed VPN</u>
  * Fast to setup
  * Encrypted VPN connection between external **customer gateway to owned VPN gateway**
  * Not guaranteed on performance
## <u>AWS Direct Connect</u>
  * High bandwidth, low-latency but takes weeks to months to setup
  * On-prem and Cloud connection, typically creates a hybrid cloud
  * Network service that provides an alternative to using the internet to connect a customer's on premise sites to AWS
  * **The connection is private**
  * Charged by port hours and data transfer, Available in 1Gbps and 10Gbps or through AWS Direct Connect Partners
  * Guaranteed performance
## <u>VPN Cloud Hub - used for connection multiple sites to AWS</u>
* Software VPN - use  3rd party software
  
## <u>ACL (Access Control Lists)</u>
* **Subnet level firewall that controls inbound and outbound traffic**
* **Allow and deny rule (White and Black lists)**
* **Stateless**, checks rules sequencially until all pass or single fail 
  
## <u>Security Group</u>
* **Instance level firewall that controls inbound and outbound internet traffic permissions on a GROUP of resources**
* Can be used in multiple subnets, because it's just traffic rules
* **Allow rules only (White list)**
* **Stateful**, checkes all rules on each request

## <u>NAT Gateway  (Network Address Translation)</u>
* Translations public ips to private ips
* VPC associated
* Used by updating route table so that private subnet route table goes to NAT Gateway id
* Managed by AWS

## <u>Nat Instance</u>
* EC2 Instance from a specific AMI
* Same functionality of a NAT Gateway
* Managed by you
* Can use as a bastion host, a way to get to the private subnet via this ec2 instance.  Typically for 'management/devops reasons'

## <u>Transit Gateway</u>
* **Connects N-number of VPCs and on-premises networks through a central hub**
* Simplifies networks and puts an end to complex peering relationships
  * Peering relationships are not transitive between VPCs
* Acts as a CLOUD ROUTER
* Inter-Region peering connects AWS Transit Gateways together using the AWS global network (AWS's private internet)
* Data is automatically encrypted and never travels over a public network

# AWS Databases

|Data Store|Use Case|
|---|---|
|Database on ECS| Need full control over instance and database <br> Third-party database engine (not available in RDS)|
|Amazon RDS| Need traditional relational database <br> e.g. Oracle, PostgreSQL, Microsoft SQL, MariaDB <br> Data is well-formed and structured|
|Amazon DynamoDB| NoSQL database <br> In-memory performance <br> High I/O needs <br> Dynamic scaling|
|Amazon RedShift| Data warehouse for large volumes of aggregated data|
|Amazon ElastiCache| Fast temporary storage for small amounts of data|

### Operational vs Analytic
|Operational/Transational|Analytical|
|---|---|
|Online Tranaction Processiong (OLTP)|Online Analytics Processing (OLAP) - the source data ceomf rom OLTP DBs|
|Production DBs that process transactions (typically customer oriented-db)|Data warehouse or Analytics focused.  Data is extracted for decision making|
|Short transactions and simple queries|Long transactions and complex queries|
|Relational examples: Amazon RDS, Oracle, IBM DB2, MySQL|Relational Examples: Amazon RedShirt, Teradata, HP Vertica|
|Non-relational examples: MongoDB, Cassandra, Neo4j, HBase|Non-relational examples: Amazon EMR (uses Hadoop), MapReduce|

## <u>Amazon Relational Database Service (RDS)</u>
* RDS is a managed service that makes it easy to set up, operate, and scale a relational database in the cloud
* RDS uses EC2 instances, so you must choose and instance family/type
* Relational database are knowm as Structured Qery Languages (SQL) databases
* RDS is in OLTP database
* Easy to setup, highly available, fault tolerant, and scalable
* Common use cases include online stores and banking systems
* You can encrypt your RDS instances and snapshots at rest by enabled the encrpytion option for your Amazon RDS DB instances
* Encryption uses AWS Key Management Services (KMS)
* Point-in-time recovery with a granularity of 5 minutes.  Uses logs which it uploads to Amazon S3 to do this.
* **Scales vertically, not Horizontally**

### Types and details
* SQL Server
* Oracle
* MySQL Server
* PostgreSQL
* Aurora
* MariaDB
* Scalability: Can only be scaled up by increasing instance size (computer and stoage)
* Fault tolerance / disaster recovery with Multi-AZ option
* Automatic failover for Multi-AZ option

## <u>Amazon Aurora</u>
* RDS database, created by Amazon
* MySQL and PostgreSQL-compatible relational database built for the cloud, up to 5x faster than standard PostgreSQL databases
* Features a distributaed, fault-tolerant, self-healing storage system that auto-scales up to 64tb per database instance
* RDS Read Replicas are used for scaling read performance, not for disaster recovery, can help offset traffic on RDS Master db

## <u>Amazon DynamoDB</u>
|DynamoDB Feature|Benefit|
|---|---|
|Serverless|Fully managed, fault tolerant, service|
|Hightly available|99.99% availability SLA|
|NoSQL type of database with Name/Value structure|Flexible scehma, good for when data is not well structured or unpredictable|
|Horizontal scaling|Seamless scalability to any with push button scaling or Auto Scaling|
|DynamoDB Accelerator (DAX)|Fully managed in-memory cache for DynamoDB that increases performance (microsecond latency)|
|Backup|Point-in-time recovery down to the second in last 35 days; On-demany backup and restore|

* Fully managed NoSQL database service that provides fast and predictable performance with seamless scalability
* Push button scaling means that you can scale the DB at any time without incurring downtime
* Data is syncchronously replicated across 3 facilities (AZs) in a region
* Global tables provides a fully managed solution for deploying a multi-region (replicated across the world), multi-master (read/write to several masters at once, cross AZs) database
* DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory cache for DynamoDB that delivers up to a 10x performance improvement

## <u>Amazon Redshift</u>
* Fast, fully managed data warehouse that makes it simple and cost-effective to analyze all your dat using standard SQL and existing BI tools
* SQL based data warehouse used for analytics applications
* Relational database that is used for OLAP user cases
* Ideal for processing LARGE amounts of data for BI
* Uses EC2 instances, so you must choose an instance family/type
* Uses columnar data storeage - enhances performance, common data structure for analytic-oriented databases
* Always keeps three copies of your data
* Provides continuous/incremental backups

Under the hood: Leader Node(s?) coordinates query execution to compute nodes.  Ingestion, back, restore on S3

## <u>Amazon ElastiCache</u>
* Web service that makes it easy to deploy and run Memcached or Redis protocol-compliant server nodes in the cloud
* In-memory caching provided by ElastiCache can be used to significantly improve latency and throughput for many read-heavy application workloads or compute-intensive workloads
* Run on EC2 instances, so you muct choose an instance family/type

|Use Case|Benefit|
|---|---|
|Web session store|In cases with load-balances web servers, store web session information in Redis so if a server is lost, the session info is not lost, and another web server can pick it up|
|Database caching|Use Memcached in fribt of AWS RDS to cache popular queries to offload work from RDS and return results faster to users|
|Leaderboards|Use Redis to provide a live leaderboard for millions of users of your mobile app|
|Steaming data dashboards|Provide a landing spot for streaming sensor data on the factory floor, providing live real-time dashboardd displays|

* Two types of ElastiCache engines:
  * Memcached - simplest model, can run large nodes with multiple core/threads, can be scaled in and out, can cache objects such as DBs
  * Redis - complex model, supports encryption, master /slave replication, cross AZ (HA), automatic failover and backup/restore

|Memcached|Redis|
|---|---|
|Simple, no-frills|You need encryption|
|You need elasticity (scale out and in)|You need HIPPA compliance|
|You need to run multiple CPU core and threads|Support for clustering|
|You need to cache objects (e.g. database queries)|You need complex data types|
||You need HA (replication)|
||Pub/Sub capability|

# Content Delivery and DNS Services

## <u>Route 53</u>

## <u>Amazon CloudFront</u>

## <u>AWS Global Accelerator</u>

# Monitoring and Logging Services

## <u>CloudWatch</u>
* Monitoring service for AWS cloud resources and the applications you run on AWS
* Can create custom metrics
* **CloudWatch is for PERFORMANCE monitoring** (CloudTrail is for auditing)
* Used to collect and track metrics, collect and monitor log files, and set alarms
* CloudWatch is a regional service
* CloudWatch Alarms can be set to react to changes in your resources
* CloudWatch Events generates events when resource states changes and delivers them to targets for processing
* CloudWatch Logs collects and centralizes logs from AWS resources
* Any log files generated by your applications
* Gain system-wide visibility into <u>resource utilization</u>
* CloudWatch monitoring includes <u>application performance</u>

## <u>CloudTrail</u>
* Web service that records activity made on your account and CAN deliver log files to an Amazon S3 Bucket
* **For AUDITING** (CloudWatch is for performance monitoring)
* CloudTrail is about logging and saving a history of API calls for your AWS account
* Provides visibility into user activity by recording actions taken on your account
* API history enables security analysis resource change tracking, and compliance auditing
* Logs API calls made via:
  * AWS Management Console
  * AWS SDKs
  * Command Line tools
  * Higher-level AWS services (such as CloudFormation)
* **Default account retention is 90 days**
* **To keep records permanently, you need to create a Trail and record events to an Amazon S3 bucket**

# Automation and Platform Services
|CloudFormation|Elastic Beanstalk|
|---|---|
|"Template-driven provisioning"|"Web apps made easy"|
|Deploys infrastructure using code|Deploys application on EC2 (Paas)|
|Can be used to deploy almost any AWS service|Deploys web applications based on Jav, .NET, PHP, Node.js, Python, Ruby, Go, and Docker|
|Uses JSON or YAML template files|Uses ZIP or WAR files (or Git)|
|CloudFormation can deploy Elastic Beanstalk environments|Elastic Beanstalk cannot deploy using CloudFormation|
|Similiar to Terraform|Similar to Google App Engine|

## <u>Cloud Formation</u>
* AWS CloudFormation provides a common language for you to describe the provision all the infrastructure resources in your cloud environment
* Cloud Formation can be used to provision a broad range of AWS resources
* **Think of CloudFormation as deploying Infrastructure as Code**

|Component|Description|
|---|---|
|Templates|The JSON or YAML text file that contains the instrcutions for building out the AWS environment|
|Stacks|The entire environment described by the template and created, updated, and deleted as a single unit|
|Change Sets|A summary of proposed changes to your stack that will allow you to see how those changes might impact your existing resources before implementing them|

## <u>Elastic Beanstalk</u>
* Focused on deploying applications on EC2 (PaaS)
* Launches in Elastic Beanstalk container
* AWS Elastic Beanstalk can be used to **quickly deploy and manage applications in the AWS Cloud**
* Developers upload their application and **EB handles the deployment details of capacity provisioning, load balancing, auto-scaling, and application health monitoring**
* Supports Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker web applications
* Integrates with VPC
* Integrates with IAM
* **Can provision most database instances**
* Allows full control of the unmderlying resources
* Code is deployed using a WAR file or Git repository

# Migration and Transfer Services

## <u>Database Migration Service (DMS)</u>
* Use to **migrate databases from on-premises to Amazon EC2 or Amazon RDS**
* Supports homogenous (e.g. Oracle to Oracle) as well as heterogenous (e.g. Oracle to Amazon Aurora).
* Data is continuously replicated while the appliction is live, minimizing downtime.
* Pay based on computer resources used during the migration and log storage
* Fully managed migration process

## <u>Server Migration Service (SMS)</u>
* Agentless services for **migrating on-premises and cloud-based VMs to AWS**.
* Source platforms can be VMware, Hyper-V or Azure.
* AWS SMS Connector is installed on the source platform.
* Server volumes are replicated (encrypted with TLS) and saved as AMIs (in EBS) which can then be launched as EC2 instances.
* Can use application groupsing and SM will launch servers in a CloudFormation stack.
* You can replicate your on-premises servers to AWS for up to 90 days (per server).
* Provides automated, live incremental server replication and AWS Console Support.

## <u>AWS DataSync</u>
* Migration of Network Attached Storage (NAS) service using either Network File Storage (NFS) or Server Message Block (SMB) protocols.
* DataSync software agent connects to on-premises NAS storage systems.
* Synchroizes data into AWS uses a Scheduled, automated ata transfer with TLS encryption.
* Destinations are Amazon S3, Amazon FSx for Windows File Server (Windows file system), Amazon Elastic File System (NFS protocol)
* Can improve performance for data transfers up to 10x faster than traditional tooling.
* <u>Permissions and metadata are preserved.</u>
* Pay per-GB transferred

## <u>AWS Snowball</u>
* With AWS Snowball (Snowball), you can transfer hundreds of terabytes or petabytes of data between your on-premises data centers and AWS S3
* Uses a secure storage devices for physical transportation
* AWS Snowball Client is software that is installed on a local computer adn is used to identify, compress, encrypt, and transfer data
* Uses 256-bit encryption (managed with the AWS KMS) and tamper-resistant enclosures with TPM
* Snowball (80TB) (50TB) "petabytes scale"
* Snowball Edge (100TB) "petabyte scale" comes with onboard storage and compute capabilities
* Snowmobile - "exabyte scale" with up to 100PB per Snowmobile

## <u>AWS Snowmobile Edge</u>
* AWS Snowball Edge is a data migration and edge computing device that comes in two options:
  * **Snowball Edge Storage Optimized** devices provide both block storage and Amazon S3-compatible object storage.
    * Used for: local storage and large scale-data transfer
  * **Snowball Edge Compute Optimized** devices provide 52 vCPUs, block and object storage, and an optional GPU for use cases like advanced machine learning and full motion video analysis in disconnected environments.
    * Used for: machine learning and processing, and storage in environments with intermittent connectivitiy (like manufacturing, industrial, and transportation) or in extremely remote locations (like military or maritime operations) before shipping them back to AWS.

# AWS Billing and Pricing
## Pricing: What you need to know
* You need to know:
  * What you get for free
  * What you get charged for
  * How you are charged - e.g. per GB, per instance hour etc.
  * Special payment options - e.g. reservations
* You don't need to know:
  * Exactly how you get charged

**Charged for - Compute, Storage, and outbound data transfer**

## AWS Pricing Models
* On-demand
  * Used for Compute and Database capacity
  * No long-term commitments or upfront payments
* Dedicated Instances
  * Available for Amazon EC2
  * Hardware is dedicated to a single customer
* Spot Instances
  * Purchase spare capacity with no commitments
  * Great discounts from hourly rates
* Reservation
  * Up to 75% discount in exchange a term commitment
  * Options for 1 or 3 year yerm
  * Options to pay:
    * No upfront
    * Partial upfront
    * All upfront
  * Available for these services:
    * Amazon **EC2** Reserved Instances
    * Amazon **DynamoDB** Reserved Capacity (reserved capacity is not EC2 instance)
    * Amazon **ElastiCache** Reserved Nodes
    * Amazon **Relational Database Services (RDS)** Reserved Instances
    * Amazon **RedShift** Reserved Nodes

## Free Stuff
* VPC - vpcs, subnets, and routing tables are free
* Elastic Beanstalk (but not the resources created)
* CloudFormation (but not the resources created)
* Identity Access Management (IAM)
* Auto Scaling (but not the resources created)
* Consolidated Billing

## Compute Pricing
### EC2 Pricing Components
  * Clock hours of server uptime
  * Instance configuration (think marketplace)
  * Instance type - CPU/Ram
  * Number of instances
  * Detailed monitoring - service that sends data to CloudWatch, every minute
    * Basic is free and on by default and that is every 5 minutes
  * Auto Scaling (resources created)
  * Elastic IP addresses (charged if allocated but not used)
  * OS and Software package could be an extra charge
### EC2 Pricing Models
  * On-demand
    * You pay for computer or database capacity with no long-term commitments or upfront payments
    * You pay for the computer capacity PER HOUR or PER SECONDS (<u>per second is linux only</u> and applies to On-Demand, Reserved and Spoy instances)
    * Recommneded for users who prefer low cost and flexibility without upfront payment or long-term commitments
    * Good for applications with shor-term, spiky, or unpredicatble workloads that **cannot be interrupted**
  * Reserved Instances 
    * Provide significant discounts, up to 75% compared to On-Demand pricing, by paying for capacity ahead of time
    * Provide a capacity resesrvation when applied to a specific AZ
    * Good for applications that have predicable usage, that need reserved capacity, and for customers who can commit to a 1 or 3-year tern
  * Spot Instances
    * Purchase spare computing capacity with no upfront commitment at discounted hourly rates
    * Provides up to 90% off the On-Demand price
    * Recommended for applications that have fliexible start and end times, applications that are only feasible at very low computer prices, and users with urgent comuting needs for a lot of additional capacity.
    * Termination of a spot instances occures with a 2 minute warning.
  * Dedicated Hosts
    * A dedicated host is an EC2 server dedicated to a single customer
    * Runs in YOUR VPC
    * Good for when you want to leverage existing server-bound software licenses such as Windows Server, SQL Server, and SUSE Linux Enterprise Server
    * Also good for meeting compliance requirements
    * Dedicated Instances
      * Run in a VPC on hardware that's dedicated to a single customer
      * Physically isolated at the host hardware level from instances that belong to other AWS accounts
      * May share hardware with other instances from the same AWS account that are not Dedicated Instances
  * Savings Plans (new November 2019, not yet on the exam)

## <u>AWS Lambda </u>
* Charges-
  * Requests - first 1Mil free, $.20 for each after
  * Duration - for 400,000 GB-Seconds free, VERY CHEAP after
* Pay only for what you user and charged based on the number of requests for functions and the time it takes to execute the code
* Prices is dependent on the amount of memory allocated to the funciton

## <u>Amazon ECS</u>
* With EC2 launch type you pay for the compute instances in your cluster
* With Fargate launch type you pay for the vCPU and memory resources that your containerized application requests

## Types of Reserved Instances
* Standard Reserved Instance
  * Some attributes, such as instance size, can be modified during the term
  * The instance family cannot be modified
  * You cannot exchange a Standard Reserved Instance, only modify it
  * CAN be sold in the Reserved Instances Marketplace
* Convertible Rserved Instance
  * Can be exchanged during the term for another Convertible Reserved Instances with new attributes including instance family, instance type, platform, scope, or tenancy
  * CANNOT be sold on the Reserved Instances Marketplace
* Scheduled Reserved Instances
  * Enable you to purchase capacity reserveration that recur on a daily, weekly, or monthly basis, with a specified start time and during, for a one-year term
  * You reserve the capacity in advance, so that you know it is available when you need it.
  * You pay for the time the instances are scheduled, even if workload doesn't run.

## Savings Plans
* Flexible pricing model that offer low prices on EC2, Lambda, and Fargate usage
* Commit to a consistent amount of usage (measured in $/hour) for a 1 or 3 year term
* Compute Savings Plans:
  * Provides the most flexibility and helps to reduce costs by up to 66%
  * Apply to EC2 instance usage regardless of instance family, size, AZ, region, OS or tenancy, and also apply to Fargate or Lambda usage
* EC2 Instance Savings Plans
  * Provide the lowest prices, with savings up to 72%
  * Commit to usage of individual instance families in a region
  * Can change your usage between instances within a family in that region

## Storage Prices
* S3
  * Storage - pricing per class, e.g. Standard or IA
  * Strage quantity - data volume store IN your buckets on a PER GB basis **(price tiers 50TB/450TB/500TB)**
  * Number of requests - the number and type of requests, e.g. GET, PUT, POST, LIST, COPY
  * Lifecycle transition requests - moving data between storage classes
  * Data transfer - data transferred out of an S3 region is charged
  * Transfer Acceleration - faster upload
  * Storage management pricing for some additional features
* S3 Glacier
  * Extremely low cost and you pay only for what you need with no commitments of upfront fees
  * Charged for requests and data transferred out of Glacier
  * "Amazon Glacier Select" pricing allows queries to run directly on data stored on Glacier without having to retrieve the archive.  Priced on amount of data scanned, returned, and number of requests initiated
  * Options for access to archives (details are irrelevent for CP exam)
    * Expedited
    * Standard
    * Bulk
* ECS
  * Volumes - volumes storage for ALL EBS volumes type is charged by the amount of GB **provisioned**
  * Snapshots - based on the amount of sapce **consumed** by snapshotes in S3.  Copying snapshots is charged on the amount of data copied across regions
  * Data transfer - inbound data transfer is free, outbound data transfer charges are tiered.
* EFS
  * Sotrage classes - Standed, Infrequent Access Storage (EFS IA)
  * Pay for the amount of sorage you use
  * Lower cost of EFS IA
  * EFS Provisioned throughput - pay extra fee for better performance

## Network Pricing
* VPC
  * VPC is free, however the following are chargeable
    * AWS Site-to-Site VPN Connection
    * AWS PrivateLink
    * Amazon VPC Traffic Mirroring
    * NAT Gateways (and Instances, though that's EC2)
  * AWS Direct Connect
    * Pay for Port Hours
    * Outbound data transfer-  1GB = $.3/hour, 10G = $2.25/hour

## Database Pricing
* RDS
  * Runs on EC2 instances, so it's about clock time and instance types
  * Clock hours of server uptime - amount of time the DB instance is running
  * Database engine, instance size and memory
  * Database purchase type - e.g. On-Demand, Reserved
  * Provisioned storage - backup is included up to 100% of the size of the DB
  * Requests - the number of input and output requests to the DB
  * Deployment type - single AZ or multi-AZ
  * Data transfer - input is free, output costs
* DynamoDB
  * Provisioned throughput (write and read)
  * Indexed data storage
  * On-demand capacity mode:
    * Pay for the data reads and writes your applicaiton makes on the tables
    * No need to specify read and write thoughput
  * Provisioned capacity node:
    * Specify the number of reads and writes per second you expect your application to require
    * Can use with Auto Scaling to automatically adjust
  * Data transfer
  * Global tables
  * Reserve Capacity
  
## CDN and DNS Service Pricing
* Amazon CloudFront
  * Traffic distribution - data transfer and request pricing, varies across regions, and is based on the edge location from which the content is served
  * Requests - the number and type of requests (HTTP or HTTPS) and the geographic region in which they are made
  * Data transfer out - quantity of data transferred out of CloudFront edge locations
  * There are some additional chargeable items such as invalidation requestions, field-level encrpytion request, and custom SSL cerificates
* Route 53
  * Monthly charge for managin hosted zones
  * DNS queries
  * Domain names

## Tools for Estimating cost and TCO
* AWS Cost Explorer
  * The AWS Cost Explorer is a free toll that allows you to view charts of your costs
  * You can view cost data for the past 13 months and forecast how much you are likely to spend over the next 3 months
  * Cost Explorer can be used to discover patterns in how much you spend on AWS resources over time and to identify cost problem areas
  * Cost Explorer can help you to identify service usage statistics such as:
    * Which services you use the most
    * View metrics for which AZ has the most traffic
    * WHich linked account is used the most
* AWS Simple Monthly Calculator
  * Helps customers and prospects estimate their monthly AWS bill more efficiently
  * You can add services in different regions
  * The calculator includes support for most AWS services and you can include additional costs such as data ingress/egress charges, data storage charges, and retrieval fees
  * It is possible to select EC2 dedicated hosts and reserved instances with various pricing models
  * Support can also be added
* Total Cost of Ownership (TCO) Calculator
  * The TCO calculator is a free tool provided by AWS that allows you to estimate the cost savings of using the <u>AWS Cloud vs. using an on-premise</u> data center.
  *  Helps avoid large capital expenditures on hardware and infrastructure
  *  Provides directional guidance on cost savings
  *  Works by you inputting cost elements of your current/or esimated on-premises data center, and comparing those cost requirements with how much it would cost on AWS
  *  What else is included in a TCO calculation?
     *  Rack infrastructure costs
     *  Virtualization software costs
     *  Facilities costs (data center space, power, and cooling)
     *  Networking costs
     *  Data transfer costs
     *  IT Labor costs - some labor costs are also required on AWS

## Support Plans
* Basic - billing and account support only (access to fourms only)
* Developer - bustiness hours support via email
  * Only one person has unlimited cases
  * Cloud Associates Engineers
  * Email support only
* Business - 24x7 email, char and phone support
  * Unlimited contacts to open unlimited cases
  * * Cloud Support Engineers
* Enterprise - 24x7 email, chat and phone support
  * Comes with a Technical Account Manager (TAM)
  * Unlimited contacts to open unlimited cases
  * Cloud Support Engineers
  * Concierge Support Team (look into)
* https://aws.amazon.com/premiumsupport/plans/ 

## Organizations and Consolidated Billing
* Organizations allows you to consolidate multiple AWS accounts into an organization that you create and centrally manage
* Available in two feature sets:
  * Consolidated Billing
  * All features
* Includes root accounts and organizational units
* Policies are aookued ti riit acciybts ir OUs (look into OUs)
* Consolidated billing includes:
  * Paying Account - independent and cannot access resources of other accounts.
  * Linked Accounts - all linked accounts are independent
* Benefites of Consolidated Billing
  * 1 bill for multiple accounts
    * Easy tracking - you can track the charges across multiple accounts and download the **combined cost and usage data**
    * Combined usage - You can **combined the usage across all accounts** in the organization **to share the volume pricing discounts** and Reserved Instance discounts.  This can result in **lower charge for your project**, deparyment, or company than with individual standalone accounts.
    * No extra fee - Consolidated billing is offered at **no additional cost**

## Resource Groups and Tagging
* Tags are key/value pairs that can be attached to AWS resources
* Tags contain metadata (data about data)
* Tags can sometimes be inherited - e.g. resources created by Auto Scaling, CloudFormation or Elastic Beanstalk.
* Resource groups make it easy to group resources using the tags that are assigned to then.  You can group resources that share one or more tags
* Reousrce groups contain general information, such as:
  * Region
  * Name
  * Health Checks
* And also specific information, such as :
  * Public & private IP addresses (for EC2)
  * Port configuratiohns (for ELB)
  * Database engine (for RDS)

## Cost Allocation Tags
* Cost allocation tags track your AWS costs on a detailed level


# Application Integration
Services that **decouple application layers**

|Service|What id does|Example use cases|
|---|---|---|
|Simple Notification Service|Set up, operate, and send notifications from the cloud (**no polling**)|Send email notification when CloudWatch alarm is triggered|
|Simple Queue Service|Message queue; store and forward patterns (message-oriented API, **polling**)|Building distributed/decoupled applications|
|Simple Workflow Service|Need to support external processes or **specialized execution logic**|Human-enabled workflows like an order fulfilment system or for procedural requests.  Note: AWS recommends that for new applications customers consider Step Functions instead of SWF|
|Step Functions|Out-of-the-box coordination of AWS service components **with visual workflow**|Order processing workflow|

# Management and Governance
## <u>AWS Config</u>
* Resource configuration inventory
* Can ensure configs match a desired state
* Provides AWS resource inventory, configuration history, and configuration change notifications to enable security and governance
* Discover existing AWS resources, **export a complete inventory of your AWS resources** with all configuration details, and determine how a resource was configured at any point in time.
* **These capabilites enable compliance auditing, securrity analysis, resource change tracking, and troubleshooting.**
* Allow you assess, audit and evaluate configurations of your AWSD resources.
* Remediation actions can automatically reseponse to non-compliant configurations (e.g. Restart EC2, Release Elastic IP, etc)

## <u>AWS Config vs AWS CloudTrail</u>
* CloudTrail records user API activity on your account and allows you to access information about this activity
* Config records point -in-time configuration details for your AWS resources as Configuraiton Items (CIs)
* Use Config to answer "What did my resouce look like?" at a point in time.
* Use CloudTrail to answer "Who mane an API call to modify this resource?"

## <u>AWS OpsWorks</u>
* Configuration Management service that provides managed instances of Cher and Puppet
* Updates include patching, updating, backup, configuration and compliance management
* Configuration changes are submitted to OpsWorks, OpsWorks just pushes the changes to your managed instance
  
## <u>AWS System Manager</u>
* View operational data about resources
* Has Compliance information
* Patch and State management
* Run Command feature is similar to user data for EC2 instances

## <u>Trusted Advisor</u>
* Helps to **reduce cost, increase performance and improve security by optimizing** your AWS environment
* Provides **real time guidance** to help you provision your resources following best practices
* Advisor **will advise you on Cost Optimization, Performance, Security, and Fault Tolerance**

## <u>AWS Service Catalog</u>
* Allows **organizations to create and manage catalogs of IT services that are approved for user on AWS**
* These IT services can include everything from virual machine images, servers, software, and databases to complete multiu-tier application architectures
* Alows you to centrally manage commonly deployed IT services
* Helps to achievce consistent governance and meet compliance requirements

## <u>AWS Personal Health Dashboard</u>
* Provides **alerts and remediation guidance when AWS is experienceing events** that may impact you
* Gives you a personalized view into the performance and availability of the AWS services underlying your AWS resources
* The dashboard displays relement and timely information to help you manage AWS events in progress
* Provides proactive notifications to help you plan for scheduled activities
* Alerts are triggered by changes in the health of AWS resources, giving you event visibility, and guidance to help quickly diagnose the resolve issues
* You get a personalized view of the status of the AWS services that power your applications, enabled you to quickly see when AWS is experienceing issues that may impact you

## <u>AWS Service Health Dashboard</u>
* Status of AWS Services across the whole world


# AWS Cloud Security and Identity 

## <u>The Shared Responsibility Model</u>
* Defines what AWS and you (the consumer of AWS services) are responsible for.  It's a shared responsibility between AWs and the Customer.
* AWS are responsible for "Security OF the Cloud"
  * AWS is responsible for protecting the infrastructure that runs all of the services offered in the AWS Cloud
  * This infrastructure is composed of the hardware, software, networking, and facilities that run AWS Cloud services
* Customers are responsible for "Security IN the Cloud"
  * For EC2 this includes network level security (NACLs, security groups), operating system patches and updates, IAM user access managemnet, and client and server-side data encrpytion
* Inherited Controls - Customers fully inherits from AWS
  * Physical and Environmental controls
* Shared Controls - Controls which apply to both the infrastrucutre layer and customer layers, but in completely separate contexts of perspectives
  * Patch Management - AWS is responsible for patching the fixing flaws within the infrastructure, but customers are responsible for patching their guest OS and applications
  * Configuration Management - AWS maintains the configuration of its' infrastructure devices, but a customer is responsible for configuring their own guest operating systems, databases, and applications
  * Awareness & Training - AWS trains AWS employees, but a customer must train their own employees
* Customer Specific - Controls which are solely the responsibility of the customer based on the application they are deploying within AWS services.  Examples include:
  * Service and Communications Protection or Zone Security which may require a customer to route or zone dat within specific security environments

Examples of Customer Responsibility:
* Staff training
* Bucket with objects
* Data encryption
* IAM Role
* IAM User
* MFA
* Network ACL
* Security Group
* SSL encrpytion
* Patch Management
* **EC2 Instances - OS updating**
* Auto Scaling
* ELB

Examples of AWS Responsibility
* Data center
* Data center security
* Network router
* Network switch
* Server
* Storage
* Disk drive
* Database Server

## <u>AWS Compliance</u>
* Enables you to understand the robust controls in place at AWS to maintain security and data protection in the cloud
* As systems are built on top of AWS Cloud infrastructure, **compliance responsibilities will be shared**
* Conpliance Programs (google it) includes:
  * Cerifications / attestations
  * Laws, regulation, policies
  * Alignments / frameworks

## <u>AWS Artifact</u>
* Your go-to, central resource for **compliance-related information** that matters to you
* It provides on-demand **access to AWS' security and compliance reports** and select online agreements
* Reports available in AWS Artifcat include
  * Service Organization Control (SOC) reports
  * Payment Card Industy (PCI) reports
  * Certifications from accreditation bodies across geopraphies
  * Compliance verticals that validate the implementation and operating effectiveness of AWS security controls

## <u>AWS Organizations Service Control Policies (SCP)</u>
* Service Control Policy can overwrite Account policies, even for an Administrator with Action: *, Resource: * 
* Can have cross-account SCPs

## <u>Amazon Inspector</u>
* Automated **security assessment service** that **helps improve the security and compliance** of applications deployed on AWS
* Automatically **assesses applications for vulnerabilities or deviations from best practices**
* Uses an agent installed on EC2 instances
* **Instances must be tagged**
* Network Assessment **doesn't** require agent to be installed
* Host Assessment **does** requires agent to be installed

## <u>AWS Web Application Firewall (WAF)</u>
* **Protects against common exploits** that could 
  * Compromise application availability
  * Compromise security
  * Consume excessive resources
* Access Control Lists used 

## <u>Shield</u>
* Managed Distributed Denial of Service (DDoS) protection service
* Safeguards web applications running on AWS with **always-on dectection and automatic inline mitigations**
* Helps to **minimize application downtime and latency**
* Two Tiers - Standard and Advanced ($3,000/month + other fees)
* Integrated with Amazon CloudFront

## <u>Amazon Macie</u>
* Fully managed **data security and data privacy service**
* User machine **learning and pattern mathing** to discover, monitor, and **help you protect your sensitive data on S3**
* Macie enables security compliance and preventive security as follows:
  * Identify a variety of data types:
    * PII
    * Protected Health Information (PHI)
    * Regulatory documents
    * API keys
    * Secret keys
  * Identify changes to policy and acces control lists
  * **Continuously monitor the security posture of Amazon S3**
  * **Generate security findings** that you can view using the Macie console, AWS Security Hub, or Amazon EventBridge
  * Manage multiple AWS accounts using AWS Organizations
* Charged by ammount of data processed

## <u>Amazon GuardDuty</u>
* Threat detection service that **continuously monitors for malicious activity and unauthorized behavior** to protect your AWS accounts and workloads
* Charged by amount of data processed
* Intelligently Analyze:
  * CloudTrail logs
  * VPC flow logs
  * DNS logs
* **Can report or remediate the threats**

## <u>Encryption in Transit vs at Rest</u>
* Transit example - SSL to SSL communication (over the web)
* Rest example - Stored data is encrypted (think EFS, S3, etc)
  
## <u>AWS KMS</u>
* Key Management Service gives you **centrailized control over the encryption keys used to protect your data**
* Encrypting you data at rest. ex. S3, EBS, EFS
* Manage the keys that encrypt your data
  * Create, import, rotate, disable, delete, define useage policies for, and audit the use of encryption keys used to encrypt your data
* **Integrated with most other AWS services** making it easy to encrpy the data you store in these services in ecryption keys you control

## <u>AWS CloudHSM (Hardware Security Model)</u>
* Cloud-based hardware security module (HSM) that **enables you to easily generate and use your own encrpytion keys** on the AWS Cloud
* KMS is multi-tenate service vs CloudHSM is **single tenant and dedicated phyical hardware module which stores the keys**
* Users FIPS 140-2 Level 3 validated HSMs
* Offers flexibility to integrate with inddustry-standard APIs, such as PKCS#11, Java Cryptography Extensions (JCE), and Microsoft CrpytoNG (CNG) libraries

## <u>AWS Certificate Manage (ACM)</u>
* Used for **creating and managing public SSL/TLS certificates** (in transit encryption)
* **Public certificates provided by ACM** (ACM certificates) or certificates that you import into ACM
* Can also request **private certificates from a private certificate authority (CA)** created using **AWS Certificate Manager Private Certificate Authority**
* ACM certificates can secure multiple domain names and multiple names with a domain
* You can also use ACM to create wildcard SSL certificates that can protect an unlimited number of subdomains

## <u>AWS Secret Manager</u>
* Helps your meet your security and compliance requirements by enabling you to **rotate secrets safely without the need for code deployments**
* Built-in integration for RDS, Redshift, Amazon DocumentDB and rotates these database credentials on your behalf automatically!
* You can customize Lambda functions to extend Secrets Manager rotation to other secret types, such as API keys and OAuth tokens

## <u>AWS Penetration test</u>
* https://aws.amazon.com/security/penetration-testing/

## Identity Providers Federation and Directory Services
## <u>Microsoft AD</u>
* Fully managed AWS services on AWS infrastructure
* **Best choice if you have more than 5000 users** an/or need a trust relationship set up
* Runs on a Windows Server
* You can setup trust relationships to extend authentication from on-premises Active Directories into the AWS cloud
* On-premise users and groups can access resources in either domain using SSO
* Requires a VPN or Direct Connect connection
* Can be used as standalone AD in the AWS cloud

## <u>Simple AD</u>
* Inexpensive Active Directory-compatible service with common directory features.
* Standalone, fully managed, directory on the AWS cloud
* Simple AD is generally the least expensive option
* **Best choice for less than 5000 users** and don't need advanced AD features

## <u>AD Connector</u>
* Directory **gateway for redirecting directory requests to your on-premise Active Directory**
* Connects your existing on-premise AD to AWs
* Best choice when you want to use an existing AD with AWS services
* You can also join EC2 instances to you on-premise AD through AD Connector
* You can also login to the AWS Management COnsole using your on-premise AD DCs for authentication

## <u>AWS Security Bulletins and Abuse Team</u>
* List of notifications of AWS' internal vulnerabilities (google Security Bulletins)

## <u>Abuse Team</u>
* Team to help you if you suspect AWS resources are being used for abusive purposes
* Google Abuse Team

# Machine Learning

## <u>AWS Rekognition</u>
* Image Analysis
* Makes it easy to add **image and video analysis to you applications using proven, highly scalable, deep learning technology trhat request no machine learning expertise to use.**
* Can identify objects, people, text, scenes, and activities in images and videos, as well as detect any inappropriate content.
* Also provides highly accurate facial analysis and facial search capability that you can use to detect, analyze, and compare faces for a wide variety of user verification, people counting, and public safety use cases.
* Rekognition Video processes a video stored in an S3 bucket
* The completion status of the request is published to an SNS topic.
* With Rekognition Video, you can track each person with a shot and through the video across shots.
* In streaming mode, you can **search faces against a collection with tens of millions of faces in real time**.
* Rekognition Video users a Kinesis Video Stream as input, to process a video stream.

## <u>Amazon Transcribe</u>
* Transribe enables you to add speech-to-text capability to applications
* Audio Transcribe users a deep learning process called automatic speech recognition (ASR) to convert a speech to text quickly and accurately
* Transcribe can be used to transcribe customer service calls, to automate closed captioning and subtitling, and to generate metadat for media assets to create a fully searchable archive.

## <u>Amazon Translate</u>
* Translate is a **neural machine language translation service** that delives fast, high-quality, and affordable language translation results
* Neural machine translation is a form of language translation automation that uses deep learning models to deliver more accurate and more natural sounding translation than traditional statistical and rule-based translation algorithms
* Allows you to localize content - such as websites and applications - for international users, and to easily translate large volumes of text efficiently

## <u>Amazon Polly</u>
* Polly is a service that **turns text into lifelike speech**, allowing you to create applications that talk, and build entirely enw categories of speech-enabled products.
* **Text-to-Speech (TTS) service** uses advaned deep learning technologies to **synthesize natural sounding human speech**.

# Analytics
## <u>Amazon Athena</u>
* Interactive query service used to **query data in S3 buckets using Structured Query Language (SQL)**
* Serverless, so there is no infrastructure to manage, and you pay only for the queries you run
* Athena uses a managed Data Catalog (AWS Glue) to store information and schemas about the databases and tables that you create for your data stored in S3
* Optimized for fast performance with S3
* With Athena, there's no need for complex ETL jobs to prepare data for analysis
* This makes it easy for anyone with SQL skills to quickly analyze large-scale datasets
  
## <u>Amazon Kinesis</u>
* Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information
* **Real-time streaming data service**
* Examples of streaming dat use cases include:
  * Purchases from online stores
  * Stock prices
  * Game data (statistics and results as the gamer plays)
  * Social network data
  * Geospatial data (think uber.com)
  * IoT sensor data

## <u>AWS Data Pipeline</u>
* AWS Data Pipeline is a web service that helps you reliably process and **move data between different AWS compute and storage services, as well as on-premises data sources, at specified intervals.**
* You can regularly access your data where it's stored, transform and process it at scale, and transfer the results to services such as S3, RDS, DynamoDB, and EMR

## <u>AWS Glue</u>
* **Glue is to fully managed, pay-as-you-go, ETL service that automates the time-consuming steps of data preparation of analytics**
* Glue automatically discovers and profiles data via the Glue Dat Catalog, recommends and generates ETL code to transform your source data into traget schemas
* Simply point AWS Glue to your data stored on AWS, and AWS Glue discovers data and stores the associated metadata (e.g. table definition and schema) in the AWS Glue Data Catalog
* Once catalog, data is immediately searchable, queryable, and available for ETL
* Works with data lakes (e.g. data on S3), data warehouses (including RedShift), and dat stores (including RDS or Amazon EC2 databases)

## <u>Amazon QuickSight</u>
* Quicksight is a fast, cloud-powered BI service that makes it easy to deliver insights to everyone in your organization
* As a fully managed service, QuickSight lets you easily create and publish interactive dashboards that include ML Insights.  Dashboards can then be accessed from any device, and embedded into your applications, portals, and websites

## <u>Amazon Elasticsearch</u>
* Operational analytics such as application monitoring, log analytics and clickstream analytics
* Allows you to search, explore, filter, aggregate, and visualize your data in near real-time

# Architecting for the Cloud

## The difference between traditional and cloud computing environments
* IT Asserts as Provisioned Resources
  * Provision as needed, and scale on-demand
* Global, Available, and Scalable Capacity
  * Deploy globally, easily, cost-effectively, and quickly
* Higher-Level Managed Services
  * Lower operational cost by leveraging managed storage, database, analytics, application and deployment services
    * e.g. S3, RDS, QuickSight, Serverless
* Built-in Security
  * Leverage AWS' significant investment in security, simplify security testing, and use native security and encryption features
* Architecting for Cost
  * Fine grained billing, transparent costs, budgets and alrting tools
* Operating on AWS
  * Tooling, processes and bset practices to support operation transitions

## <u>Scaling vertically and horizontally</u>
* **Scaling Vertically - increasing resources used**
  * EC2 instances
  * RDS Database instances (run on EC2)
  * Limitations
    * Often requires manual intervention (though can be scripted/automated)
    * Typically requires downtime
    * Can reach a limit of scalability
* **Scaling Horizontally (PREFERABLE!)- add more instances**
  * DynamoDB - scaling based on read/write capacity units
  * EC2 Auto Scaling
  * Benefits
    * Seamless scaling, without downtime
    * Can scale almost limitlessly (in some cases)

## <u>Architecting for the Cloud: AWS Best Practices</u> (google it, whitepaper)
* Disposable resources instead of fixed servers
  * With traditional infrastructure you work with fixed resources due to upfront costs and lead time
  * With the cloud you can take advantage of the dynamically provisioned resources
  * When designing for AWS you can take advantage of the dnyamically provisioned nature of cloud computing
  * You think of servers and other components as temporary resource
  * You can launch as many as you need, and use them only for as long as you need them
* Automation
  * Use automation tools on AWS to **improve system stability and organizational efficiency**
  * Examples are:
    * Serverless Management and Deployment
    * Infrastrucure Management and Deployment
    * Alarms and events
* Loose Coupling
  * **Design IT systems to reduce interdependencies**
  * A change or a failure in one component should not cascade to other components
  * Examples are:
    * Service Discovery - components should be able to "find" each other
    * Asynchronous Integration - introduce a layer between application components to store messages
* Services, Not Servers
  * Best practices that advise customers to leverage more than ust Amazon EC2
  * Try to use the breadth of services available on AWS
  * Examples:
    * CloudFront for content delivery
    * DynamoDB for NoSQL databases
    * CloudSearch for search workloads
    * Elastic Transcoder for video encoding
* Databases
  * Use the right database technology for each workload
  * Consider performance requirements, scalability, durability, functionality, etc.
  * Choose bettewen Relational, NoSql, Dat Warehouse, Search, and Graph
* Removing Single Points of Failure
  * **Design for failure! (REMEMBER THIS!)**
  * Intorduce redundancy - ensure if a component/ resource fails, another can take over
  * Detect failure - use health checks and alarms to try to automate detection and reaction
  * Durable Data Storage - ensure your data aligns with your Recovery Point Objective (RPO) and Recovery Time Objective (RTO)
  * Automated Multi-Data Center Resiliencee - be resilient in the face of a major disater
* Optimize for Cost
  * Right Sizing - use the best instance sizes and number of instances for cost efficiency
  * Elasticity - horizontally scale as needed with changing demand
  * Take Advantage of the Variety of Purchaing Options - use Reserved Instances and Spot Instances
* Caching
  * Caching can be used to improve performance and cost efficiency
  * Methods of aching include:
    * Application Data Caching - examples are Amazon ElastiCache (in-memory database) and DynamoDB DAX (accelerator)
    * Edge Caching - key example is CloudFront
* Security
  * Use AWS Features for Defense in Depth
  * Share Security Responsibility with AWS
  * Reduce Privileged Access
  * Security as Code - for instance, use CloudFormation to repeatably build secure environments
* Real-Time Auditing - tools such as Trusted Advisor, AWS Config, and Amazon Inspector

## The Five Pillars of Operational Excellence
* **Operational Excellence** - The operational excellence pillar includes the ability to run and monitor systems to delivery business value and to continually improve supporting processes and procedures
* **Security** - The security pillar includes the ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies
* **Reliability** - The reliability pillar includes the ability of a system to recovery from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such a misconfigurations or transient network issues
* **Performance Efficiency** - The performance efficiency pillar includes the ability to use computing resources efficiently to meet system requirements and to maintain that efficiency as demand changes and technologies evolve
* **Cost Optimization** - The cost optimization pillar includes the ability to avoid or eliminate unneeded cost or suboptimal resources




# Misc services
## <u>Cloud Directory</u>
* Enables you to build flexible cloud-native directories for organizing hierarchies of data along multiple dimensions.

# Misc things
## <u>AWS Quick Start</u>
* Solutions built by AWS Solutions Architects and Partners to help you deploy popular technologies on AWS, based on best practices for security and high availability.
* Each Quick Start includes CloudFormation templates and a guide discussing the architecture.
