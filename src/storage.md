# Storage

* [Elastic Block Storage](./elastic_block_storage.md)
* [Elastic File System](./efs.md)
* [S3 Object Storage](./s3_misc.md)
* [DynamoDB](./dynamodb.md)
* [ElastiCache](./elasticache.md)

## EBS vs EFS vs S3

| | **EBS** | **EFS** | **S3** |
|---|---|---|---|
| Type | Block storage | File storage (NFS) | Object storage |
| Attaches to | 1 EC2 instance (unless Multi-Attach) | Many instances concurrently | N/A — accessed via API/HTTP |
| Scope | Single AZ | Regional (multi-AZ via mount targets) | Regional, replicated across ≥3 AZs (Standard) |
| OS support | Linux & Windows | Linux only (NFSv4) | Any (HTTP API) |
| Scaling | Manual resize | Elastic, automatic | Effectively unlimited |
| Speed | Lowest latency (local block device, especially io2/gp3) | Higher latency than EBS (network file system overhead) | Higher latency, but high throughput/parallelism for many objects |
| Cost | Pay for provisioned capacity (GB/month), regardless of usage | Pay for storage used (GB/month), no pre-provisioning; pricier per GB than EBS | Cheapest per GB, especially with lifecycle tiers (IA, Glacier) |
