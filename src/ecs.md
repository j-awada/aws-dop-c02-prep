# Elastic Container Service

Is a fully managed container orchestration service that helps deploy, manage and scale containerised applications.

* You can configure this using a service definition
* Defines how the task should scale
* A load balancer can be deployed with the service for load distribution
* When using a single simple task, a service is not needed

## Cluster mode

Can be EC2 or Fargate.

**1. EC2 mode**

* There is some overhead but allows flexibility.
* ECS management components eg. scheduling, orchestration, cluster management and PlacementEngine.
* EC2 is used to run containers with ASG (Auto Scaling Group).
* You can manage the container host, capacity ad availability.

**2. Fargate mode**

* Also includes ECS management components such as scheduling, orchestration, cluster management and PlacementEngine.
* Removes management overhead
* You don't have to manage EC2 instances as container hosts
* The difference with Fargate is how containers are actually hosted. AWS maintains a shared Fargate infrastructure platform, which is offered to all users of Fargate. You still make use of VPC.
