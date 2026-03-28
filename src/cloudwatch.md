# CloudWatch

Is a public service to ingest, store and manage logging data.

It supports native ingests of logs from AWS services. The CloudWatch agent can be installed to bring in logs from systems or custom applications.

## Log event

A log event consists of 2 parts: the timestamp and a raw message.

## Log stream

Is a sequence of log events that share the same source.

## Log group

Contains several log streams and represents the thing being monitored eg. `var/log/messages`.

Retention, access permissions and encryption are set on the log group level.

A metric filter can also be defined on a log group and it looks for patterns in the log streams eg. a failed login attempt or an application crash. This allows creating metrics to that specific filter eg. the total failed login attempts which can trigger an alarm that can send notifications etc.

# CloudTrail

This product logs API calls or account activities as CloudTrail Events that are either management events or data events. By default, stores 90 day management event history. Data events need to be enabled and they come at an extra cost.

You can create a trail and configure it to be stored in S3 or CLoudWatch logs.

CloudTrail is not used for realtime logging, it has a 15 mins delay.
