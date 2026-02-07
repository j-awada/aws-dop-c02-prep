# SNS and SQS

## Simple Notification Service (SNS)

* Is a public AWS service i.e. it can be accessed from public internet but also VPC as well
* It coordinates the sending and delivery of messages
* The message payload is <= 256 Kb
* SNS is used across AWS eg. CloudWatch, CloudFormation
* SNS is highly available (HA) and scalable i.e. per region
* It is capable of server-side encryption (SSE)


**How it works:**

* A **publisher** sends messages to a **topic**
* Topics have **subscribers** which receive messages
* Subscribers can be HTTP(s) endpoints, email addresses, SQS, SMS messages, Lambda funcitons
* A filter can be applied on a subscriber so that it receives messages relevant to it
* SNS supports delivery status and delivery retries
* Topic policy can be applied for cross-account use

## Simple Queue Service (SQS)

* Is a public service, fully managed that provides highly available message queues
* Can access private services
* Messages in a queue can be up to 256 Kb in size
* A client can send messages to a queue and another client aka consumer can poll the queue and check for any messages on the queue.

**2 types of SQS queues**

**Standard:**

* message delivered at least once and is scalable
* standard queues can scale to unlimited TPS (transaction per second)

**FIFO:**

* message delivered exactly once, ordered delivery, lower level of scaling
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
