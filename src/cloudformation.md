## Cloudormation (CFN)

Cloudformation allows automating infrastructure provisioning using JSON or YAML templates. A template; which can be reusable, defines logical resources and results in the creation of a stack.

The stack definition will create physical resources based on the logical resources. If the stack definition is updated, the physical resources will also change.

Templates can take parameters i.e. they accept input. Parameters are defined in the template and can have a type for eg. an EC2 image. Pseudo parameters are defined by AWS. They are injected by AWS into the template eg. Region, stack ID, stack name, account ID, etc.

Cloudformation does things in parallel which is why directives like `DependsOn` can be used to define an order of execution.

Cloudformation can be configured to wait until a number of signals are successful before returning the `CreateComplete` signal.

### Dynamic references

Provide a compact and powerful way to specify external values that are stored and managed in other services such as the Systems Manager Parameter Store, in your stack templates. When dynamic references are used, CloudFormation retrieves the value of the specified reference when necessary during stack and change set operations.

CloudFormation currently supports the following dynamic reference patterns:

* ssm, for plaintext values stored in AWS Systems Manager Parameter Store
* ssm-secure, for secure strings stored in AWS Systems Manager Parameter Store
* secretsmanager, for entire secrets or specific secret values that are stored in AWS Secrets Manager

### Intrinsic functions

* Allow you to gain access to data at runtime i.e. when the template is being used to create a stack, for example:
    - `Ref` and `Fn::GetAtt`
    - `Fn::Join` and `Fn::Split`
    - `Fn::GetAZs` and `Fn::Select`
    - `Fn::`(IF, And, Equals, Not, Or)
    - `Fn::Base64` and `Fn::Sub`
    - `Fn:Cidr`
    - `Fn::Import Value`
    - `Fn::FindInMap`
    - `Fn::Transform`

### Mappings

* Is an object in the template eg. map the prod env to a specific database config
* Mappings use the `!FindInMap` intrinsic function

### Outputs

* Provide status information or how to access services created by a stack
* Outputs are optional

### Nested stacks

* There is a limit of 500 resources per stack and nested stacks can be used to overcome this resource limit
* Stacks are isolated and resources in them can't be referenced
* A root stack is the stack that gets created first
* A parent stack usually has nested stacks
* You can define a stack as a logical resource and make it refer to a YAML template
* Use nested stacks when resources are lifecycle-linked i.e. create and deleted together

**Cross-stack references**

* Is used when we want to create a shared resource eg. VPC usable by other stacks
* Stacks are isolated meaning things in a stack can't be referenced by another i.e. the `!Ref` function can't be used, with the exception that root stacks can reference the output of a stack
* A use case of this is a VPC stack with a long lifecycle and app stacks that use it but have a shorter lifecycle
* Outputs of a stack can be exported making them visible to other stacks. You can then use `Fn::ImportValue` in another stack instead of `!Ref` in the same stack

### Stack sets

* Allow you to deploy CFN stacks across many accounts and regions
* Stack sets are containers in an admin account. They contain stack instances that reference stacks

### CFN DeletionPolicy

* Deleting a stack causes deleting a physical resource. This can cause data loss for eg. databases
* The deletion policy can be defined on a resource in a CFN template to specify an action for when the resource is deleted eg. Delete, Retain, Snapshot

### CFN stack roles

* By default, CFN uses the permissions of the logged in identity
* CFN can assume another role to gain permissions
* We have a role attached to a stack
    - a user creates a template with a role attached
    - this user has `PassRole` permissions but doesn't actually have permissions to create the stack
    - the role can be created by an admin

### CFN Init, cfn-init and cfn-hup

* Is a configuration management system
* The CFN Init can be defined in the metadata of a logical resource in a template. `UserData` can be passed which contains `cfn-init` used to implement the config we define
* `cfn-init` only runs once and doesn't run again when the stack is updated
* `cfn-hup` can point at a logical resource in a stack to detect changes in its metadata. When it detects change it can run things like `cfn-init`

### Notes on creating CFN templates

* The `UserData` section serves as an init script that runs automatically when an EC2 instance launches for the first time. It executes at boot time
* CFN waits for a signal from the EC2 instance. As soon as the instance is launched, CFN marks Creation Complete even though the `UserData` script is still running i.e. instance bootstrapping hasn't finished.
    - here you can use `CreationPolicy` and `cfn-signal` to signal to Cloudformation when bootstrapping the instance is finished
* The `UserData` part is only executed once when the instance launches. If the `UserData` is changed and the stack is updated, the instance won't rerun this configuration
* Use the `cfn-init` feature and `cfn-hup` to rerun `cfn-init` on metadata changes

### CFN change sets

* Provides an overview of the changes that are to be applied to a stack. You can preview many change sets for a stack. The chosen change set can then be applied to the stack

### CFN custom resources

* CFN doesn't support creating all AWS products
* Use custom resources to compensate for what CFN does not support natively
    - eg. a Lambda function used to populate or remove items from a bucket. This is because CFN can't delete buckets that have objects in them. The Lambda function receives a CREATE or DELETE event and then uploads or deletes bucket items. The custom resource references both the Lambda function and then bucket.


### CFN drift detection

* When the logical resources and the physical resources in a stack no longer match
* CFN offers drift detection
* What can be done is:
    - removing the logical resources while keeping the physical resources in place and then importing those physical resources to bring back into sync the relationship between logical and physical resources

### Exam notes

* If a multi-region deployment via StackSets fails in the first region and the engineer wants to ensure that future failures stop the rollout immediately, the `FailureTolerance` parameter can be decreased to 0 or 1 so that a single failure terminates the entire global deployment execution loop.

* If a CloudFormation stack deployment fails and gets permanently stuck in `UPDATE_ROLLBACK_FAILED` because an S3 bucket was manually deleted, execute the `ContinueUpdateRollback` action via the CLI/Console, specify skipping the deleted S3 bucket and let the stack reach a stable state then manually remove it from the template.

* To create a decoupled, shareable infrastructure without hardlocking stacks, do not use `Fn::ImportValue`. Instead, publish the variable into SSM Parameter Store using a strict naming pattern and have the application stack read the values dynamically using Dynamic References. This helps avoid locked dependencies.

* If a custom resource in a CFN stack is used to invoke a Lambda function, the request will include a pre-signed URL. The Lambda function is responsible for returning a response to this pre-signed URL to indicate if the resource creation was successful or not. If the Lambda function fails to respond to the pre-signed URL, the CloudFormation stack will remain in the `CREATE_IN_PROGRESS` state.
