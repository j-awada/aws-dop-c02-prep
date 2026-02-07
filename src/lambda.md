# Lambda

* FaaS: Function-as-a-Service
* Part of serverless or event-driven architecture
* It is stateless i.e. every time the function is invoked, it uses a brand new environment
* Lambda functions can run for up to 900 seconds ~ 15 minutes
* Lambda functions assume IAM roles to interact with other AWS resources
* Lambda is provided an execution role

### Lambda networking modes

**1. Public**

* In the public AWS network
* Can access public-space AWS services like SQS and DynamoDB as well as the public internet
* But Lambda here has no access to services running in a VPC unless they have a public IP and access permissions

**2. Private / VPC**

* Lambda runs in a private subnet in a VPC and is subject to the networking rules of that VPC
* In this case rules need to allow Lambda access to public internet
* The Lambda here still needs permissions to interact with services in the same subnet


### Invoking a Lambda function

A Lambda function can be invoked in 3 different ways:

**Synchronous**

Where the client waits for a response from the Lambda function.

**Asynchronous**

For when an AWS service invokes a Lambda function. The service does not wait for a response. Upon failure, the Lambda function will do a retry.

**Event source mapping**

This is used on streams or queues which don't generate events.

### Lambda function versions

Lambda functions can have different versions eg. 1, 2, 3, etc. A version is the function code plus the configuration.

Once published, a version is immutable and has its own ARN. The `$latest` tag points to the latest version of a Lambda function. Aliases can be created to point at a particular version of a Lambda function eg. a prod alias.

### Lambda startup time

* A Lambda function can have a warm or cold startup
* AWS creates and keeps execution contexts warm and ready to use
* To speed up running a Lambda function, the execution context can be reused. This is done using a feature called Provisioned Concurrency where an execution context is provisioned in advance.
* Use `/tmp` to pre-download artifacts eg. images that will be available to other functions using the execution context.
* You should however by default assume that execution contexts are stateless.

### Lambda function handler

**Lambda execution lifecycle**

* init: creates or unfreezes the execution environment
* invoke: runs the function handler
* next invoke(s): warm start eg. same environmnet
* shutdown: terminate the environment

**init includes:**

* extension init
* runtime init
* function init

**shutdown has:**

* runtime shutdown
* extension shutdown

**Lambda function has 2 parts:**

* function init: this is executed during cold starts. It contains anything that might be reused eg. database connection.
* invoke: the handler component which performs the logic of the function.

### Logging and tracing

Lambda sends execution logs to Cloudwatch logs `/aws/lambda/<function name>` log group but the STDOUT/STDERR can also be viewed in the function itself.

To log, the Lambda function needs permissions provided via the execution role.

XRay shows requests to the function and Active Tracing can be enabled on the function.

### Lambda layers

This groups similar libraries into layers that can be shared between Lambda functions instead of including those libraries in every function. You can create layers or use pre-existing ones provided by AWS.

### Lambda container images

This is used when CI/CD processes produce container images. To use the container image with Lambda, you need to include the Lambda runtime API within the image.

### Lambda funtion permissions

* execution role: assumed by the function. It determines what the function can do.
* resource policy: determines who can do what with the Lambda function.

The resource policy is used for cross-account invocation or for service access i.e. when a service wants to invoke Lambda eg. S3.
