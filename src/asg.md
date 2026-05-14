# Auto-Scaling Group

Auto-scaling allows EC2 to scale automatically based on demand placed on the system. Together with Elastic Load Balancers, Auto-scaling Groups (ASG) are used to deliver an elastic architecture.

An ASG which makes use of configuration defined in Launch Templates or Launch Configurations, has minimum and maximum desired capacity. It runs instances at a desired capacity by provisioning and terminating these instances.

## Scaling policy

Scaling policies help update the desired capacity based on metrics or criteria. They can be:

- **Manual**: where the desired capacity is manually adjusted

- **Scheduled**: for time-based adjustment

- **Dynamic**

    - Simple: based on metrics, for example when CPU usage exceeds 50%
    - Stepped: allows granular control for eg. if CPU usage is between 50 to 60% then add 1 instance, between 60 to 70% add 2 instances, etc. and the same in reverse.
    - Target tracking: it comes with a pre-defined set of metrics eg. CPU utilisation, average network in/out and ALB request count per target.

## ASG Lifecycle Hooks

Hooks enable configuring custom actions on instances to get executed for eg. during instance launch or termination.

This can be integrated with EventBridge or SNS notifications to allow event-driven processing based on launch or temination of EC2 instances within an ASG.

## ASG Health Checks

EC2 instance health is evaluated in 3 ways:

- EC2: which is the default and uses the following unhealthy statuses: `Stopping`, `Stopped`, `Terminated`, `Shutting Down` or `Impaired`.

- ELB: is the load balancer health check where the instance needs to be both healthy and pass the load balancer health check.

- Custom: where an external system can be integrated to mark instances as healthy or not.
