# Elastic Load Balancing

It distributes traffic across different resources such as EC2 instances, containers and IP addresses in 1 or more availability zones.

LBs can be set to private or public.

## Types

**Application LB**

* Runs at the OSI application layer, L7
* Targets IP addresses, EC2, EKS, Fargate, Lambda

**Network LB**

* At the OSI transport layer, L4
* Handles more traffic than the ALB

**Gateway LB**

* Runs at the OSI network layer, L3
* Can act as a single entry/exit for all traffic
