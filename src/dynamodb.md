# DynamoDB

Is a no-SQL, wide-column database as a service product. It is a public service that can handle key/value and document data.

It is highly resilient across AZs and optionally globally. It is fast and is SSD-based.

It allows backups an data is encrypted at rest. It supports event-driven integration.

## DynamoDB tables

A table is a grouping of items that share a primary key. The number of items is infinite but the max size of an item is 400KB.

A primary key can be simple (Partition) aka `pk` or composite which is a combination of the `pk` and the `sk` (Sort key).

## Point-in-time Recovery (PITR)

Is disabled by default and needs to be enabled on a table-by-table basis. It allows for a 35 day recovery window.

## Access

DynamoDB can be accessed via the console, CLI or API.

## Capacity

2 capacity modes:

* on-demand: managed by AWS.

* provisioned: by specifying the RCU (read capacity unit) and WCU (write capacity unit) on a per table basis.

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

**Global Secondary Indexes (GSI)**

They can be created at any time with a default limit of 20 per base table.

It allows for creating an alternative view of the table using a different partition key and sort key.

## Streams and Triggers

**Streams**

Is a time ordered list of item changes in a table with a 24-hour rolling window. Changes include inserts, updates and deletes.

**Triggers**

Allow for actions to take place upon a change in the data. Actions can be via a Lambda function.

## DynamoDB Accelerator (DAX)

Is an in-memory cache for DynamoDB that improves performance.

The interaction with the DynamoDB database is handled by the DAX SDK rather than the user application itself.

## Global Tables

Provides a multi-master cross-region replication. Tables are created in multiple regions and added to the same global table becoming a replica table.

## TTL

Used to set a timestamp for automatic delete of items. Useful for items that lose relevance after a period of time.
