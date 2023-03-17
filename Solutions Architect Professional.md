# Networking
* DHCP - Dynamic Host Configuration Protocol (DHCP) provides a standard for passing configuration information to hosts on a TCP/IP network. DHCP options include domain name, name servers, NTP servers, NetBIOS name servers, NetBIOS node type.
* Network ACL attaches to Subnets (typically associated with a load balancer)
* NLBs can be configured with an Elastic IP (cannot be changed)
* Transit Gateway - A transit gateway (TGW) is a network transit hub that interconnects attachments (VPCs and VPNs) within the same AWS account or across AWS accounts.
* PrivateLink - Uses ENI, 3rd party integrations, uses AWS Backbone, private through-and-through, onprem integration available
* AWS Managed VPN - relies on public internet
* VPC Peering - connection VPC with private connection.  Con: Transitive peering is not supported.
* Transit VPC - Hub and Spoke. Common strategy for connecting geographically dispersed VPCs and locations to create a global network transit center
  * Transit VIF is used when DX is being connected to Transit Gateway
* Shared Services VPCs - sharing subnets with other AWS accounts in the SAME Organization!
* A network ACL is an optional layer of security that acts as a firewall for controlling traffic in and out of a subnet.
* A security group acts as a virtual firewall for your instance to control inbound and outbound traffic.    
* Virtual Private Gateway - VPC integration with external (onprem or otherwise) customer gateways.
* Customer Gateway - IPSec tool exposing a CIDR block that associates with a VPG 
* Direct connect
  * Add IPSec VPN as backup (goes over internet)
  * Not recommended for speeds above 1Gbps, due to AWS VPN limitations
  * LAGs - Single logical connection from multiple physical connections
* ALB's port is dynamic, so routing rules towards ALBS need the port to be a range from 1024 - 65535
* Endpoints
  * Interface - uses ENI + security group allowing AWS service connection
  * Gateway - uses ? + route table pointing to specific subnets
## Route 53
* Default limit of 5 requests per second - batch calls to Route 53 if this is occuring

## AWS System Manager
Operational Insight and Take Action on AWS Resources
* Group resources, view insights, take action
* Agent - Remote connection, no need for ports opened, bastion host, or SSH keys
* Run tasks on groups (automation)
* Remote execution at scale
* Parameter store lives here!
* Maintenance Windows
* Run Command - Secure and safe remote management - Note: trace with CloudTrail
* Inventory - collect and query instance metadata for compliance/etc.

# AWS Service Catalog
Take control of your company's cloud resources - Create and goven self-service access to approved IT services.
* Catalog Admins create a Portfolio and THEN a product.
* Product is accessable by specificed users/groups
* Patch Manager - 
* Run Command - 


# AWS Billing

## Billing
* Cost and Usage report
  * Analyze with insights on specific product offerings and usage amounts associated with the costs
  * Ex. "Dynamo::Read" instead of just "Dynamo"
  * Managing account can create a CUR report for each Org and allow each team to visualize in QuickSight 
* Cost categories - managed at the payer account level
* Cost Allocation tags - managed at the payer account level
* Conductor - Customize billing data by account and rate to share with end users -> Cost and Usage Report
  * Organizations use Conductor to help display billing in a meaningful way
* Budget
  * Custom alerts on criteria
    * Cost - How much you want to spend on a service
    * Useage - How much you want to use on one or more services
    * Underutilized RI (EC2) alerts
    * Savings plan - alert when usage Savings plan falls below threshold
* Cost Explorer
  * Messure RI Coverage %
  * Alert on $ thresholds on daily bases

## Cost Management
* Budgets - create, get alerted, response with action
    * Refine with Filtering
    * Cost Anomoly and insights
    * Attach actions to large costs/etc
    * Free
* Compute Optimizer - has a “ExportLambdaFunctionRecommendations” operation for the Lambda functions which exports a csv file as output


# Security
* WAF - monitor web requests that are forward to APIG, Cloudfront, or ALB
  * New WAF - can manage rule groups
  * Logs can be pointed to Kinesis Firehose (which supports http for third party integrations)
* Firewall Manager - manage firewall rules across accounts and applications
* Shield - DDoS protection
  * Advanced - Layer 7, lot more DDoS protection, event and report stuff, 24/7 support
    * WAF and Firewall Manager free with Advanced
* APIG - Usage plans can be used for rate-limited customers.  Requires API keys for each customer.
  * AWS_IAM authentication
    * Uses policies for auth management (group or role preferred).  Actions like 'execute-api:Invoke or execute-api:InvalidateCache' are used in this process.
    * User passes in token to APIG and AWS_IAM auth validates against user's permission boundary.

# CI/CD
* Canary vs Linear - Shifted to new app in customizable increments vs static increments
* Blue/green - Traffic is shifted after NEW ENV is bootstraped and healthy
* OpsWorks - Chef for Cloud, only supports blue/green for fast rollbacks
* Elastic Bean Stalk - supports ENVs switching of CNAMES (URLs) with blue/green
* Lambda - canary deployments work by creating a new alias and shifting weight over time


# Databases
## Aurora
* Reader endpoints with RDS Proxy - A proxy makes applications more scalable, more transparent to database failures, and more secure.
* Best practice to move database connection outside the lambda event handler for subsequent invocations of Lambda can reuse it 
* 15 cross-AZ replica max.  One is automatically promoted to write in event of failure
* Global tables - cross-region
* OLAP vs OLTP - Analytics vs Transactional oriented
* Error logs enabled by default
* Can enable "Slow Query" and "General Logs" with DB parameter groups

## Migrations
### DB
* Most efficient:
  * Oracle on-prem -> Redshift (using SCT - Schema Convertion Tool and DMS)
  * MySQL -> RDS for MySQL (using DMS)
* AWS DMS - offers Ongoing replications or Change Data Capture (CDC)
* Migration Hub - centralized migration dashboard
  * Application Discovery Service - on-prem app discovery and data collection
  * Use DMS for data migration
  * DataSync for Nas/File System migration
  * Server Migration Service for VMs/etc.
* Cross-Account (with RAM) cloning is much faster than creating and restoring a database snapshot
* Dynamo
  * Application Auto Scaling can be scheduled for known burst times

### Data
* DataSync
  * Use for FSx for Windows File Server systems.  
  * Works with DX
  * Supports copying NTSF ACLs and System ACLs for audit.
  * Data is TLS encrypted in transit 

# File Storage
## S3
* Multi-part upload speeds up performance (parallel processing)
* Transfer Acceleration is for upload performance where Cloudfront is for download performance
* IgnorePublicAcls - causes S3 to ignore all Public ACLs on bucket/objects
* Access Logs - details of who is accessing bucket, including IP
* Can decrypt encrypted files with AWS CLI "S3 COPY" command (and with original encrpytion key of course)
* Failed multi-part uploads take up storage and need to be deleted with a lifecycle policy
* Cloudfront - share multiple files with Set-Cookie headers and signed cookies strategy
## EBS
* Throughput Optimized caps at 500 IOPS
* EFS is more expensive than EBS.
## EFS
* Lustre is faster than EFS and fully managed


# Organizations
* Service Control Policy - controls an account's AVAILABLE permissions.
  * Can not be modified for a single OU if attached to multiple OUs
* Resource Sharing
  * Resource Access Manager - cross-Organization sharing
    * A Prefix List is a collection of CIDR blocks that can be used to configure VPC security groups, VPC route tables, and AWS Transit Gateway route tables and can be shared with other AWS accounts using Resource Access Manager
    * Prefix Lists are shared, not subnets specifically

# Virtualization
* AppStream 2.0 - Secure, reliable, and scalable application streaming and low-cost virtual desktop service
* Workspaces - Secure, reliable, and scalable access to persistent desktops from any location
* LightSail - Simple Cloud Server, for users with little to no cloud or technical experience.  Limited Features compared to EC2

# ECS
* Spot Draining will puts instances in DRAINING status as a 2 minute warning.  No new tasks will run on that instance.  **Helps prevent service interruptions.**