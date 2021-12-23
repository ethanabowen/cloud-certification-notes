# Elastic Beanstalk
* Fully manages LBs, Subnets, ASGs, and Subnet configurations.
* Only have to upload your source code
* Complete resource control
* Beanstalk is free, like lambda
* Uses CloudFormation
* Versions can be deployed into Environments
* Environments (ec2 instances)
  * Web Server - receive web requests and send message to an SQS Queue
  * Worker Server - receive messages off of SQS Queue, process messages and do the work
* To migrate an RDS DB out of Beanstalk:
  * Take a snapshot of the RDS DB
  * Enable deletion protection on the RDS DB
  * Create a new environment without an RDS DB and point applications to the existing RDS DB
  * Perform a blue/green deployment and swap the new and old environments.
  * Terminate the old env
  * Delete the CloudFormation stack (will be in a DELETE_FAILED state).
* .ebextensions go in your source code that allow you to modify EB settings
  * option_settings - defines values for configuration options
  * Resource section - further customization of resources and ability to extend default configuration options.
  * Other sections: packages, sources, files, user, groups, commands, container_commands, and services
* HTTPS - assign service cert to the env's LB, assign cert to single instances, redirect via rule
* Dockerrun.aws.json - file that is used to describe Docker Containers configurations

# DynamoDB
* DynamoDB supports identity-based policies only! NOT RESOURCE-BASED POLICIES!
  * Can't add iam scope to tables or Dynamo resources specifically
  * Access is scoped to identity-based iam policies
* DynamoDB Streams captures a time-ordered sequence of item-level modifications
  * Available for 24 hours
  * Can view item as they were before and after change
  * Stream types:
    * KEYS_ONLY - Only the key attribute of the modified item.
    * NEW_IMAGE - The entire item, as it appears AFTER it was modified.
    * OLD_IMAGE - The entire item, as it appears BEFORE it was modified.
    * NEW_AND_OLD_IMAGES - Both the new and the old images of the item.
* Transactions
  * Manage complex business workflows that requires adding, updating, or deleting of multiple items as a single, all-or-nothing operation.
  * API - TransactWriteItems, TransactGetItems (group of gets)
  * No additional cost
  * DynamoDB performs 2 reads or writes for every item in the transaction.
    * One to prepare the transaction
    * One to commit the transaction
* DynamoDB Session Handler is a customer session handler for PHP that allows developers to use DynamoDB as a session store.
  * Can alleviate distributed web application session management issues by moving sessions off of the local file system and into a shared location
* Scans
  * Searches the entire table or secondary index, THEN filters out values to provide the result you want
  * Avoid on large tables
  * Can use up the provisioned throughput for a large table or index in a SINGLE operation!
  * To minimize the impact of a scan on a table's provisioned throughput, Reduce page size (LIMIT keyword)
    * The LIMIT makes for fewer read operations and creates a "pause" between each request
  * Parallel scan when:
    * Table is 20GB+
    * Normal scans are slow
    * Table's provisioned read throughout is not being fully used.
* Time to Live (TTL) - auto purge of expired items
* Read Capacity Unit (RCU) - represents 1 Strongly Consistent read per second OR 2 Eventually Consistent reads per second, for an item up to 4KB in size.
  * Calculation: Item Size / 4KB * required RCU/s   Ex.   8KB / 4KB * 3 RCUs = 6 RCUs
  * Example: Table has 10 provisioned RCUs.  That means you can perform 10 Strongly Consistent Reads per seconds, or 20 Eventually Consistent Reads per seconds, for items up to 4KB.
  * Item sizes for reads are rounded up to the next 4KB amount.
    * Example: 3,500 bytes = 4KB  So same amount of RCUs for SC and EC
    * Example: 1KB = 4KB Same. (1 SC RCU, 1 (Minimum) EC RCUs)
    * Example: 128KB (32 SC RCU, 16 EC RCUs) Starts to scale well for ECs on Large Scans/Queries
* WCU Calculation: Average Item Size * required WCU/s   Ex.   9KB * 2 WCUs = 18 RCUs
* Conditional Writes - Protection of data integrity by applying a constraint in the code to write only if attribute has expected value.
  * If Bob and Alice update the item "at the same time" (or really close to it), one of their writes needs to probably fail.
* GetItem has a ConsistentRead true(Strong)/false(Eventual) option, false by default.
* DynamoDB currently retains up to 5 minutes (300 seconds) of unused read and write capacity which can be consumed quickly.
* API BatchWriteItem – can put or delete up to 25 items in one call (max 16MB write / 400KB per item).  
* Optimistic locking - ensure that the client-side item being updated/deleted is the same as the item in DynamoDB.
* Security
  * VPC endpoints are available.
  * Encryption at rest - KMS enabled
  * Encryption in transit - SSL / TLS
  * "Encryption Client" -  client-side encryption library that enable protection of data before submitting it to DynamoDB
* BatchGetItem - up to 100 items and/or 16MB.  Max of 2 tables.  Reduces latency by sending all the calls at once to DynamoDB.    

# Lambda
* /tmp - persistent data between lambda functions
  * Should only be used for data that can be regenerated or for operations that require a local filesystem
  * Content remains when the execution context is frozen, providing transient cache that can be used for multiple invocations.
  * It is not guaranteed that the execution context will be reused so the data could be lost
  * Up to 512MB
* Encryption Headers
  * Encrypt env vars before passing them to into Lambda.
  * Enhanced security to prevent secrets from being displayed in Lambda console or function configurations returned by the Lambda API.

# RDS
* Read replicas can be directly queried (think BI/reporting) via a read replica connection string

# API Gateway
* Stages can be used as a mechanism for versioning of APIs (there are other ways to do this as well).
  * Other options with Stages:
    * Enable Caching,
    * Customize request throttling
    * Configure logging
    * Define stage variables
    * Attach a canary release for testing
*  Usage plan - Manages who can access API stages/method, how much, and how fast via API keys
  * API keys - alphanumeric string values.
    * Can be imported from an eternal source (file)or generated on your behalf by API Gateway
    * The CreateUsagePlanKey API must be called for Newly created keys to be used.  This associated them with an existing Usage Plan.
* HTTP integration is fully custom, where HTTP_PROXY is setup for you.
* Logging:
  * Access - logs who has accessed your API and how they accessed it.
    * You manage the CloudWatch system and information passed down.
  * Execution - manages CloudWatch logs (groups, streams, and reporting requests/responses).
    * Includes errors/execution traces (parameter/payload values)
    * Data used by Lambda Authorizers
    * Whether API keys are required
    * Whether usage plans are enabled
    * etc.

# CloudFront
* E2E encryption is done by configuring the View and Origin Protocol Policy
  * Example of what an E2E communication with CloudFront looks like:
    1. A viewer submits an HTTPS request to CloudFront. There’s some SSL/TLS negotiation here between the viewer and CloudFront. In the end, the viewer submits the request in an encrypted format.
    2. If the object is in the CloudFront edge cache, CloudFront encrypts the response and returns it to the viewer, and the viewer decrypts it.
    3. If the object is not in the CloudFront cache, CloudFront performs SSL/TLS negotiation with your origin and, when the negotiation is complete, forwards the request to your origin in an encrypted format.
    4. Your origin decrypts the request, encrypts the requested object as the response, and returns the object to CloudFront.
    5. CloudFront decrypts the response, re-encrypts it (with the CF SSL encryption), and forwards the object to the viewer. CloudFront also saves the object in the edge cache so that the object is available the next time it’s requested.
    6. The viewer decrypts the response.
* Modifying CF responses - You can change the responses (body and http status) via origin-response triggers.
  * If CF receives an HTTP response from the origin server, AND IF an origin-response trigger is associated with the cache behavior, you can modify the HTTP response to override what was returned from the origin.

# VPC Flow
* Network logs, sent to S3 or CloudWatch, that can be used for network analysis

# Lambda
* Doesn't have a VPC Endpoint?  Instead you add the function to the VPC through the function's configuration
* Alias-Version weighting - you can point an alias to multiple versions of a function and assign a weight to direct certain amounts of traffic to each version.

# X-Ray
* Annotations - K/V pairs (indexed) used with filter expressions.  Can be String, Number, or Boolean type.  Use annotations to record data that you want to group in the Console or the GetTraceSummaries API.
* Metadatas - K/V pairs (not indexed) used with filter expressions.  Can be ANY type, including objects and lists.  Not usable for searching, only for storing data in the trace
* Subsegments - Provides more granular timing and addition information related to AWS Service, External Service, or DB calls.
* EC2 daemon - required to be installed and running (and permissions via role/policy to upload to X-Ray).  A user script can run the daemon automatically when the instance is launched.  It uses the AWS SDK.

# CodeCommit
* Version Control Service that hosts private Git repositories.
* You configure your Git client to communicate with CodeCommit repositories.
* You provide IAM credentials that CodeCommit can use to authenticate you. IAM supports CodeCommit with three types of credentials:
  * Git credentials - IAM-generated username/password pair that can be used to communicate with CodeCommit repositories over HTTPS.
  * SSH keys - locally-generated public-private key pair that can be associated with your IAM user to communicate with CodeCommit repositories over SSH.
  * AWS access keys - can use with the credential helper included with the AWS CLI to communicate with CodeCommit repositories over HTTPS.
* put_file - command that pushes and commits a file to a repo via the AWS SDK

# CodeDeploy (Lambda)
* LambdaCanary10Percent5Minutes - means 10% at the beginning, then the rest after 5 minutes.
* LambdaLinear10PercentEvery1Minute - means 10% every minute until 100% is cut over

# CodePipeline
* To trigger a Lambda that notifies of stage changes, use a CloudWatch Event rule and site CodePipeline as the event source.

# CodeStar
* Unified user interface, enables easy management of software development activities in one place.
* Setup entire continuous delivery toolchain in minutes.
* Includes integrated issue tracking capability power by JIRA!

# Cloud9
* Integrated IDE!

# CloudFormation
* Mappings - Matches a key to a corresponding set of names values.  Ex. region, you can associate a value based on region "Mappings.RegionMap.region"
* package - command uploads artifacts to s3 returning modified template with injected references to artifacts.
* Lambda function code can be directly INLINE.  Doesn't have to be a seperate file/zip.
  * Resources.MyFunction.Properties.Code.FunctionName: <code>
* StackSets - Cross-Account, Cross-Region Stack CRUD operations from an Admin account
* AWS Serverless APplication Model syntax must include Resource (what is to be created) and Transform (CF version) sections
* S3 is used as a data-store for references resources to be used when executing the CF package.
  * Ex.  Lambda function is Zipped into an S3 bucket and it's location is referenced in the CF AWS::Lambda::Function portion of the CF template.

# CloudWatch
* Cross-Account, Cross-Region dashboards - ya, it's a thing.
* Namespace - container for CloudWatch metrics
* High resolution monitoring - retrieve logs in periods of 1/5/10/30/60 seconds
* CW Logs - Error Reporting - Code-less monitoring of custom values in logs (such as NullPointerException, 404).  When a term is found, CW Logs reports to CW Metrics the specified data.  Log data is E2E encrypted.

# SQS
* ReceiveMessage API - retrieve up to 10 messages at a time from a queue
* Long polling - ReceiveMessage API set WaitTimeSeconds to 20 (max)

# Kinesis
* Kinesis Client Library (KCL) - layer of abstraction specifically for processing data in a consumer role.
  * Takes care of many of the complex tasks associated with Distributed computing such as:
    * Load Balancing
    * Responding to instance failures
    * Checkpointing processed records
    * Reacting to resharding
* Kinesis Data Streams API - manage Create and Reshard of streams as well as Getting/Putting of records.  And many more operations.
* Firehose - managed stream, handles shard and resource management

# Secrets Manager
* Rotates credentials, NOT encryption keys

# KMS/Encryption
* Envelope encryption - The practice of encrypting plaintext data with a data key, and then encrypting the data key under another key.
  * 3 keys total - Customer/AWS Managed Key, Encrypted Data Key (which then is decrypted to the) Plaintext Data Key.
  * Best practice is to remove the Plaintext Data Key as soon as possible from memory (after using it).
  * Master Key --Encrypts--> Data Key --Encrypts--> Data
  * Think "Unlock the envelope with a key, use the key IN the envelope to encrypt/decrypt the data"
* Data encryption keys - type of key that is fully managed by customer and can be used for large amounts of data.
* Customer Master Key (CMK) - encrypts up to 4KB per encrypt/decrypt/re-encrypt operation
* GenerateDataKey API - generates a unique symmetric data key
* STS decode-authorization-message API -  decodes additional information about the authorization status of a request from an encoded message returned in response to an AWS request.  Ex. "An error occurred (UnauthorizedOperation) when calling the RunInstances operation: You are not authorized to perform this operation. Encoded authorization failure: <truncated encrypted message>"

# Cognito
* Cognito Sync - Cross-platform syncing of up to 1MB of user-oriented data
* Unauthenticated identities - Identity Pools can support unauthenticated identities by providing a unique identifier and AWS credentials for users who do not authenticate with an identity provider.
* Developer Authenticated Identities - register and authenticate users via your own existing authentication process, while still using Amazon Cognito to synchronize user data and access AWS resources.

# ECS
* Task Placement Constraint - specified when either running a task or creating a new service
  * Ex. distinctInstance or memberOf
* Cluster Query Language is used to build syntax around where/how to place Containers in ECS
