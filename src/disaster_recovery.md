## Observability questions to ask

* What is the average response time of the website over the last hour?
* Are there any error rate spikes?
* What specific endpoints are taking long to respond?

These questions are answered through data provided by the application's logs, metrics, traces. 

Relevant metrics include:

- CPU usage
- memory usage
- response times
- database query execution time
- 4xx and 5xx errors

## Observability Vs. Monitoring

* Monitoring tells you that a system has failed while observability helps you find out why that system failed.

* Monitoring is tooling that allows teams to watch and understand the state of their system via metrics and logs.

* Observability is not about knowing that something went wrong but about understanding why it went wrong. For example, a website slows down unexpectedly -> analyse and review logs to identify a recent code deployment that caused a memory leak.

## Resiliency

Is the ability of a workload to recover from infra, service or app disruption. Disaster recovery (DR) is part of the resiliency strategy and it concerns how the workload responds when a disaster strikes. This is based on RPO (recovery point objective) and RTO (recovery time objective).

### RPO

* How much data a business can lose expressed in time
* Is determined using the time at which a successful backup of data occurs
* More frequent backups -> lower RPO eg. a backup every 6 hours
* What is considered an acceptable loss of data is defined by the organisation

### RTO

* The max tolerable length of time that a system can be down for after a failure occurs
* Can be reduced by effective monitoring, notification and processes

## Backup and recovery strategies on cloud

By deploying across multiple AZs in a single AWS region, your workload is better protected against failure of a single (or even multiple) data centres. For extra assurance, you can back up data and configuration to another region.

In addition to data, you must redeploy infrastructure, configuration and application code. To enable infrastructure to be redeployed quickly, you should always redeploy using IaC.

In addition to user data, you should back up code and configuration including any machine images (AMI) used to create EC2 instances. The AMI can be created from snapshots of an EC2 instance.

For S3, you can use CRR (Amazon S3 Cross-Region Replication) to asynchronously copy objects to an S3 bucket in the DR region continuously, while providing versioning for the stored objects so that you can choose your restoration point.

Another strategy is to have active and passive regions. An active site in a region is used to host the workload and serve traffic. A passive site in another region is used for recovery. The passive site does not actively serve traffic until a failover event is triggered.

You can use a warm standby approach where you have a fully scaled down but functional copy of production in another region. This approach decreases the time to recovery.
