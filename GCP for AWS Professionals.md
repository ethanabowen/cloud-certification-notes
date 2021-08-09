# AWS vs GCP Service comparison
| System                         | GCP                                               | AWS                                             |
| ------------------------------ | ------------------------------------------------- | ----------------------------------------------- |
| Virtual Machines               | Compute Engine (GCE)                              | Elastic Cloud Compute                           |
| Block Storage                  | Persistent Disk                                   | Elastic Block Storage (EBS)                     |
| Distribution of Load among VMs | Cloud Load Balancing                              | Elastic Load Balancer                           |
| Simplify Web App setup         | App Engine (GAE)                                  | Elastic Beanstalk                               |
| Container Orchestration        | Google Kubernetes Engine (GKE)                    | EKS, ECS                                        |
| Serverless (via function)      | Cloud Functions                                   | Lambda                                          |
| Private Networks               | Cloud VPC                                         | Virtual Private Cloud                           |
| On Prem to Cloud               | Cloud VPN(shared), Cloud Interconnect (dedicated) | AWS VPN(shared), AWS Direct Connect (dedicated) |

| Storage | AWS | GCP             |
| ------- | --- | --------------- |
| Object  | S3  | Cloud Storage   |
| Block   | EBS | Persistent Disk |
| File    | EFS | Filestore       |

| Database                 | GCP                                 | AWS                 |
| ------------------------ | ----------------------------------- | ------------------- |
| OLTP                     | Cloud SQL, Cloud Spanner            | RDS (Aurora)        |
| Relational Datawarehouse | BigQuery                            | Redshift            |
| NoSQL                    | Datastore/Firestore, Cloud Bigtable | DynamoDB,DocumentDB |
| Cache                    | Memorystore                         | ElastiCache         |

| Other                        | GCP                      | AWS             |
| ---------------------------- | ------------------------ | --------------- |
| Messaging                    | Cloud Pub/Sub            | SNS, SQS        |
| Authentication/Authorization | Cloud IAM                | Amazon IAM      |
| Encrpyting data keys         | Cloud KMS                | KMS             |
| Automate deployment          | Cloud Deployment Manager | CloudFormation  |
| Monitor Metrics              | Cloud Monitoring         | CloudWatch      |
| Logging                      | Cloud Logging            | CloudWatch logs |
| Tracing                      | Cloud Trace              | X-Ray           |

# GCP Services



| Feature                             | GCP   | AWS                        |
| ----------------------------------- | ----- | -------------------------- |
| Create Virtual Machine              | GCE   | EC2                        |
| Choose OS and Software              | Image | AMI (Amazon Machine Image) |
| Choose the right family of hardward |       |                            |

| Edge Locations | GCP                                                | AWS        |
| -------------- | -------------------------------------------------- | ---------- |
| CDN            | Cloud CDN (ex usage: App Engine and Cloud Storage) | CloudFront |

## Billing
* Billing in sub-hour increments (seconds for some services)
* Discounts for sustained user - Usage of 25% or more triggers discounts
* Discounts for committed use - Pay less for steady, long-term workloads
* Discounts for preemptible use - Pay less for interruptible workloads
* Custom VM instance types - Pay only for the resources you need for your application

### Security designs
| Layer                   | Notable security measures (among others)                                                                          |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Operational security    | Intrusion detection systems; techniques to reduce insider risk; employee U2F user; software development practices |
| Internet communication  | Google Front End; designed-in Denial of Service protection                                                        |
| Storage services        | Encryption at rest (on SSD/Hardware                                                                               |
| User Identity           | Central identity service with support of U2F                                                                      |
| Service deployment      | Encryption of inter-service communication                                                                         |
| Hardware infrastructure | Hardware design and provenance; secure boot stack; premises security                                              |

U2F - universal second factor (ex. usb)


## Google Front-end (GFE)
* Checks incoming network contections for correct certificates and best practices
* Additional applies protection against DDoS attacks
* ADDITIONAL layers of DDoS protections BEHIND the GFE as well

## Google Best Practices
* Ensure that you aren't running privileged containers
* Ensure that you are using the native logging mechanisms
  
# Services

## Compute Services
### Compute Engine (GCE)
  * Provision & Manage Virtual Instances
  * Virtual Machines- Virtual Instances in GCP (AWS Eqiv - EC2 Instances)
  * Can create or import images
  * Persistent storage - standard or SSD.  Local SSD content wipes with the VM termination/restart
  * Metadata/Start-up-script processes exist
  * Disk snapshots - useful for transfering vm to new region
  * Preemptible VM
    * Interruptable workloads, for less $ (like AWS Spot Instances)
    * Runs for 24 hours max
    * Will be charged for licenses (if applicable)
  * Scale out before scaling up.
  * 30 second start up vs minutes for AWS
  * Regional persistant disks - HA.  Not available on AWS
  * On-demand charging by the second (minimum of 1 minute)
  * Reserved instances 1-3 years
  * Discounts increase with consistant (duration ) usage

  | Type                | GCP                                      | AWS                                |
  | ------------------- | ---------------------------------------- | ---------------------------------- |
  | Machine RAM and CPU | Machine types                            | Instance types                     |
  | Machine images      | Images                                   | AMI                                |
  | Block storage       | Persistent disks                         | EBS                                |
  | Local attached disk | Local SSD                                | Ephemeral drives                   |
  | Discounts           | Preemptible VMs, Sustained-Use Discounts | Spot Instances, Reserved Instances |


### Kubernetes Engine
### App Engine
### Cloud Functions

## Storage Services
### Cloud Storage
  * Buckets - create, configued, and used
    * Globally unique name
    * Location
    * Storage class
  * Object storage - key value association
    * 5TB object size limit
  * Multi-region buckets do not serve all global areas with similar latency
  * Multiple levels of storage (Coldline, Archive) supports long-term storage with infrequent access
  * Versioning is available objects.
  * Objects are immutable
  * Data encrypted at rest
  * Lifecycle management policies available (ex. delete by duration, date, version #, etc)

  | Bucket attributes                    | Bucket contents             |
  | ------------------------------------ | --------------------------- |
  | Globally unique name                 | Files (in a flat namespace) |
  | Storage class                        |                             |
  | Location (region or multi-region)    |                             |
  | IAM policies or Access Control Lists | Access Control Lists        |
  | Object versioning setting            |                             |
  | Object lifecycle management rules    |                             |

  #### Storage Classes
  |                              | Multi-region                 | region                              | Nearline                        | Coldline                       |
  | ---------------------------- | ---------------------------- | ----------------------------------- | ------------------------------- | ------------------------------ |
  | Intended for data that is... | Most frequently accessed     | Accessed frequently within a region | Accessed less than once a month | Accessed less than once a year |
  | Availability SLA             | 99.95%                       | 99.90%                              | 99.00%                          | 99.00%                         |
  | Access APIS                  | Consistent APIs              | Consistent APIs                     | Consistent APIs                 | Consistent APIs                |
  | Access time                  | Millisecond access           | Millisecond access                  | Millisecond access              | Millisecond access             |
  | Storage price                | Most                         | Less                                | Less                            | Least                          |
  | Retrieval price              | Least                        | Least                               | More                            | Most                           |
  | Use cases                    | Content storage and delivery | In-region analytics, transcoding    | Long-tail content, bacups       | Archiving, disaster recovery   |

  When is comes to GCP vs AWS and storage, the largest different is locality.  GCP is broader with multi-region solutions.  Also faster with Coldline retrieval.

  #### Tranfer
  * Online transfer - self-managed copies using cli (gsutil) or drag-and-drop
  * Storage Transfer Service
    * Scheduled, managed batch transfer from another cloud provider, other region, or http endpoint
  * Transer Applicance (Beta) - Rackable appliances to securely ship your data (physical transfer, like AWS Snowball)
  * Examples of storage with services
    * Import and export tables with BigQuery
    * Object sotrage, logs, and Datstore backups with App Engine
    * Startup scripts, images, and general object storage with Compute Engine
    * Import and export tables with Cloud SQL
### Cloud SQL (managed RDBMS)
  * Offers MySQL, PostgreSQL and SQL Server (beta) databases as a service (terabyte storage)
  * Offers transactions
  * Automatic replication
  * Managed backups
  * Vertical scales (read and write)
  * Horizontal scales (read replicas)
  * Google security - network firewalls and all other data rest/motion
  * Ways to use:
    * App Engine using standard language drives
    * Compute Engine using authorized access via an external (exposed) ip (preferred zone configurable)
    * External service/application - Standard database admin tools (ex. Toad)
### Cloud Spanner
### Cloud CDN
  * Used to cache data hosted on other cloud providers and supports large objects such as video
  * Apigee Edge is **not** designed to cache data larger thatn 512KB
### Firestore
### Cloud Datastore

## Big Data Services
### BigQuery
  * Serverless cloud data warehouse for analytics
  * Easy querying for report generation
  * Low storage costs
### Bigtable
  * Fully managed NoSQL, wide-column database service for terabyte applications
  * Optimized for read-write latency and analytics throughputs, not analytics querying and reporting
  * Ideal for single look-up key.
  * Think of it as a persistent hash table
  * Operational and Analytics ready
  * Accessed using HBase API (open source)
    * Native database for Apache Hadoop
  * Scalable storage and computation capabilities with machine count changes
  * Handles admin tasks like upgrades and restarts transparently
  * All data is encrpytred in-flight ad at rest
  * Can be IAM restricted
  * Many of googles core services use Bigtable
  * Data can be read/write via
    * Application API (examples)
      * HBase REST Server
      * Java Server using the HBase client
    * Streaming (examples)
      * Cloud Dataflow Streaming
      * Spark Streaming
      * Storm
    * Batch Processing (examples)
      * Hadoop MapReduce
      * Dataflow
      * Spark
* 
### Pub/Sub
### Dataflow
### Dataproc
### Datalab

## Machine Learning Services
### Natural Language API
### Vision API
### Machine Learning
### Speech API
### Translate API
### AutoML

## Network/Security Services/Tools
### Cloud Armor
  * Built-in DDoS protection
### VPC Service Controls
  * Improves your ability to mitigate the risk od data exfilration from Google Cloud services
  * Protects data
### Identity-Aware Proxy (IAP)
  * Allows you to manage access to HTTP-based app outside of Google Cloud
### Cloud Data Los Prevention (DLP)
  * Designed to de-identify PII information
### Secrets Manager
  * Secure and convenient storage system for API keys, passwords, certificates, and other sensitive data
  * Provides a central place and single source of truth to manage, access, and audit secrets across Google Cloud
### Virtual Private Cloud (VPC)
  * Routing table forward traffic from one instance to another instance within the same network even across subnetworks and within GCP zones, without requiring an external IP address
  * Firewalls are globally distributed for you.
    * Metadata **tags** can be used to define firewall rules (tag ex. Web)
  * Shared VPC - Share a network, or individual subnet, with other GCP projects (via IAM)
  * VPN Peering - interconnect metworks in GCP projects
  * Cloud Load Balancing (whoa)
    * Single, Global anycast IP address
    * Traffic goes over the Google backbone from the closest PoP to the user
    * Backends are selected based on load
    * Only healthy backends receive traffic
    * No pre-warming is required
    * Multi-region failover baked in
    * Vpc offers a suite of load-balancing options
      * Global Http(s) - Layer 7.  Can route different URLs to different backends.
      * Global SSL Proxy - Layer 4 non-HTTPS SSL. Supposed on specific port numbers.
      * Global TCP Proxy - Layer 4 non-SSL TCP traffic. Supposed on specific port numbers.
      * Regional - Any traffic (TCP, UDP). Supposed on any port number.
      * Regional internal - Traffic inside a VPC.  Use for the internal tiers of multi-tier applications.
  * Create managed zones, then add, edit, delet DNS records.
  * Modifications to subnet IP range does not affect already configured VMs
  
  | VPC                 | GCP                       | AWS                             |
  | ------------------- | ------------------------- | ------------------------------- |
  | Virtual networks    | VPC networks (**GLOBAL**) | VPCs (regional)                 |
  | IP address ranges   | Subnets (**REGIONAL**)    | Subnets (AZs)                   |
  | Routing entries     | Routes(**GLOBAL**)        | Routes (regional)               |
  | Security boundaries | Firewall rules (global)   | NACLs, Security Groups (global) |

Clearly, Google has 1-uped Amazon's Global network strategy.  Simlifying architecture across the board.
### Cloud DNS
  * Domain Name System - converst internet host names to addresses
  * Public Domain Name System is reachable at 8.8.8.8
  * Highly available and scalable.  Free and Managed.  Low-latency and HA.
  * Content Delivery Network (CDN) - globally distributed edge caches cache content close to the user
    * CDN Interconect Partner program
### Interconnection
  * VPN
    * Secure multi-Gbps connectino over VPN tunnels
    * IPSec protocol
  * Direct Peering
    * Private connection between you and Google for your hybrid cloud workloads
    * Put a router in the same public data center as a google point of presence (over 100 of these around the world)
    * Not covered by Google Service Level Agreement
  * Carrier Peering
    * Connection through the largest parnet network of service providers 
    * Not covered by Google Service Level Agreement
  * Partner Interconnection
    * Allows for customer to use existing lower bandwidth interfaces that they have on their current firewall
  * Direct Interconnection
    * Connect N X10 gig transport circuits for private cloud traffic to Google Cloud at Google POPs
    * Requires the customer to buy new hardware for their firewall
### Cloud Router
  * Dynamic VPN connections
  * Allows other networks and Google GCP networks excahnge route information over the VPN
  * Uses border gateway protocol
  * Ex.  Add a new subnet to your Google GCP, your on-prem network will automatically get the routes to your new subnet.
### Load Balancing
  * HTTP, TCP, UDP requests
  * Internal and external access
  * Firewall protection
  * Health checks and sesion affinity
  * Path-based routing

|                      | GCP                                        | AWS                                             |
| -------------------- | ------------------------------------------ | ----------------------------------------------- |
| Service type         | Software-based                             | Instance-based                                  |
| Managed service      | Global                                     | Regional                                        |
| Request routing      | URL map (HTTP only)                        | Listener, listener rule                         |
| Service health check | Instance group, Backend service (capacity) | Target group                                    |
| Load balanced scope  | Global                                     | Region (requires Global Accelerator for Global) |


## Misc Services/Tools
### Cloud Shell
  * Facilitates rapid iteration of Google Cloud architecture changes
### Cloug Logging
  * Optimized for monitoring, error reporting, and debugged
  * Not for analytics purposes

## *-as-a-Service Responsibilities
<u>Legend:</u>

O - Customer-managed

X - Google-managed

| Responsibility            | On-prem | IaaS | PaaS | Managed Services |
| ------------------------- | ------- | ---- | ---- | ---------------- |
| Content                   | O       | O    | O    | O                |
| Access policies           | O       | O    | O    | O                |
| Usage                     | O       | O    | O    | O                |
| Deployment                | O       | O    | O    | X                |
| Web application security  | O       | O    | O    | X                |
| Identity                  | O       | O    | X    | X                |
| Operations                | O       | X    | X    | X                |
| Access and authentication | O       | X    | X    | X                |
| Network security          | O       | X    | X    | X                |
| OS, data, and content     | O       | X    | X    | X                |
| Audit logging             | O       | X    | X    | X                |
| Network                   | O       | X    | X    | X                |
| Storage and encryption    | O       | X    | X    | X                |
| Hardware                  | O       | X    | X    | X                |

# Billing
## Billing Tools
* Budgets & Alerts - Alerts typically sent at 50/90/100% of usage (% and $ based)
* Reports
* Quotas - Rate/Allocation (requests/resources)

| Billing Type     | GCP                              | AWS                     |
| ---------------- | -------------------------------- | ----------------------- |
| Billing accounts | Many per account                 | One per account         |
| Billing roll-ups | Projects                         | Sub-accounts            |
| Policy levels    | Account, org, folder, project    | Account, org            |
| Admins           | Google users or Groups           | IAM user, Groups, Roles |
| Account Admin    | Gmail user or G Suite super user | Root user (per account) |

# Policy Management

### Resource Hierarchy structure:

Org Node --1:M--> Folders --1:M--> Projects --1:M--> Resources

* Policies are inherited downward.
* A resource belongs to 1 and only 1 project.
* Project ID is globally unique, chosen by you, and immutable
* Project name not required to be unique, chosen by you, and mutable
* Project number, globally unique, assigned by GCP, and immutable
* Resources in a folder inherit IAM permissions FROM their folder
* To use folders you HAVE to have an Org Node at the top of the hierarchy
  * It's the 'root' node of the policy system
  * If you are a GSuite customer, you already have an Org Node
  * Otherwise you can create one in the Identity service
* Policies are inherited TOP DOWN. Parent policies can not take away access granted at a lower levels

#### IAM breakdown
* Who - Google account, Cloud Identity user, Service account, Google group, Cloud Identity or G Suite domain
* can do what - IAM Role
* on which resource - IAM Policy

| IAM concept                   | GCP                                                                                   | AWS                                                                        |
| ----------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Programmatic identity         | Cloud IAM service account                                                             | IAM role and instance profile                                              |
| User identity                 | Managed outside Cloud IAM.  Identity federated to external identity management system | Managed in IAM.  Identity federated to external identity management system |
| Policy                        | A list of bindings.  A binding binds a list of members to a role.                     | A document that explicitly lists permissions                               |
| Permission collection         | Role                                                                                  | Policy                                                                     |
| Perdefined set of permissions | Predefined roles                                                                      | Managed policies                                                           |

#### Types of role
* Primative - broad, 'can do what, on all resources'
  * Owner, Editor, Viewer, Billing administrator
* Predefined - 'can do what, on specific resources on specific projects' 
* Custom - best for Least Privileged approach
  * Can not be used at the folder level.  Only Project or Organization levels.

#### Service Accounts
* Provide an identity for carrying out **server-to-server** interactions in a project
* Used to **authenticate from** one service to another
* Used to **control privileges** used by resources
  * So that applications can perform actions on behalf of authenticated end users
* Identified with an **email** addresss:
  * PROJECT_NUMBER-computer@developer.gserviceaccount.com
  * PROJECT_ID@appspot.gserviceaccount.com
* Is a resource, can have IAM policies attached to it.

**User-Role pairings defines a users permissions in GCP.**


# GCP interactions
* Cloud shell - in browser CLI
* SDK - CLI tools gcloud, gsutil (Cloud Storage), and bq (BigQuery)
* API Explorer - interactive tool that lets you easily try APIs in using a browser
* Libraries
  * Cloud Client Libraries - Communit-owned, hand-crafted client libraries
  * Google API Client Libraries
    * Open source, generated
    * Support various languages
* Mobile app - management of VMs, DBs, insight on usage, etc.