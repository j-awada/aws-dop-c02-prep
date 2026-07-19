# DynamoDB

Is a no-SQL, wide-column database as a service product. It is a public service that can handle key/value and document data. It is highly resilient across AZs and optionally globally. It is fast and is SSD-based.

It allows backups and data is encrypted at rest. It supports event-driven integration.

## DynamoDB tables

A table is a grouping of items that share a primary key. The number of items is infinite but the max size of an item is 400KB.

A primary key can be simple (Partition) aka `pk` or composite which is a combination of the `pk` and the `sk` (Sort key).

## DynamoDB global tables

This is a fully managed solution for deploying a multi-region, multi-active database without having to build and maintain your own replication solution. You can specify the AWS regions where you want the tables to be available and DynamoDB will propagate ongoing data changes to all of them.

## Point-in-time Recovery (PITR)

Provides continuous backups with per-second recovery precision. It is disabled by default and needs to be enabled on a table-by-table basis. It allows for a 35-day recovery window.

Enabling PITR does not consume any table provisioned throughput (RCUs/WCUs) and has zero impact on application performance.

## Access

DynamoDB can be accessed via the console, CLI or API.

## Capacity

2 capacity modes:

1. On-demand: managed by AWS.

2. Provisioned: by specifying the RCU (read capacity unit) and WCU (write capacity unit) on a per table basis.

Every operation consumes at least 1 RCU/WCU with a max of 4KB per RCU and 1KB for WCU.

Every table has a RCU and WCU burst pool of 300 seconds of the read/write capacity unit on the table.

## Operations

**Query**

Is the most efficient operation and is used with a partition key.

**Scan**

Is not too efficient since it moves through the table item by item.

Even if an item is not returned, capacity is still consumed.

## Indexes

They help improve the efficiency of data retrieval and are of 2 types of indexes:

**Local Secondary Indexes (LSI)**

They must be created when the base table is created and they allow for an alternative sort key `sk` on the table.

* Partition key: same as the table
* Sort key: different from the table
* Supports strongly consistent reads (i.e. the latest version of the data after a write) and is used for sorting/querying items within the same partition key

**Global Secondary Indexes (GSI)**

They can be created at any time with a default limit of 20 per base table.

It allows for creating an alternative view of the table using a different partition key and sort key.

* Partition key: can be different from the table.
* Sort key: can also be different
* Does not support strongly consistent reads (i.e. the latest version of the data after a write) and is used to query the data using a completely different key

## Streams and Triggers

**Streams**

Amazon DynamoDB Streams is a feature of AWS DynamoDB that captures a time-ordered sequence of changes (inserts, updates and deletes) to a DynamoDB table. This enables applications to react to data changes in real time. Streams have a 24-hour rolling window.

Stream records are organised into groups or "shards".

**Triggers**

Allow for actions to take place upon a change in the data. Actions can be via a Lambda function.

**Example case**

A user updates a row in DynamoDB -> DynamoDB instantly writes this change into a Stream Shard -> this gets polled internally by AWS -> the Lambda function set as trigger gets invoked.

The Lambda function should have a proper IAM Execution role `AWSLambdaDynamoDBExecutionRole`.

In case the Lambda function needs to write to a private database inside a VPC, the Lambda function must be configured with VPC membership.

## DynamoDB Accelerator (DAX)

Is an in-memory cache for DynamoDB that improves performance.

The interaction with the DynamoDB database is handled by the DAX SDK rather than the user application itself.

## Global Tables

Provides a multi-master cross-region replication. Tables are created in multiple regions and added to the same global table becoming a replica table.

## TTL

Used to set a timestamp for automatic delete of items. Useful for items that lose relevance after a period of time.

## Exam notes

* In DynamoDB, writes to a main table are replicated asynchronously to its GSIs. If a GSI cannot keep up with the write volume because its WCUs are capped, DynamoDB will purposefully throttle the writes on the main table to prevent the index from falling too far behind. Increasing the GSI's auto-scaling upper limit removes this backpressure.

    This manifests as a `ProvisionedThroughputExceededException` error on the user side.

* Native DynamoDB backup is regional. For centralised, use AWS Backup for cross-account automated governance.