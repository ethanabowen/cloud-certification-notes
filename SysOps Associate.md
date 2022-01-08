# EC2
* NAT Gateways - attached to subnets.
* NAT Instance - Along with other steps, you need to disable the source/destination checks that are default for address translation.  Disable the SrcDestCheck attribute for a NAT instance that’s either running or stopped using the console or the command line.
* Internet Gateway - attaches to a VPC.
* EC2Rescue - GUI-based troubleshooting tool that can be run on EC2 Windows Server instances to troubleshoot OS-level issues, collect advanced logs, and configuration files for further analysis.
  *  AWSSupport-TroubleshootSSH installs EC2Rescue then runs SSH-oriented diagnostics.  Can auto repair issues related to subnets, S3 buckets, or an IAM role.
* Burstable instances - credit based instances that can exceed the instance limit for a credit-used amount of time.   An "unlimited mode" of sorts for performance during a burst window.
* Disk(Read/Write)Bytes is the CloudWatch metric associated with EC2
  * If this metric is not available, it is most likely that the instance is using an attached EBS (therefore the instance store volume is not attached).

# ELB
* Access logs - logs with client/target:port and IP address
* ALB can Drop Invalid Header Fields.  Useful as a mitigation technique against malicious attacks.

# Elastic Beanstalk
* Command timeout period can be increased.

# EBS
* Elastic Volumes -
* EC2 Impaired I/O state: Perform a consistency check on the volume attached to the EC2 Instance
  1. Stop any applications from using the volume.
  2. Enable I/O on the volume.
  3. Check the data on the volume.
* gp instances have credit system, the larger the instance type the more credits rewarded
* Volume(Read/Write)Bytes is the CloudWatch metric associated with EBS

# Auto Scaling
* Lifecycle hooks - 1 hour, by default, period between launching/termination of instances where the ASG will Pause the instances until the *complete-lifecyle-action* command/operation is completed...or a time expiry occurs.

# S3
* Glacier Vault Lock Policy - example Write Once Read Many (WORM). Uneditable policy. Useful for compliance.
* Bucket ACL - enable/disable cross-account access, etc.
* S3 Inventory - Parquet formatted file that lists objects and metadata.
* 30 day storage time required before moving to Standard-IA

# Storage Gateway
* Shutdown procedures:
  * File - shutdown your VM.
  * Volume/Tape - stop the gateway, reboot the VM, then start the gateway.

# EFS
* Throughput and scaling
  * Bursting - scales as the size of the storage growth
  * Provisioned - instantly provisions the throughput in MiB/s independent of the amount of data stored.
* Mountable in multiple subnets (AZs)
* Encryption
  * Transit - only enable-able when mounting to EC2.
  * Rest - only enable-able when creating file system.

# Networking
* Security Groups - remember, what goes in CAN come out (stateful)
  * Troubleshooting - Timeout errors are more often Security Group oriented than NACL.
* Network ACL - stateless, need the inbound and outbound rules setup
  * Ports are auto-assigned from 1024-65535.  An outbound rule for allow * to 1024-65535 is usually required for internet access.
* Inter-Region VPC Peering - Share resources between regions or replication data for geographic redundancy. No single points of failure or network bottlenecks.
* Virtual Pirate Gateway - required with Corp-to-AWS VPN type connections.
  * AWS VPN - secure (TLS/SSL) communication
* Layer X investigation
  * layer 3/4 (src/dest ip/port)- VPC Flow Logs
  * layer 7 (Http status codes/path/etc) - CloudFront access logs, ELB logs
* AWS Global Accelerator - optimized endpoint pathing for global services.  Fixed entry points using static IP addresses

# Route 53
* Service Locator Record (SRV) - ?
* CNAME - redirection?
* A vs AAAA vs Alias ?
  * Alias - only for AWS resource redirection/forwarding
  * A - Address record using for IPv4 domain mapping
* String Matching condition - searching health check both (first 5120 bytes)

# AZ/Subnet
* DescribeAvailabilityZones and DescribeSubnets API - returns Ids can be used to identity underlying zones and help match up where resources are deployed across different between accounts.

# CloudFront
* Pre-signed Urls exist just like S3 pre-signed Urls.
* Cache-hit ratio increase
  * Increase Cache-Control max age
  * Use Origin Shield - ?
  * Cache based on query string, cookies, request headers
  * Remove Accept-Encoding header when compression is not needed
  * Serving media content by using HTTP - ?

# Cost and Billing
* CreatedBy Tag - special tag only available in Billing and Cost Management console/reports
  * Applies to each resource created after enabled, doesn't count towards the Account's Tag limits
* Compute Savings Plans - Lambda and EC2 oriented cost-savings plans in Cost Explorer
* Disable Credit Sharing - flag in Consolidated Billing.
* Project-by-Project cost - Cost Allocation Tags, assign Resource Tags
* User cost - createdBy tag
* Cost Allocation Tags manager - manager Organization accounts setup for Cost Allocation Tags

# CloudFormation
* AutoScalingRollingUpdate policy - control how updates work.  Common with wanting to keep the same Auto Scaling group and then replace the old instances based on the parameters set in the policy.
* OUTDATED status can be a permission issue, service quota issue, or any normal multi-account access issues.

# CloudWatch
* Custom Metrics display - create a dashboard, ADD a widget, and choose the metrics from the custom namespace
* Rules - used for state changes not metrics breaches
* Insights - Interactively search and analyze your log data.  Up to 20 log groups.  Results available for up to 7 days.
* Can take action on State changes of underlying hardware.

# AWS Config
* Enforce S3 logging
* restricted-common-ports rule - Checks SGs to ensure they are blocking TCP traffic to specified ports.
* access-keys-rotated rules - enforce access key rotation by looking at the maxAccessKeyAge field.
* encrypted-volumes rule - checks if attached EBS volumes are encrypted (if Key is attached, can check for a specific KMS key as well).
* Can also check AMI ids for compliance
* Config often uses AWS System Manager for automation

# AWS Systems Manager - Session Manager
* Fully manage EC2, edge devices, and on-premise servers/VMs via Console shell or CLI.
* Purpose: Improve security and audit posture, reduce operational overhead by centralizing access control on managed nodes, and reduce inbound node access.
* No open inbound ports, so no need for bastion hosts
* Audit logging available for compliance (who connected to what box and when).

# AWS Systems Manager - Patch Manager
* Automates the process of patching (OS and apps) managed instances with both security related and other types of updates.
* Uses resource tags?

# AWS Service Catalog
* IT service that helps you manage who can use specific products and how they can use them.
* Portfolio - collection of products.
* Granting users access to a portfolio enables that user to browse the portfolio and launch the products in it.  You apple AWS IAM permissions to control who can view and modify your catalog.

# CloudTrail
* Log File Integrity - A file of hashes based on delivered logs.  Using a digest file, you can ensure your logs have not been edited.  The digest files are segregated in a separate folder in the same S3 bucket as the logs.  You can use IAM and S3 MFA on the digest files to ensure they are not accessible.
* Can track the activity of federated users via the STS calls AssumeRoleWithWebIdentity and AssumeRoleWithSAML.
* Data Event logging - data plane operation monitoring, typically high-volume activities
  * S3 object-level API activity
  * Lambda function execution activity (the Invoke API)

# STS
* AssumeRole is often used as the first step of granting access to Federated users.

# AWS Resource Groups
* Tag Editor - can search for tagged and untagged resources across ALL regions. (ex. EC2 Instances)

# AWS Trusted Advisor
* AWS resource best practice monitor - helpful for compliance

# KMS
* Key Material - ?, automatic key rotation disabled

# AWS SSO
* Permission Set - define the level of access that users and group have to an AWS account.  Provisioned by IAM roles.

# AWS Inspector
* There is an EC2 agent for this.

# Redshift
* Enhanced VPC Routing - enable VPC features to manage permissions to access Redshift

# AWS Elastic Search (ES)
* ES can not be moved between public to private data-scopes.  Instead you must create a new domain and either manually reindex or migrate your data.

# DynamoDB
* Global tables can replicate your DynamoDB tables automatically across regions.
  * DynamoDB Streams (ordered flow of information about item changes) must be enabled for data propagation
