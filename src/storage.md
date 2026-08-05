# Storage

* [Storage types](./storage_types.md)
* [S3 Object Storage](./s3_misc.md)
* [DynamoDB](./dynamodb.md)
* [ElastiCache](./elasticache.md)

## Storage comparison

<div class="wide-table">

| | **EBS** | **Instance Store** | **EFS** | **S3** | **Storage Gateway** |
|---|---|---|---|---|---|
| Type | Block storage | Block storage (ephemeral) | File storage (NFS) | Object storage | Hybrid storage service (bridges on-prem to AWS) |
| Attaches to | 1 EC2 instance (unless Multi-Attach) | 1 EC2 instance (physically attached to host) | Many instances concurrently | N/A — accessed via API/HTTP | On-prem servers/apps via NFS/SMB (File), iSCSI (Volume), or VTL (Tape) |
| Scope | Single AZ | Single instance/host — data lost on stop, terminate, or hardware failure | Regional (multi-AZ via mount targets) | Regional, replicated across ≥3 AZs (Standard) | On-premises, backed by S3/EBS/Glacier in AWS |
| OS support | Linux & Windows | Linux & Windows | Linux only (NFSv4) | Any (HTTP API) | Any (protocol-based: NFS/SMB/iSCSI) |
| Scaling | Manual resize | Fixed size, determined by instance type | Elastic, automatic | Effectively unlimited | Effectively unlimited (backed by S3) |
| Speed | Lowest latency (local block device, especially io2/gp3) | Highest — directly attached NVMe SSD, no network overhead | Higher latency than EBS (network file system overhead) | Higher latency, but high throughput/parallelism for many objects | Depends on local cache + link speed to AWS |
| Cost | Pay for provisioned capacity (GB/month), regardless of usage | Included in EC2 instance price, no separate storage cost | Pay for storage used (GB/month), no pre-provisioning; pricier per GB than EBS | Cheapest per GB, especially with lifecycle tiers (IA, Glacier) | Pay for storage used in AWS + local gateway appliance/cache |

</div>
