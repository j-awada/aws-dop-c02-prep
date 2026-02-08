# Storage

## EBS (Elastic Block Storage)

Is a service that provides block storage. Allocates raw physical disk as a volume. Volumes can be unencrypted or encrypted using KMS.

Storage is provisioned in 1 AZ where it is isolated and resilient. An EC2 instance in 1 AZ can not attach to a volume in another AZ.

A volume can be snapshotted into S3. It can be used to create volumes in different AZ and regions.

A volume is usually attached to 1 EC2 instance but you can use a multi-attach service to attach it to more than 1 instance at the same time.

## EBS volume types

### SSD-based

Where it uses flash memory chips (no moving parts).

1. **GP2 (General Purpose SSD)**

Is good for boot volumes, low-latency interactive applications and dev/test environemnts.

A newly created volume can be as small as 1GB or as large as 16TB. It is created with an IO credit allocation.

* IO is 1 input/output operation
* An IO credit is a 16Kb chunk of data
* 1 IOP is 1 IO (16Kb) in 1 second

An IO credit bucket has a capacity of 5.4 million IO credits and fills at the baseline performance rate of the volume.

2. **GP3**

Is also SSD-based but removes the credit/bucket architecture used in GP2 for something simpler.

Use case is similar to GP2 i.e. virtual desktops, medium sized single instance databases, low-latency apps, dev/test environments and bootable volumes.

Every GP3 volume regardless of size starts with 3000 IOPs and 125 MiB transfer per second. More specs come at a higher cost.

3. **IO2 BlockExpress**

Designed for super high performance situation for consistency and low latency.

Can achieve 256,000 IOPs per volume and 4,000 MB/s throughput.

<div align="center">
    <img src="./images/storage.png" alt="cloud services" width="500" />
</div>

### HDD-based (Hard Disk Drive)

Uses spinning magnetic disks with read/write heads.

Cheaper than SSD volumes.

4. **st1 (throughput optimised)**

Is cheap.

Designed for data that is sequentially accessed.

Range from 125 GB to 16 TB in size.

IO is measured as 1MB block and st1 is capable of max 500 IOPs i.e. 500 MB/s.

5. **sc1 (cold HDD)**

Is cheaper, lowest cost of EBS volume available.

Used to store data without the focus on performance.

Max of 250 IOPs (1MB IO size). Max 250 MB/s.

Range from 125 GB to 16 TB in size.

## Instance Store volumes

They have higher performance than EBS volumes.

They offer the highest storage performace in AWS because they are locally attached.

Provide temporary block storage devices which are raw volumes that can be attached to instances to be used as a basis for a filesystem. They are local and are not presented over the network.

They are ephemeral and should not be used when persistence is requried.

They physically connect to 1 EC2 host. Instances on that host can access those volumes. An instance can use more than 1 volume.

They are only attached at launch time.

Data is lost if the volume moves, is resized or if there is hardware failure.
