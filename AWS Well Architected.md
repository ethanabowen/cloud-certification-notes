## Operation Excellence
### Organization

A Organization's organization is primarily up to the C-suite and Management, then trickles down to the individual teams.
It is their shared resposibility at communicating business needs and everyones individual role to meet those needs.

#### Governance
* Recognition of regulatory obligations
* Documentation and communication of obligations
* Periodic review of obligations and current state

#### Education

Look for answers to questions:
  * AWS Knowledge Center
  * AWS Discussion Forums
  * AWS Support Center
  * AWS Documentation
  * AWS Blog
  * AWS Podcast
  * AWS Cert Training

Dedicated structured time for learning is key to being successful long term.
Seek diversity across the organization to gain more perspective, allowing for more veted-benefitial solutions and increased innovation.


### Prepare

#### Metrics
* Gather metrics across all workflows and their components that expose internal state needed for support and monitoring.
* Use for insight (analysis) and situational awareness of how components are used and operating.
* Use Static Code Analysis (SonarQube, etc) for insight into code health and operational risks.  Industry suggests ~80% coverage?
* Tag resources for identification to help with cost accounts, access controls, etc.

#### Deployment Preparation
* Fail fast, fail often.
* Consider a deployment checklist for consistency.
	* Use "pre-mortems" to identify and anticipate failures, correcting where necesssary!
	* Plan for reverts ahead of each deployment.
* Evaluate the operational readiness of your worklord, processes, procedure, and personnel to understand operational risks related to your workload.
	* Who do I contact for deployments?
	* How far in advance?
	* What information do I need to give them for operational success?

#### Deployment Operational Repeatability
* Use deployment runbooks for each product offering detailings in depth happy/grumpy path scenarios and more.
* Operations as Code for applications, infrastructure, policy, governance, and operations.  Ex. CloudFormation (CDK), Terraform, Serverless


### Operate

Operational health includes the health of workloads and success of the operation activities performed in support of those workloads (deployments, incidents, etc).
* Use runbooks for well-understood operational events
* Use playbooks to aid in investigation and resolution of issues.
	* Understand when to raise alerts to team and management


Measure health against Service Lifetime Agreements (SLAs) such as:
  * API Response time
  * Product up time (99.99999% aka 5 9s)
  * Deployment cadence.  Customers might expect or need new features/bug-fixes on a set scheduled duration.
 
Capture Operation and Workload metrics to ensure there is visibility into occuring events so that you can take appropriate action.
Communicate the operational status of workloads via dashboards/notifications to the appropriate audience (customers, business, developers, operations).

### Evolve
