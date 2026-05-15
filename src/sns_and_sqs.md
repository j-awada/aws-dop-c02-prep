# SNS and SQS

## Simple Notification Service (SNS)

Is a public AWS service i.e. it can be accessed from public internet but also a VPC as well. It coordinates the sending and delivery of messages. The message payload is <= 256 Kb.

SNS is a push-based notification service for real-time messaging, supporting both application-to-application and application-to-person communication. It involves 3 actors: the publisher, subscriber and the topic.

SNS is used across AWS eg. CloudWatch, CloudFormation. It is highly available (HA) and scalable i.e. per region. It is capable of server-side encryption (SSE).


**How it works:**

* A **publisher** sends messages to a **topic**
* Topics have **subscribers** that receive messages
* Subscribers can be HTTP(S) endpoints, email addresses, SQS, SMS messages, Lambda funcitons
* A filter can be applied on a subscriber so that it receives messages relevant to it
* SNS supports delivery status and delivery retries
* Topic policy can be applied for cross-account use

## Simple Queue Service (SQS)

This is a public service, fully managed that provides highly available message queues. It can be configured to access private services. Messages in a queue can be up to 256 Kb in size.

SQS is a poll-based message queue service for decoupling services, supporting application-to-application communication and message retention.

SQS uses queues to decouple microservices, enabling you to create asynchronous workflows in your applications. It involves 3 actors: the producer, consumer and the queue. A client can send messages to a queue and another client aka consumer can poll the queue and check for any messages on the queue.

### 2 types of SQS queues

**1. Standard:**

* Message gets delivered at least once and is scalable
* Standard queues can scale to unlimited TPS (transaction per second)

**2. FIFO:**

* Message gets delivered exactly once, ordered delivery, lower level of scaling
* FIFO queues can handle 300 TPS or messages per second without batching and 3000 with batching


### SQS and SNS Fanout architecture

* A message is added to a SNS topic
* Each topic has a SQS queue i.e. a queue is a subscriber to a topic

### SQS extended client library

Allows sending a message with size more than 256 Kb.

### SQS delay queues

* Allows you to postpone the delivery of messages to consumers
* Is something other than Visibility Timeout (which allows automatic reprocessing of problematic messages)
* Messages are delayed to become visible on a queue

### Dead-letter queues

* Help handle reccurring failures while processing messages within an SQS
* When `ReceiveCount` > `maxReceiveCount` and message isn't deleted => the message is moved to the dead-letter queue
