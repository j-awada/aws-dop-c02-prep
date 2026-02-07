## Cloudormation (CFN)

* Cloudformation allows automating infrastructure provisioning
* Starts with a template written in YAML or JSON
* The template defines logical resources
* A template can create 1 or more stacks
* A template can be reusable
* The stack definition will create physical resources based on logical resources
* If the stack definition is updated, the physical resources also change

* Templates can take parameters i.e. accept input
* Parameters are defined in the template and can have a types eg. an EC2 image
* Pseudo parameters are defined by AWS. They are injected by AWS into the template eg. Region, stack ID, stack name, account ID, etc.

* Cloudformation does things in parallel so use `DependsOn` to define an order if needed
* You can configure Cloudformation to hold until a number of signals are successful before returning `CreateComplete`
* You can configure a timeout for Cloudformation execution

### Intrinsic functions

* Allow you to gain access to data at runtime i.e. when the template is being used to create a stack eg.
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
    - eg. a Lambda function used to populate or remove items from a bucket. This is because CFN can't delete buckets that have objects in them. The Lambda function receives a CREATE or DELETE event and then uploads or deletes bucket items. The custom resource references both the Lambda function and then bucket

### CFN drift detection

* When the logical resources and the physical resources in a stack no longer match
* CFN offers drift detection
* What can be done is:
    - removing the logical resources while keeping the physical resources in place and then importing those physical resources to bring back into sync the relationship between logical and physical resources
