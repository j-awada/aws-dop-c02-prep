## IAM (Identity and Access Management)

An AWS account is a container for identities and resources. Every AWS account has a unique email which is that of the root user of the account.

With IAM, you can create users, groups and roles. Multiple identities can be created within a single AWS account. A max of 5000 IAM users are allowed per account. And an IAM user can be a member of max 10 groups.

## Identity types

* **users**: for humans or applications
* **groups**: a collection of related users eg. developers
* **roles**: for AWS services or for getting external access

### Policies

Policies can be created and attached to users, groups or roles to allow or deny access to AWS services. It contains statements which grant or deny permissions to AWS services.

A statement object consists of: 

* Sid: statement ID
* Effect: is either allow or deny
* Action: a list of actions/operations on a service
* Resource: a list of resources/names

A policy can be:

* **managed**: used for multiple identities
* **inline**: used for exceptional allow/deny to a managed policy


### IAM Groups

* Are containers for IAM users
* Groups can not be nested in other groups
* In a policy, users and roles can be referenced using their ARN. Groups however can not be referenced in policies because groups are not a true identity. Groups can have permissions attached to them.

### IAM Roles

* Used by multiple principals, humans, service, applications or an unknown number of principals
* Roles are generally used on a temporary basis i.e. borrowing permissions for a short time
* Roles can have identities, premissions, policies attached to them
* Roles can be referenced in resource policies
* Roles can have 2 types of policies attached: **Trust policy** and **Permissions policy**

**Trust policy** controls which identities can assume that role.

**Permission policy** controls access to resources.

Scenarios for using role:

* In case of AWS services like Lambda where it needs access rights to perform actions. You should avoid hardcoding access keys into the Lambda function. A role is created and the Lambda function is added to its Trust policy. This role has a Permissions policy which grants access to AWS products and services. The `sts::AssumeRole` is then used by the Lambda function.

* External identity. To allow users with external identities outside of AWS to do single sign-on. This use case enables exceeding the 5000 user-limit enforced on AWS accounts. In this example, a role can be assumed by an external identity.

### ARN (Amazon Resource Name)

Used to uniquely identify resources within an AWS account.

It can have the following structures:

* `arn:partition:service:region:account-id:resource-id`
* `arn:partition:service:region:account-id:resource-type/resource-id`
* `arn:partition:service:region:account-id:resource-type:resource-id`

Eg. `arn:aws:s3:::my-bucket/*`
