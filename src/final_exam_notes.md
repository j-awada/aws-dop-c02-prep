# Final exam notes

## CI/CD

* For the CodeBuild service to properly download encrypted files from an S3 bucket, the CodeBuild service should have the right role permissions for the KMS key used for encryption.

* To verify the health of a Fargate application, look for CodeDeploy successfully deploying as well as no errors in the CloudWatch alarms.

* For a CI/CD pipeline to deploy resources in multiple AWS accounts, create a cross-account IAM role in the target account that trusts the pipeline account. Also update the pipeline to assume the cross-account role in the target account.

* To prevent accidental _replacement_ of an RDS instance during a CloudFormation stack update, use CloudFormation Stack Policy to prevent updating the database.

* Terraform is cloud-agnostic and supports multiple providers incl. AWS, Azure and on-prem. It's the most flexible IaC tool in hybrid cloud or multi-cloud scenarios.

* Terraform workspaces can isolate environments and `assume_role` allows secure cross-account access using STS.

* Manual approval actions pause a CodePipeline pipeline such that a human can approve a release to prod.

* CodeBuild supports local caching of dependency directories (eg. `.npm`, `pip`) to speed up builds by avoiding redownloading packages.

* AWS CloudTrail records API-level actions including pipeline stage executions, approvals and deployment events enabling full auditability of the CI/CD flow.

* A pre-traffic hook in a blue/green CodeDeploy deployment allows you to run logic (eg. tests or config checks) before shifting any traffic to the new environment.

## Roles and organisations

* To deploy standardised IAM roles to all current and future accounts within an organisation, deploy a CloudFormation StackSet targeting the organisaton root.

A StackSet is like a stack but it deploys accross multiple regions and accounts.

## Automation

* To manage security patches for EC2 instances and on-prem servers, install the AWS Systems Manager agent and create a Hybrid Activation.

* State Manager allows adminstrators to apply and maintain consistent configurations using pre-defined or custom SSM documents ensuring automated and continuous compliance across accounts.

## Observability

* Memory metrics are not available by default in CloudWatch for EC2 instances. A CloudWatch agent must be installed and configured on the instance to obtain internal metrics like memory utilisation, disk usage, process health, etc.
