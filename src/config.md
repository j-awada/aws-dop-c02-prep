# AWS Config

With AWS Config, you can:

- Evaluate AWS resource configurations for desired settings
- Stream configuration changes and notifications to an Amazon SNS topic
- Get a snapshot of current configurations
- Retrieve historical configurations of 1 or more resources
- Receive a notification whenever a resource is created, modified or deleted
- View relationships between resources

An aggregator is an AWS Config resource type that collects AWS Config configuration and compliance data from:

- multiple accounts and multiple regions
- single account and multiple regions
- an organisation in AWS Organizations and all the accounts in that organisation

When you add a rule to AWS Config, you can specify when you want it to evaluate the rule i.e. the trigger. There are 2 types of triggers:

1. Configuration changes
2. Periodic

If both triggers are chosen, AWS Config invokes the Lambda function when it detects a configuration change and also at the frequency specified.

Note that AWS Config evaluates periodically and not in real-time. It continuously evaluates resources against rules. It triggers automatic remediation using an SSM Automation document. There is a list of remediation actions to use or you can create custom remediation actions using AWS Systems Manager Automation documents.

A conformance pack is a collection of AWS Config rules and remediation actions that can be deployed as a single entity in an account, region or across an organisation. They can ensure that all deployed infrastructure meets compliance and security standards automatically.

AWS Config can not be used to perform emergency maintenance on instances.
