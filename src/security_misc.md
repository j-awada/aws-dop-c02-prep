# Security practices and automation

## AWS Systems Manager (SSM)

Can be used to configure, schedule, automate and execute operations on AWS resources. Resources include IAM users, groups and roles as well as Lambda functions, EC2 ASG and S3 buckets.

A Systems Manager Automation document defines the Automation workflow (the actions that Systems Manager performs on the managed instances and AWS resources). These documents can perform common tasks such as restarting 1 or more EC2 instances or creating Amazon Machine Images (AMI). Documents use JSON or YAML and include steps that run in sequential order.

To use SSM, the host needs to have the SSM-agent installed along with proper IAM permissions.

SSM can automate common and repetitive IT operations and management tasks across AWS resources. This can be done using JSON documents or on-demand using the `RUN` command.

### Session Manager

SSM allows managing instances at scale without the need for bastions, SSH or remote sessions.

By default, AWS Systems MAnager does not have permission to perform actions on your instances. You must grant access by using IAM. An instance profile that contains the AWS managed policy `AmazonSSMManagedInstanceCore` is needed to be attached to the EC2 instance for the Session Manager to work.

### State Manager

Enables configuration management such that EC2 instances or on-prem instances are consistently configrued.

### Patch manager

Allows applying OS or software patches to a large group of EC2 or on-prem instances.

* Patch Baseline: defines what patches get installed according to operating system and distribution.
* Patch Groups: act as grouping of compute resources i.e. what gets patched.
* Maintenance Windows: is the configuration item used to tie all this together.
* Run Command: `AWS-RunPatchbaseline` is defined within the maintenance window and is used to perform the task on the target group.

## Amazon Inspector

Is an automated security assessment service that scans EC2 instance and container images for vulnerabilities and deviations from best practices. After performing an assessment, Amazon Inspector produces a detailed list of security findings prioritised by level of severity.

Amazon Inspector security assessment helps check for unintended network accessibility of EC2 instances and for vulnerabilities on those EC2 instances. Assessments are offered as pre-defined rules packages mapped to common security best practices and vulnerability definitions. Examples of built-in rules include checking for access to EC2 instances from the internet, remote root login being enabled, or vulnerable software versions installed.

## AWS Shield

Is a service suited for preventing DDoS attacks on AWS resources. It is available in 2 tiers: Standard and Advanced. AWS Shield Standard is included automatically and transparently in your Elastic Load Balancing load balancers, CloudFront and Route 53 at no additional costs.

## Amazon Macie

Is an ML-powered security service that helps you prevent data loss by automatically discovering, classifying and protecting sensitive data stored in Amazon S3. Macie uses machine learning to recognise sensitive data such as personally identifiable information (PII) or intellectual property. It provides visibility into where this data is stored and how it is being used in your organisation.

## AWS GuardDuty

Is a threat detection service that continuously monitors AWS accounts, workloads and data for malicious activity and unauthorised behaviour. It uses machine learning and threat intelligence. GuardDuty generates a finding whenever it detects unexpected and potentially malicious activity in the AWS environment.

Findings from Amazon GuardDuty can be published to Amazon EventBridge as events that can be used to trigger a Lambda function which will send notifications to the external messaging platform.

Amazon Detective can provide analysis information related to a given finding.

<div align="center">
    <img src="./images/guardduty.png" alt="AWS GuardDuty" width="500" />
</div>

## AWS Trusted Advisor

Provides realtime guidance on provisioning resources according to AWS best practices.

It provides a number of checks in 5 major areas:

* Cost optimisation
* Performance
* Security
* Fault tolerance
* Service limits

It has 7 core checks that are free at a basic/developer tier:

* S3 bucket permissions check (not objects)
* Security groups check for ports with unrestricted access
* IAM use
* If root account has MFA enabled
* Checks permissions on EBS public snapshots
* Checks permissions o RDS public snapshots
* Identifies if you are over 80% usage of the 50 most common services

## WAF (Web Application Firewall)

Is the AWS implementation of Layer 7 web application firewall; capable of understanding HTTP(S). It helps protect your web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources.

WAF can protect global services like CloudFront but also regional services like ALB (Application Load Balancer).

Web ACL is associated with a WAF distribution and has Rules and Rule Groups that control how the WAF reacts.

AWS WAF provides control over which traffic to allow or block to a web application by defining customisable web security rules. This includes blocking common attacks like SQL injections or cross-site scripting. New rules can be deployed within minutes.

<div align="center">
    <img src="./images/network_firewall.png" alt="AWS Network Firewall" width="500" />
</div>
