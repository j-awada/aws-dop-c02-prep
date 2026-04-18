# Disaster Recovery

## Storage DR

DR is based on different types of storage that in turn have different levels of resiliency.

**1. Instance store volumes**

Those are physical storage attached to EC2 hosts that can run EC2 instances.

This is viewed as temporary and unreliable storage.

**2. EBS: Elastic Block Store**

Allows creating volumes that can presented to EC2 instances. Those are not replicated across AZs.

**3. S3: Simple Storage Service**

This is replicated across multiple AZs in a region. So you can create a snapshot of an EBS volumes and store that in an S3 bucket.

**4. EFS: Elastic File System**

This is replicated across multiple AZs in a single region. It can tolerate the failure of an AZ.

<div align="center">
    <img src="./images/dr_storage.png" alt="dr storage" width="500" />
</div>
