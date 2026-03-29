# Kinesis

Is a scalable realtime streaming service designed to ingest data from lots of devices or applications.

Producers send data to Kinesis eg. EC2, IoT, applications, etc.

The Kinesis stream ingests the data and the Consumers consume this data.

The stream starts with 1 shard and increases the number of shards as it scales. A shard provides 1 MB/sec ingestion and 2 MB/sec consumption capacity.

## Amazon Kinesis Data Firehose

Is a fully-managed service that delivers data to supported services like S3 allowing data to be persisted beyond the rolling window of Kinesis data streams.

It is a near-realtime service where data of size 1MB is buffered for 60 secs before delivery.

Data Firehose can deliver data to HTTP endpoints, Splunk, Redshift, ElasticSearch and S3.

Firehose can transform data using Lambda before delivering it.

## Amazon Kinesis Data Analytics

Is a service that provides realtime processing of data which flows through it using SQL.

It ingests from Kinesis Data Streams or Firehose.

The destination for the data can be Streams, Firehose or any of the services that Firehose supports.
