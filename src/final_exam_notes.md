# Final exam notes

## CI/CD

### CodeBuild

* For the CodeBuild service to properly download encrypted files from an S3 bucket, the CodeBuild service should have the right role permissions for the KMS key used for encryption.

* CodeBuild supports local caching of dependency directories (eg. `.npm`, `pip`) to speed up builds by avoiding redownloading packages.

* Parameter masking in CodeBuild redacts environment variables marked as secrets, preventing them from being displayed in logs or console output.

### CodeDeploy

* To verify the health of a Fargate application, look for CodeDeploy successfully deploying as well as no errors in the CloudWatch alarms.

* A pre-traffic hook in a blue/green CodeDeploy deployment allows you to run logic (eg. tests or config checks) before shifting any traffic to the new environment.

* A CodeDeploy strategy that is efficient for updating thousands of EC2 instances with minimal service disruption is the rolling update strategy.

    Rolling deployments divide EC2 instances into batches, updating them incrementally to ensure that the application remains available during the rollout.

* Registering on-prem servers with CodeDeploy:
    - Create an IAM role that the on-prem instances will assume
    - Generate temporary credentials for individual instances using AWS STS
    - Set up a Cron job to refresh those credentials regularly
    - Install CodeDeploy agent on the on-prem servers
    - Register the on-prem servers with CodeDeploy
    - Create tags for the on-prem servers
    - Set up a deployment group based on the tags

### CodePipeline

* For a CI/CD pipeline to deploy resources in multiple AWS accounts, create a cross-account IAM role in the target account that trusts the pipeline account. Also update the pipeline to assume the cross-account role in the target account.

* Manual approval actions pause a CodePipeline pipeline such that a human can approve a release to prod.

* AWS CloudTrail records API-level actions including pipeline stage executions, approvals and deployment events enabling full auditability of the CI/CD flow.

* Canary deployment helps minimise risk by routing a small portion of the traffic to the new version before scaling up.

* CodePipeline offers a feature to retry individual failed stages allowing developers to fix and resume deployments without re-running the full pipeline workflow.

* CodePipeline integrates with external systems using SNS or Lambda allowing teams to implement Slack, Jira or custom approval workflows between pipeline stages.

* CodePipeline can only have one source stage. To trigger CodePipeline when code is pushed in more than 1 repo, an EventBridge rule can be created for each repo that can trigger the same CodePipeline.

### IaC

* To prevent accidental _replacement_ of an RDS instance during a CloudFormation stack update, use CloudFormation Stack Policy to prevent updating the database.

* Terraform is cloud-agnostic and supports multiple providers incl. AWS, Azure and on-prem. It's the most flexible IaC tool in hybrid cloud or multi-cloud scenarios.

* Terraform workspaces can isolate environments and `assume_role` allows secure cross-account access using STS.

* Remote state locking in Terraform prevents simultaneous apply operations from different sources avoiding race condition and corrupted infrastructure state.

* To scan IaC templates for security misconfigurations before deployment, use static analysis tools like tfsec (Terraform) and cfn-nag (CloudFormation) to scan for vulnerabilities, missing encryption, open ports or IAM risks.

* During IaC migration, the Terraform `import` command lets you bring manually created or existing AWS resources under Terraform management without recreating or destroying them.

## Lambda

* Lambda functions are region-based and automatically use multiple AZs.

* For non-zero downtime when deploying Lambda updates, use a weighted alias that allows controlling traffic, shifting to new Lambda versions and reducing deployment risks.

* To implement canary deployment for a Lambda function, CodeDeploy natively supports Lambda canary deployments with built-in traffic shifting and automatic rollback based on CloudWatch alarms. The `Canary10Percent5Minutes` perference shifts 10% of traffic initially then completes the deployment after 5 minutes if no alarms trigger.

## Roles and organisations

* To deploy standardised IAM roles to all current and future accounts within an organisation, deploy a CloudFormation StackSet targeting the organisaton root.

A StackSet is like a stack but it deploys accross multiple regions and accounts.

## Automation

* To manage security patches for EC2 instances and on-prem servers, install the AWS Systems Manager agent and create a Hybrid Activation.

* State Manager allows adminstrators to apply and maintain consistent configurations using pre-defined or custom SSM documents ensuring automated and continuous compliance across accounts.

* Amazon ECR supports image scanning via Amazon Inspector or other integrated tools to detect known vulnerabilities in containers before deployment.

* A conformance pack is a collection of AWS Config rules and remediation actions that can be deployed as a single entity in an account, region or across an organisation. They can ensure that all deployed infrastructure meets compliance and security standards automatically.

* EC2 Auto Recovery monitors health and recovers the instance in the same AZ. It ensures quick recovery without manual intervention.

* An ECS service maintains the desired task count and automatically replaces failed tasks when integrated with health checks.

* Elastic Disaster Recovery (DRS) can be set up on source servers to initiate secure data replication. This minimises downtime and data loss reducing both RTO and RPO significantly.

* Route53 health checks with latency routing provide automated failover and route users to the nearest healthy region.

* Note that AWS Config evaluates periodically and not in real-time. It continuously evaluates resources against rules. It triggers automatic remediation using an SSM Automation document.

* AWS Config can not be used to perform emergency maintenance on instances.

* To ensure that an application does not experience downtime during database credentials rotation, a multi-user rotation strategy is used where the Secrets Manager creates 2 database users and alternates between them during rotation.

## Security

* For a web app to maintain performance during a DDoS attack, CloudFront with WAF can be used to filter malicious traffic.

## Observability

* Memory metrics are not available by default in CloudWatch for EC2 instances. A CloudWatch agent must be installed and configured on the instance to obtain internal metrics like memory utilisation, disk usage, process health, etc.

* X-Ray helps trace and visualise requests across microservices providing insight into performance bottlenecks and dependencies.

* CloudWatch Logs Insights allows teams to query logs and detect anomalies using pattern recognition and statistical analysis.

* CloudWatch log subscriptions can be used to route logs from multiple accounts to a centralised logging account for auditing and analysis.

* CloudWatch Container Insights provides granular metrics like memory and CPU for ECS tasks, making it suitable for memory-related monitoring.

* CloudWatch Anomaly Detection uses ML models to baseline normal metrics behaviour and detect outliers without static thresholds.

* To collect application logs and metrics from all containers, install Fuent Bit which can aggregate and forward logs and metrics from containers to CloudWatch enabling centralised logging.

* VPC Flow Logs is a feature that can be enabled on a VPC, subnet or Elastic Network Interface (ENI) so that network traffic can be logged to CloudWatch Logs.

* To improve observability, use X-Ray and OpenTelemetry (OTel) which provide deep observability for distributed services and improve trace analysis.

* In case of a custom application metric that is not supported natively in CloudWatch but needs to be monitored, the metric can be published from the application using the CloudWatch `PutMetricData` API.
