# Architecture evolution example 1

### Stage 1

- A single EC2 instance which hosts a website is running and is directly reachable from public internet. It is running in a public subnet in a VPC. It uses a security group to open the TCP port for HTTP connection.

- Make use of the Systems Manager to create parameters in the Parameter Store for eg. database username, database name, database hostname, database user password, etc.

- Bottlenecks at this point:

    * the application instance was created manually -> no automation
    * the database and application are hosted on the same instance -> risk data getting lost
    * the application media is also stored on the instance -> risk data getting lost
    * customer connections are directly to the instance -> no scaling option, no healthchecks or automatic healing
    * the instance public IP is not static i.e. it will change when the instance restarts -> IP might be hardcoded in some place eg. database hostname

### Stage 2

- Create a launch template for the instance to automate the build. The template contains configuration such as the AMI, instance type, SG, IAM instance role to interact with other AWS services, User Data, etc.

### Stage 3

- Migrate the database from the EC2 instance to a separate RDS instance.

- Use 3 AZs with a subnet in each

- Create a MySQL RDS instance with specific settings eg. username, password, etc.

- The address of the RDS used by the app needs to be updated. This can be done by adjusting config in the EC2 launch template. Make sure to update the version of the launch template to v2.

### Stage 4

- Migrate the media content to EFS (Elastic File System)

- Update the parameter store with the new EFS

- The `efs` support package needs to be installed on the instance to allow it to interact with the EFS

- The media data in the instance needs to be migrated to the mounted EFS. This can be done using manual commands from the instance console.

- At this point, the EC2 instance can be scaled without loss of data from the database or media files

- The launch template needs to be updated incl. the User Data section

### Stage 5

- Add an auto-scaling group and load balancer to provision instances automatically based on the load of the system

- Create an ALB and create/define a target group

- The load balancer DNS name needs to be exposed to the EC2 instances from the parameter store, and the launch template updated

- Create an auto-scaling group and configure it to use the launch template. Attach the ASG to a load balancer.

- An alarm from CloudWatch can be set for when CPU usage exceeds a limit.
