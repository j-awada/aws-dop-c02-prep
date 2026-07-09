# Elastic Container Service

Is a fully managed container orchestration service that helps deploy, manage and scale containerised applications. It is configured using a service definition where you define how the task should scale.

A load balancer can be deployed with the service for load distribution.

You can also configure containers in your tasks to send log information to CloudWatch Logs.

## 2 Cluster modes

**1. EC2 mode**

* There is some overhead but allows for flexibility.
* It required manual component management for eg. scheduling, orchestration, cluster management and PlacementEngine.
* EC2 is used to run containers with ASG (Auto Scaling Group).
* You would manage the container host, capacity and availability.

**2. Fargate mode**

* Scheduling, orchestration and cluster management are fully handled by AWS which removes management overhead
* You don't have to manage EC2 instances as container hosts
* The difference with Fargate is how containers are actually hosted. AWS maintains a shared Fargate infrastructure platform, which is offered to all users of Fargate. You still make use of VPC.

AWS Fargate platform versions are used to refer to a specific runtime environment for Fargate task infrastructure. It is a combination of the kernel and container runtime versions. New platform versions are released as the runtime environment evolves, for example if there are kernel or operating system updates, new features, bug fixes or security updates.

To change the platform version that is being used, you can do a **Force new deployment** which allows a Fargate task to use a more current platform version when **LATEST** is used.

## Elastic Container Registry (ECR)

Is a managed container image registry service. Registries can be public or private. A registry can contain many repositories.

ECR is integrated with IAM. It comes with image scanning, basic and enhanced.

ECR offers replication of container images that can be cross-region or cross-account.
