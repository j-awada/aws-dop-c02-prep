# Auto-Scaling Group

Auto-scaling allows EC2 to scale automatically based on demand placed on the system.

They are used together with elastic load balancers and launch templates to deliver elastic architectures.

ASG makes use of configuration defined in launch templates or launch configurations.

An ASG has minimum, desired and maximum capacity. It runs instances at a desired capacity by provisioning and terminating instances.

## Scaling policy

Scaling policies help update the desired capacity based on metrics or criteria. They can be:

**Manual** where the desired capacity is manually adjusted.

**Scheduled** for time-based adjustment.

**Dynamic**

- Simple: based on metrics, for example when CPU usage exceeds 50%
- Stepped: allows a quicker action eg. spike in usage
- Target tracking: allows defining a desired aggregate eg. CPU at 40%
