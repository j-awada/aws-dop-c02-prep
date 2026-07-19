# AWS Systems Manager (SSM)

Can be used to configure, schedule, automate and execute operations on AWS resources. Resources include IAM users, groups and roles as well as Lambda functions, EC2 ASG and S3 buckets.

A Systems Manager Automation document defines the Automation workflow (the actions that Systems Manager performs on the managed instances and AWS resources). These documents can perform common tasks such as restarting 1 or more EC2 instances or creating Amazon Machine Images (AMI). Documents use JSON or YAML and include steps that run in sequential order.

SSM can automate common and repetitive IT operations and management tasks across AWS resources. This can be done using JSON documents or on-demand using the `RUN` command.

### Execution permissions

To use SSM, the host needs to have the SSM-agent installed along with proper IAM permissions. There are 2 specific roles:

* Administration role (in the central account): authorises SSM to deploy the execution down to target accounts.

* Execution role (in the target accounts): is the `AWS-SystemsManager-AutomationExecutionRole` role which would have permissions to perform actions like patching or terminating instances.

### Session Manager

SSM allows managing instances at scale without the need for bastions, SSH or remote sessions.

By default, AWS Systems Manager does not have permission to perform actions on your instances. You must grant access by using IAM. An instance profile that contains the AWS managed policy `AmazonSSMManagedInstanceCore` is needed to be attached to the EC2 instance for the Session Manager to work.

### State Manager

Enables configuration management such that EC2 instances or on-prem instances are consistently configrued.

### Patch manager

Allows applying OS or software patches to a large group of EC2 or on-prem instances.

* Patch Baseline: defines what patches get installed according to operating system and distribution.
* Patch Groups: act as grouping of compute resources i.e. what gets patched.
* Maintenance Windows: is the configuration item used to tie all this together.
* Run Command: `AWS-RunPatchbaseline` is defined within the maintenance window and is used to perform the task on the target group.

## Parameter store

The Parameter Store is a sub product of the Systems Manager (SSM).

You can create a standard-tier (up to 10,000 parameters at 4 Kb size) or advanced-tier (more than 10,000 parameters at 8 Kb size plus other features) parameter.

You can create a parameter eg. `/app/app_username` of type String, StringList or Encrypted via the Key Management Service (KMS) that can be used to store configuration information.

The Parameter Store does not provide dedicated storage with lifecycle management and key rotation, unlike the Secrets Manager.

## AWS Secrets Manager

It shares functionality with Paramter Store but is designed specifically for storing secrets (passowrds, API keys).

It can be used via the Console, CLI, API or SDK i.e. used from within applications. It supports the automatic rotation of secrets (via Lambda) and directly integrates with some AWS products for eg. when a secret is rotated, it is changed also inside an RDS (Relational Database Service).

Secrets Manager integrates with IAM and its secrets are encrypted using KMS. It can store secrets up to 12 Kb in size each.

<div align="center">
    <img src="./images/secrets_manager.png" alt="cloud services" width="500" />
</div>