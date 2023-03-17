# EC2
* Security group controls inbound and outbound traffic, through ports and acts as a firewall
* EBS volume is a virtual hard  drive associated with the ec2 instance
* EC2 Instances are not IN a subnet, they are access by interfaces that are in public/private subnets
* Internet Gateway enables access to/from the internet FOR A VPC
* User data
  * 16kb text or file of commands run as/before the instance starts up
* Meta-data
  * information about the instance http//169.254.169.254/latest/meta-data
* Monitoring
  * CloudWatch metrics can show Storage, Networking, Resource usage, and more.
  * Detailed Monitoring, which has a cost to it, will send CloudWatch metrics event 1 minute.  Up from the default of every 5 minutes
*  Placement groups
  * Cluster - packs instances close together INSIDE an AZ.  This strategy enables workloads to achieve the **low-latency** network performance necessary for **tightly-coupled** node-to-node communication that is typical of **HPC (High Performance Computing) applications**.
    * Inter-Instance traffic
    * Considered advanced networking
  * Partition - spreads your instances across logical partitions such that groups of instances in one partition **do not share the underlying hardware** with groups of instances in different partitions.  This strategy is typically used to large **distributed and replicated workloads**, such as Hadoop, Cassandra, and Kafka.
    * Separate AWS racks
 * Spread - strictly places a small group of instances across **distinct underlying hardware** to reduce correlated failures.
* Instance Types
  ![EC2 Instance Types](/diagrams/ec2_instance_types.png)
* Network Interfaces
  * Elastic Network Interface - public/private IP
    * Basic adapter type for when you don't have high-performance requirements
    * Can be used with all instance types
    * Multiple ENI must be in the same AZ
  * Elastic Network Adapter - advanced and specific instances
    * Enhanced Networking performance
    * Higher bandwidth and lower inter-latency
    * Must choose supported instance type
  * Elastic Fabric Adapter
    * Use with HPC and MLI/ML use cases
    * Tightly coupled applications
    * Can use with all instance types
* Elastic IP - attaches to ENI
  * When attached to an ENI, they persist when an EC2 instance is shutdown
  * Cost money if not used
  * Can be mapped ACROSS AZs
  * Can be mapped to new instance and Elastic Network Adapters
  ![ENI Multi AZ](/diagrams/eni_multi_az.png)
* NAT for Public addresses
  * Network Address Translation
  * Internet Gateway Nat
  ![Internet Gateway Nat](/diagrams/internet_gateway_nat.png)
  * Translation of private ip in ec2 instance to public ip that the world knows of, and vice versa.
* Internet Gateway
  * **You cannot attach an Internet Gateway to a Private Subnet!**
  * Private subnets do not have a CIDR block for public IPs like Public Subnets do!
  * Attach to VPC, along SIDE of Route tables
* Public or Private, subnets are auto assigned to default Router Table unless assigned otherwise.
* Bastion Hosts
  * Use SSH Agent forwarding to allow a local SSH agent to reach through an existing SSH connection and authenticate on a remove server.
  * ssh-add -K <key>
    * Adds key into memory and can be used again in the SSH shell
* NAT Gateways
  * Managed by AWS, auto HA across AZs
  * Allows for Private Subnet instances to access the outside world via a Public Subnet (and often Internet Gateway)
  * You always deploy your NAT Gateways in Public subnets!
  * Elastic IP assignment only.  Can't do public IP?
  ![NAT Gateway](/diagrams/nat_gateway.png)
* NAT Instance, no HA out of the box
  * Managed by you, EC2 Instance
  * The OLD way of doing NAT Gateway
  * Disable Source/Desctination Checks - enables NAT I guess
  * Uses AMI "amzn-ami-vpc-nat"
* EC2 Lifecycles
 * Hibernating EC2 Instances
   * Applies to on-demand or reserved Linux Instances
   * Contents of RAM saved to EBS volume
   * Must be enabled for hibernation when launched
   * When started (after hibernation):
     * The EBS root volume is restored to its previous state
     * The RAM contents are reloaded
     * The processes that were previously running on the instance are resumed
     * Previously attached data volumes are reattached and the instance retains its instance ID
  * Recovering EC2 instances
    * CloudWatch can be used to monitor system status checks and recover instance if unneeded
    * Applies if the instance becomes impaired due to underlying hardware / platform issues
    * Recovered instances is identical to original instance
* AWS Nitro System
 * Nitro is the underlying platform for the next generation of EC2 instances
 * Support for many virtualized and bare metal instance types
   * No hypervisor, direct connect to physical infrastructure
 * Breaks functions into specialized hardware with a Nitro Hypervisor.
 * Specialized hardware includes:
  * Nitro cards for VPC
  * Nitro cards for EBS
  * Nitro for Instance Sotrage
  * Nitro card controller
  * Nitro security chip
  * Nitro hypervisor
  * Nitro Enclaves
* Improves performances, security and innovation:
  * Performance close to bare metal for virtualized instances
  * Elastic Network Adapter and Elastic Fabric Adapter use this system
  * More metal instance types
  * Higher network performance (100 Gbps)
  * HPC
  * Dense storage instances (60TB)
* Pricing
  * On Demand - No discount
  * Reserved - 1-3 year commitment, up to 75% discount
  * Spot - up to 90% discount, workloads can terminate at any time
  * Dedicated Instances - Physical Isolation, account isolation; pay per instance
    * Other non-dedicated instances in your account can share the same hardware, but no other customer can.
  * Dedicated Hosts - Physical server dedicated for your use; Socket/core visibility, host affinity; pay per host; workloads with server-bound software licenses.  Most expensive
  * Savings Plans - Commitment to a consistent amount of usage (EC2 + Fargate + Lambda); Pay by $/hour; 1 or 3-year commitment
  * Red Hat EL, SUSE, ES, Windows - billed per hour
  * Linux/Ubuntu - Billed per second; minimum of 1 minute
  * EBS - billed per second, minimum of 1 minute
  * Reserved Instances
    ![Reserved Instances](/diagrams/ec2_reserved_instances.png)
    * Attributes that must match for the RI to be applied:
      * Instance Type
      * OS
      * AZ + Region
      * Tenancy: Default or Dedicated
  * Savings Plans (1 or 3 year, hourly usage amount)
    * Compute - Fargate, Lambda, and EC2: Any Region, family, size, tenancy, and OS
    * EC2 - EC2 only: Selected Region and Instance Family; Any size, tenancy, and OS
  * Spot Instances - Big for unused capacity at up to 90% discount
    * Spot Instance: One or more EC2 instances
    * Spot Fleet - Launches and maintains the number of Spot/On-Demand instances to meet specified target capacity
    * EC2 Fleet - Launches and maintains specified number of Spot/ On-Demand / Reserved instances in a **single API Call**
    * Spot Block - 1-6 hour of dedicated time with a Spot instance at 30-45% of the price of On-Demand
      * aws ec2 request-spot-instances --block-duration-minutes 360 --instance-count 5 --spot-price "0.25"
  * EC2 Architectures
    * A tightly coupled HPC workload requires a low-latency between nodes and optimum network performance.  Solution: Launch EC2 instances in a SINGLE AZ in a Cluster Placement Group and use an Elastic Fabric Adapter (EFA)
    * A line of business application receives weekly bursts of traffic and must scale for short periods - need the most cost-effective solution.  Solution?  Reserve instances for the minimum required workload and then use Spot instances for the bursts in traffic!
    * A fleet of Amazon EC2 instances run in private submarkets across multiple aces.  Company needs a redundant path to the internet.  Solution?  Deploy NAT Gateways into multiple AZs (into their respective public subnets) and update route tables.
    * An application requires enhanced networking capabilities. Solution? Choose an instance type that supports enhanced networking, and you need to ensure that the ENA module is installed and ENA support is enabled
    * An instance needs close to bare metal performance EFI, the elastic fabric adapter and high performance networking. Solution? Use an IWC Nitro instance type
* Load Balancers
  * Load Balancers + Auto Scaling are often used together for failure recovery
  ![Load Balancer + Auto Scaling](/diagrams/load_balancer_auto_scale.png)
  * Auto Scaling
    * Define collections of EC2 instances that are scaled and manged together
    * Scaling policies define how to respond to changes in demand
    * Can response to CloudWatch notifications and scale up and down accordingly
    * Launch Template - attributes of EC2 instanes launched in the ASG
    * Launch Config - Launch Template with less options
    * Can configure EC2 Demand type and VPC/Subnet(s) deployment values
    * Optional load Balancer can be attached
    * Configure health checks EC2 (EC2 status checks)& Elastic Load Balancer (additional ELB health checks)
      * Health check grace period - period of time to wait before checking or acting on health status
    * Group size and scaling policies
    * Group Metrics (ASG)
      * Data points about the Auto Scaling group
      * 1-minute granulariy
      * No charge
      * Must be enabled
    * Basic monitoring (Instances) - No charge, 5 minute interval
    * Detailed monitoring (Instances) - Charges apply, 1 minute interval
    * Cooldowns - simple scaling policy approach that prevents launching or terminating before the effects of previous activities are visible.  Default value is 300 seconds (5 minutes)
    * Termination Policy - order of temination
    * Termination Protection
    * Stanby State - Used to put an insatnce in the InService state into the Standby state, update or troubleshoot the instance
    * Lifecycle Hooks - Used to perform custom actions by pauses instances as the ASG launches or terminates them
      * Use Cases:
        * Run a script to download and install software after launching
        * Pause an instance to process data before a scale-in (termination).
    * Stickness Sessions can be enabled (think Session Store, etc)
  * Load Balancers route to Target Groups.  Groupings of instances in subnets to route to.
    * Can balance between Target Groups with the LB Listener Rules
  * ELB
    * Access logging is option and disabled by default
    ![load_balancers_1](/diagrams/load_balancers_1.png)
    ![load_balancers_2](/diagrams/load_balancers_2.png)
  * Application Load Balancer - HTTP/HTTPS protocols. Layer 7 routing.  Host-based, Path-based, HTTP header-based, HTTP method-based, Query string parameter-based, Source IP address CIDR-based routing.
    * Supports **Weighted Target Groups**, think Blue-Green deployments
  ![ALB Routing](/diagrams/alb_routing.png)
  * Network Load Balancer - TCP,TLS,UDP,TCP_UDP protocols. Doesn't support path-based routing or host-based routing
    * Used with VPC Endpoint services.
    * Gives ultra-low latency
    * **Static IP addresses**
    ![NLB Routing](/diagrams/nlb_routing.png)
    * A Network Load Balancer deploys virtual nodes in each subnet it's associated with, associates those nodes to an Elastic IP address, and points to a Target Group of EC2 instances that can be managed by an Auto-Scaling Group.  When one of those instances goes down, the Auto-Scaling Group will deploy a new instance based on the Launch Template associated with the ASG.
    * BYOIP - Bring your own IP.  Can assign your own IP to NLP, doing so removes the need to update and existing whitelists
  * Classic Load Balancer - EC2-Classic network only.  Doesn't support path-based routing or host-based routing.
  * Gateway Load Balancer
    * Layer 3 - listens for all packets on all ports.
    * Doesn't support path-based routing or host-based routing.
    * Exchanges traffic with applications using the GENEVE protocol on port 6081.
    * Used in frotn of virtual applinces such as firewalls, IDS/IPS, and deep packet inspection systems.
 * Scaling
   * Dynamic - elastic scaling by configuration
   * Step - scaling rate at intervals.  Example 2/4/6 instances based on 50,75,90 % CPU utilization.  **Can be based on multiple metrics**.  Can bypass grace period for scaling
   * Scheduled - interval scaling
   * Predictive - intelligent scaling, managed by AWS?
   * Target Tracking - Multi value, can bypass grace period for scaling
 * SSl/TLS Termination
   * TLS Termination means that the Load Balancer will be the final place in a request that the network communication is encrpyted
   ![ALB SSL/TLS Termination](/diagrams/ssl_tls_termination_1.png)
   ![NLB SSL/TLS Termination](/diagrams/ssl_tls_termination_2.png)
* Organizations
  * Service Control Policies - Do not grant **ANY permissions**, they control the **AVAILABLE** permissions
* VPC
  * IPv4 Addressing Primer
    * DNS - Domain Name System
      * DNS Resolution - change www to ip
    * Each part of the address is a binary octet
      * 192.168.0.1 - dotted decimal notation
      * 192.168.0.1 = 11000000 10101000 00000000 00000001      
    * Network Id = 192.168.0    Host Id = 1
    * Subnet Mask = 255.255.255 Host Id = 0
      * A subnet mask is used to define the network and host id
      * Where 255 is present the binary is 11111111, which in networking represents a static portion of the network id in the IP address.
      * Where 0 is present, that means the octet can be assigned different values, ie the host id
      * In this example, there are 3 255 octets (24 one digits, or 3 sets of 8 ones), which makes the ip notation 192.168.0.0/24
    * The last digit in an IP can not be 255, it is reserved for the broadcast address
    * The last digit in an IP can not be 0
    * Private IP Adress Ranges (RFC-1918)
      * Class A 10.0.0.0 -> 10.255.255.255
      * Class B 172.16.0.0 -> 172.32.255.255
      * Class C 192.168.0.0 -> 192.168.0.255
  * Classless Interdomain Routing (CIDR) uses Variable length subnet masks (VLSM) where you can use more than /8 /16 /24 /32, if you want to use a range inbetween you can without problem!
  ![CIDR](/diagrams/cidr.png)
    * Split your HA resources across subnets in different AZs
    * VPC Peering requires non-overlapping CIDR blocks - this is across ALL VPCs in ALL Regions/Accounts you want to connect
    * **Avoid overlapping CIDR** blocks as much as possible!
    * Tool: https://network00.com/NetworkTools/IPv4SubnetCreator/
  ![VPC Subnets](/diagrams/vpc_subnets.png)
  * VPC Router takes care of routing within the VPC and outside of the VPC via Route tables
    * To access Internet, add IG as target in Route Table
  * Internet  Gateway is attached to a VPC and used to connect to the Internet (ingress and egress)
* Stateful vs Stateless Firewalls
  * Network ACL - Stateless.  Must allow rule for inbound and outbund communications
  * Security Groups - Stateful.  Allows the RETURN (outbound) TRAFFIC automatically.  Only need to apply single rule.
* Custom Metrics
  * Memory utilization
  * Disk swap utilization
  * Disk space utilization
  * Page file utilization
  * Log collection

# VPC Connection Types
* VPC Peering
  * Transitive networking doesn't exist, so **FULL MESH TOPOLOGIES** are required a fully linked VPC Peered Network
  * Can communicate Private Subnet <-> Private Subnet
  * Communicates on AWS's private network
* VPC Endpoints
  * Private communication to AWS Services.
  * VPC Interface - Attaches an ENI to EC2 instance for network flow to services privately
    * Lambda,
    * Securitiy Groups
  * VPC Gateway - Private communication to S3/DynamoDB.  Uses S3 or DynamoDB Endpoint and requires **Route Table entry**
    * Policies- can be applied to the endpoint and bucket.
      * VPC Endpoint policy
      * S3 Bucket policy - Ex. Only traffic from S3 endpoint gateway can access this bucket
  * Interface and Gateway Load Balancer endpoints are powered by AWS PrivateLink
  * Interface endpoints and Gateway Load Balancer endpoints are powered by AWS PrivateLink, and use an elastic network interface (ENI) as an entry point for traffic destined to the service.
* AWS Client VPN - VPN Endpoint uses ENI EIP to SNAT from VPN (SSL/TLS) to CIDR
  ![AWS Client VPN](/diagrams/aws_client_vpn.png)
* AWS Site-To-Site VPN
  * Most common 'connect to data center' architecture
  * Uses Virtual Private Gateway (VPG) and Customer Gateway which is deployed on their site
  ![AWS Site-To-Site VPN](/diagrams/aws_sitetosite_vpn.png)
* AWS VPN CloudHub
  * Architectural pattern, not a service
  * Hub-and-Spoke model
  * 1 VGW to N Customer Gateways
  * Each Spoke must have a unique BGP ASN, Border Gateway Protocol Autonomous System Number
    * BGP - use for advertizing routes to different parts of your network
  * Connections are VPN (IPSec VPN)
  * **Does support transitive communication between Spokes**!
  ![AWS VPN CloudHub](/diagrams/aws_vpn_cloudhub.png)
* Direct Connect
  * Private connect into AWS (VPC)
  * Physical Fibre connection running 1-100Gbps
  * Consistent speed
  * Lower cost for LARGE data transfer
  * Private VIF - A virtual interface (802.1Q VLAN) and a BGP session.  Connection to a single VPC in the same AWS Region used a Virtual Private Gateway (VGW)
  * Public VIF - Can be used to connect to AWS Public services in any Region (but NOT the Internet)
  * **DX Connections are NOT encrypted!**
    * Use an IPSec S2S VPN conneciton over a VIG to add encryption in transit
* Direct Connect - 1 Private VIF can be mapped to multiple regions using a DX Gateway.  Non-transitive communcation between connected regions.
![Direct Connect](/diagrams/direct_connect.png)
![Direct Connect Multi-Region](/diagrams/dx_multi_region.png)
* AWS Transit Gateway - Cloud Router. Spoke and Hub for VPC(s) to 'out of AWS building' networking.
 * Corporate office connects via a Customer Gateway (if not using DX)
 * TGWs can be attached to VPNs, DX Gateways, 3rd party applicances, and TWGs in other Regions/Accounts
 * TGW + DX architecture
   * Uses a Transit VIF and supports full transitive routing between on-premises, TGW and VPCs!
   ![Direct Connect Multi-Region](/diagrams/transit_gateway_dx.png)
* IPv6
  * Hexidemical notation
  * **Egress-only Internet Gateway - outbound for IPv6**, but not inbound
  * Your subnet has an IPv4 address AND IPv6 address.
  * IPv6 Address example:   2020 : 0001 : 9d32 : 5bc2 : 1c48 : 32c1 : a93b : b12c
    * Network Part is 2020 : 0001 : 9d32 : 5bc2
    * Node Part is in 1c48 : 32c1 : a93b : b12c
    * Last 2 digit must be unique (ex. 2406:da1c:f7b:ae**11**::/64)
* VPC Flow Lows
  * IP Traffic in and out of a VPC
  * Integrates with CloudWatch and S3
  * Can be connected at the VPC, the Subnet, and/or a Network Interface
* Equal-cost multi-path (ECMP) routing supports traffic over multiple VPN tunnels.
  * A single VPN tunnel still has a maximum throughput of 1.25 Gbps. If you establish multiple VPN tunnels to an ECMP-enabled transit gateway, it can scale beyond the default limit of 1.25 Gbps.

# S3
* Key-Value store. 5tb file max.
* **read-after-write consistency**, no longer eventual consistency!
* S3 Gateway Endpoint - EC2 instance connect using privcate addresses (type of VPC Endpoint)
* S3 Storage Classes
     ![S3 Storage Classes](/diagrams/s3_storage_classes.png)
* S3 Object Permissions can be Public while the Bucket is Private
* ACL - Can grant account access via canonical value
* Replication rules apply for NEW changes to a bucket, doesn't move exist files over.
* _bucket-owner-full-control_ can be forced for all files via a bucket policy
* **Lifecycle Rules** -  All or filtered objects.
  * Transition current or previosu versions between storage classes
  * Expire current versions of objects
  * Permanently delete previous versions of objects
  * Delete expired delete markers or incomplete multipart uploads
  * Charged for Glacier Ingress
* Supports MFA
  * For versioning state of a bucket or permanently detleting an object version
  * x-amz-mfa
  * Versioning required
  * MFA can only be enabled by bucket owner
  * MFA-Protected API Access
    * MFA protection for AWS resources (not just S3)
    * Enforced using the **aws:MultiFactorAuthAge** key in a bucket policy
* Encryption-At-Rest
  * Default Encryption for new objects only (S E)
  * Encrpytion/Decryption Saved and Loaded from bucket
  * SSE-S3 - Server-side encryption with S3 managed keys
    * S3 managed keys
    * Unique object keys
    * Master key
    * AES 256 - Advanced Encrpytion Standard
  * SSE-KMS - Server-side encryption with KMS managed keys
    * KMS managed keys
    * Customer master keys
    * CMK can be customer generated
  * SSE-C - Server-side encryption with client provided keys
    * Client managed keys
    * Not stored on AWS
  * Client-side encryption
    * Client managed keys
    * Not stored on AWS
    * OR you can use a KMS CMK
* Event Notificatons - SNS topics, SQS queues, Lambda
* Presign URL - valid url for private S3 Object for certain amount of time.  Grants public access for duration of link.
* Multipart upload - Recommended for 100MB+ files. Required for 5GB+ files. Can be used for 5MB-5TB files.
* S3 Transfer Acceleration
  * Uses CloudFront edge locations to improve performance to transfer from client to S3 bucket.
  * Bucket level setting
  * Only charged if more performant
* S3 Select and Glacier Select
  * SQL expression to retrieve individual files in a object that's zipped in the bucket.   Ex: "SELECT * FROM s3object s WHERE..."
* Server Access Logging
  * Access Logs and requests for an S3 bucket.
  * Must be enabled and Logs should go to a seperate bucket.
  * Amaazon S3 Log Delivery group on destinatoin bucket must be enabled.
* S3 Static Hosting
  * **Bucket name MUST match domain name**
  * index.html requires
* CORS - can be JSON configured with AllowedHeaders, AllowedMethods, and AllowedOrigins
* Cross-Account Access - "Handshake" between principal and role to assume.  Principal has to have access to assume the role, the role has to allow the Principal to assume it.  The role then needs to have access to the S3 Bucket (or AWS resource) being accessed.

# Route 53
* Hosted Zone represents a set of records belonging to a domain
  * **A record points to IP**
  * **Alias record point to domain, can point to Load Balancer**
  * You can migrate hosted zones in and out of Route 53
* Domain Registration
* Health checks of ec2 instances
* Traffic Flow
* **You can also associate a Route 53 hosted zone with a VPC in another account**
  * **Authorize associatation with VPC in a second account**
  * **Create an association in the second account**
* A record, AAAA record? DNS Alias record
* CNAME - ex.   **console**.aws.com vs aws.com
* Routing Policies
  * Failover Routing will direct to secondary if primary is down
  * Geoproximity is GeoLocation (you set location:region mapping) routing with preset location bias.
  * Bias can expand or shrink the size of the geographic region from which traffic is routed to a resource
  * Weighted routing is useful for version testing new software (a/b).
  * Latency routing is when AWS selects the best region based on location
  * Multivalue is for load balancing
* Resolver - Inbound and Outbound Endpoints (names of network interfaces) resolve and return DNS lookups between EC2 instances and External clients over VPN/Private connections
* With an A record aliased, Route 53 will route all traffic addressed to your website "www.ethanabowen.com" to the load balancer DNS "madeupelsdnsaddress.elb.amazonaws.com")."

# CloudFront
* AWS Global Internet, not public
* Origins - Source of data
* Distributions (Edge [Point of Presense], ~210 around the world) - ex. a83f20828f.cloudfront.net  can be put behind a custom domain. Cached from Source by configuration.
  * Behaviors
    * Path Pattern - if /dog then S3 else EC2
    * Viewer Protocol Policy - redirect to https/etc
    * Cache Policy - time-to-live, etc.
    * Origin Request Policy - configuration of security between Edge to Origin
* Regional Edge Cache (Only 1 per region) - longer TTL, sits between Origin and Edge.  Default 24 hours cached
* File is removed when TTL expires
* Low TTL is best for Dynamic Content.
  * Versioning is suggested as well
* TTL can be file-type specific
* Cache-Control max-age=(seconds) = CF will go get the file from origin about duration
* **Caching Based on Request Headers**
  * You can configure CloudFront to forward headers in the viewer request to the origin
  * CloudFront can then cache multiple versions of an object based on the values in one or more request headers
  * Controlled in a behavior to do one of the following:
    * Forward all headers to your origin (objects are **not cached**)
    * Forward a whitelist of headers that you specify
    * Forward only the default headers (doesn’t cache objects based on values in request headers)
* Signed Urls - Control over access to content.
  * Can specify beginning of expirtation date and time, IP addresses/ranges of users.
  * Can **not** be used for multiple files
* Signed Cookies - similar, used when you don't want to change URLs.
  * Can also be used when you want to provide access to **multiple restricted files**
* Origin Access Identity (OAI)
  * Locks S3 content to CloudFront only via a Bucket Policy that only allows the OAI associated with the CF Distribution to access it.
* WAF ACL can be attached to CF to prevent known malicious attacks at the EDGE
* SSl/TLS
  * Can have an SSL/TLS Certificate from AWS Certificate Manager but it MUST be issues in us-east-1.  CNAME can be use dto change default CF domain name.
  ![CloudFront SSL/TLS](/diagrams/cloudfront_ssltls.png)
* SNI - Server Name Indication - Multiple Domain names can be mapped to the same CloudFront distribution

# Lambda Edge
* Python/Node Lambda function sto customer the content CloudFront delivers
* **Executes functions closer to the viewer**
* Can be run at the following points:
  * After CloudFront receives a request from a viewer (viewer request)
  * Before CloudFront forwards the request to the origin (origin request)
  * After CloudFront receives the response from the origin (origin response)
  * Before CloudFront forwards the response to the viewer (viewer response)
* **Simply put, before and after CloudFront handles requests/responses**

# AWS Global Accelerator
* Networking service to optimize the sending of data to your applications via AWS Global Network
![AWS Global Accelerator](/diagrams/global_accelerator.png)
* Uses 2 AnyCast IP addresses - which can be used anywhere around the world
  * These must be whitelisted by the customer to be used
* Make sure you configure your Load Balancer and GA health checks
* **Automatic routing to the nearest region, with failover possible to other regions**

# EBS
* \# of IOPS is instanceIOPS * GiB.   So a 10 GiB volume with an instance of 50 IOPS can have 500 IOPS max.
* 4 classes of volumnes
  * 2 for high IO, low latency - Provisioned IOP SSD (io1), General Purpose SSD (gp2)
  * 2 for sequencial IO without need for latency, etc (maintain high queue length) - Throughput Optimized HDD (st1), Cold HDD (sc1) sc2
* Network Attached Storage - NIC -> <-Network Switch-> -> Network Attached Storage Server (NAS)
* Multi-Attach up to 16 EC2 instances (Nitro) to io1 volume in same AZ
* **st1 and sc1 can not be boot volumes!**
* Backup - Snapshot, point-in-time copy stored in S3 (regionally)
  * Ex1. Move to new AZ - backup and restore into new AZ
  * Ex2. Snapshot -> AMI -> Volume in new AZ
* **When encrypting (from a copy) Snapshots/Volumes/AMIs, you cannot move across regional boundaries, ony AZ.**
  * Snapshot of an encrypted volume is encrypted.
* **Basically, Encryption = cross-AZ, Unencrypted = cross-Region**  
* Data on an encrypted volume is encrypted-at-rest and encrypted-in-transit (between the instance and the volume)
* **Encrpytion by default** is a Region-specific setting
* EBS supports symetric CMK keys only
* Use Amazon Data Lifecycle Manager (Amazon DLM) to automate the creation of EBS snapshots
  * Can automate backups to another account for DR
  * Target resources by TAGS!
* RAID - Redundant Array of Independent Disks
  * Must be configured via OS
  * RAID 0 and 1 are available
  * RAID 0 - stripping, using 2 or more disks.
    * If 1 disk fails, the entire RAID set fails
    * Read and Write from multiple volumes
  * RAID 1 - Mirror data.  Data written to at least 2 disks at a time.

# EFS
* Uses NFS Protocol.
* Multiple EC2 instances can connect at the same time
* **Linux only**
* Can connect instance from other VPCs or VPN
* **Cross-region/accounts via Mount Target IP and peering connection**
* EFS encryption is are creation only
* File System Policy can be created to prevent root access, read-only access, anonymous access and/or enforce in-transit encrpytion for all clients.  Out of the box.

# Amazon FSx
* **Fully managed third-party file systems**
* Windows File Server for Windows-based applications.
  * **NTFS** file systems are accessible via **SMB** protocol from EC2 instances
  * AZ replications for HA
  * Multi-AZ Replication with active and standby file server options
  ![FSx Windows](/diagrams/fsx_windows.png)
* **Lustre for compute-intensive workloads**
  * ML, HPC, automation, modeling
  * **Works natively with S3**.  The objects in S3 are presented as if in file system.
  * POSIX compliant
  * VPN connection capible
  ![FSx Lustre](/diagrams/fsx_lustre.png)

# Storage Gateway
  ![Storage Gateway](/diagrams/storage_gateway.png)
* On-prem to (optional cached) Cloud data storage intergration/migration
* Can go directly to S3 Glacier Deep Archive
* Volume Gateway
  * Block-based volumes - iSCSI
  * Cached mode - stored in S3 and cache of most frequently accessed data is cached on-site
  * Volume mode - store on-site and is asynchronously backed up to S3
    * **EBS point-in-time snapshots**
    * Snapshots are incremental and compressed.
* Tape Gateway
  * **Provides low-latency access to data by caching frequently accessed data on-premises while storing the archive data securely and durable in Amazon cloud storage services.**
  * Caches virtual tapes on-premises for low-latency data access
  * Not suitable for large data.

# File Gateway
* Application Server -> File Gateway (NFS/SMB) (amount specified) --evicted--HTTPS --> S3 bucket

# Amazon RDS
* Enable Multi-AZ Deployment for automatic failover into another **AZ**.
  * HA during system upgrades like OS patching or DB Instance scaling.
  * Synchronous standby replica in different AZ
  * CNAME is switched from primary to standby instance.
* Enhanced Monitoring metrics for CW from RDS - OS Processes and RDS child processes
* IAM DB authentication exists - encryptes DB with SSL (Cert).
* AWSAuthenticationPlugin — an AWS-provided plugin (in MySQL) that works seamlessly with IAM to authenticate your IAM users.
* Doesn't scale like Aurora and is also only Relational implementations
* For SQL Server DB instances, RDS creates an SSL certificate for it.  Can be used to For SSL and Encrypt specific connections.
  * Use rds.force_ssl = true to for SSL communication only to the DB.  Reboot required.

# Aurora
* Multi-master setting adds the ability to scale out write performance across multiple AZ and provides configurable **read after write** consistency.
* On failover, without read-replicas, will createa  new DB Instance in the same AZ as the original instance and is done on a best-effort basis.
* MySql and PostgreSQL
* Up to 64 tebibytes (TiB)
* Automates and standardizes database clustering and replication
* Serverless - on-demand, auto-scaling configuration
* Provisioned - non-Serverless
* Reader Endpoint for expected larged Read requests

# DynamoDB
* Streams - capture table activity and automatically trigger the Lambda function
* Accelerator (DAX) - fully managed, HA, in-memory cache.  Milliseconds to Microseconds. 1 million+ response per second capable.

# CloudWatch Logs/Alarms
* Custom metrics can be created to restart EC2 instances, etc.
  * Standard resolution - every 1 minute
  * High resolution - faster, ec2 setting?
* Unified CW Agent - enables you to Collect log and metrics from **on-prem** servers

# Kinesis Data Streams
* Durable, sequence processing.
* Set of shards that has a sequence of data records, and each data record has a sequence number that is assigned by KDS
![Kinesis Data Streams](/diagrams/kinesis_data_streams.png)
* **UpdateShardCount** and **MergeShard** are valid commands that increase and decrease, respectively, data capacity and cost
* A new shard iterator is returned by every **GetRecords** call (as _NextShardIterator_), allowing you to use in the next **GetRecords** requests (as _ShardIterator_)
  * Shard iterators **expire** because you have not called **GetRecords** for more thatn 5 minutes, or because of some application freeze/restart.
  * Increasing the write capacity assigned to the shard table can reduce share iterator expiries.

# AWS Backup
* Automated cross-account backup of resources (ebs, efs, ec2, FSx, RDS, DynamoDB, Aurora)

# CloudTrail
* Organization trail - capture all accounts in an Organization, Admins cannot modify or delete the trail
* Trigger on Root user account API event - EventBridge rule that filters for root API events and configure an SNS notification

# Config
* Configuration audit. Configuration rules can be defined.
* Auto Remediation can take action on findings

# SQS
* Can set ApproximateAgeOfOldestMessage in CloudWath Alarm with ASG to ensure messages aren't getting old and are processed quickly.
* 14 day max!

# AWS Database Migration Service
* **Homo and Hetero -genious** database migration from on-prem to AWS cloud!
* Examples
  * Oracle -> Oracle
  * Oracle -> Aurora
  * On prem -> RDS or EC2
  * EC2 -> RDS
  * RDS -> RDS
  * **SQL <-> NoSQL**
  * text based targets
* **Heterogenious migrations** have two step progress migration processes
  * 1st - Run the **AWS Schema Conversion Tool** to convert the source schema and code to match target database
  * 2nd - Run the AWS Database Migration Service to migrate data from the source database to the target database

# AWS Systems Manager Run Command
* Remotely and securely manage the configuration of your managed instances (EC2 AND on-prem!)
* Intended for the automation of admin tasks or ad hoc changes at scale

# AWS DataSync
* Simplifies, automates, and accelerates moving of datasets to AWS
* Can be active datasets
* Can go directly to S3 Glacier Deep Archive

# Redis
* AUTH command requires password before granted access to Redis commands on a password-protected Redis server
  * Enable --transit-encrpytion-enabled and --auth-token parameters

# AWS GuardDuty
* Continuous-monitoring threat detection software

# Macie
* Scan S3 data for PII/PHI/etc and report on it.

# API Gateway
* Can cache and throttle calls

# Active Director Federation Service (AD FS)
* Basically, SAML federation with Microsoft AD

# AWS Resource Access Manager
* Resource Share - Share resources across AWS accounts of AWS Organizations
  * **Select** Resources -> **Specify** Principals -> **Share** Resources

# Amazon EMR cluster (S3/Storage Source -> format/process -> Redshift)
* "big data processing frameworks"
* "various business intelligence tools and standard SQL queries" to analyze the data.
* EMR will perform data transformations (ETL) and load the processed data into Amazon Redshift for analytic and business intelligence applications.

# WAF - common security protection (SQL-injection, cross-site scripting, filter out traffic patterns)
* Can be attached to ALB, API Gateway, CloudFront
* Web ACLs - rules that can match request attributes: URI, query-string, HTTP method, header key, etc.
* DDoS protection - use web ACLs with rate-based (5-minute period) rules **as part of your AWS Shield Advanced protection**
* Whitelist/Blacklist of IPs/etc.

# Snowball Edge
* Shippable Snowball device with computer and storage capabilities.
* Petabyte scale
* If it's going to take 1 week+ to uplaod your data, consider a Snowball device
