# nyudl-mdi-guidelines


## Overview
This repo contains guidelines, best practices, and patterns as we
learn more about developing with, deploying, running, and monitoring a
Message Driven Infrastructure (MDI).

## Status
Under Development

## Version
0.1.0-alpha

## Approach
* learn
* question
* prototype
* avoid premature optimization
  * *slow and correct is better than fast and wrong*
* iterative development
  * exercise full toolchain
    * Github + Travis + Jenkins + TBD for monitoring

## Message Attributes
* persistent

## Queue Attributes
* durable

## Exchange Types in scope
* topic

## Routing Key and Binding Key Template
#### `<service code>.<message type>[.<result code>]`

#### Template Components:
| Component | Required? | Description |
|-----------|-----------|-------------|
| `service code` | YES | A value from the **Service Code Controlled Vocabulary** (see below) |
| `message type` | YES | A value from the **Message Type Controlled Vocabulary** (see below) |
| `result code` | no | A value from the **Result Code Controlled Vocabulary** (see below) |


###### Template Usage Examples
| Message Routing Key Examples  |
|-------------------------------|
|`deriv_gen.request`            |
|`e01verify.response.success`   |
|`e01verify.response.failure`   |
|`bag_validation.status`        |  


| Queue Binding Key Examples | Use Case              |
|---------------------------|-----------------------|
| `*.response.*`            | event logger          |
| `#`                       | jobs database         |
| `bag_validation.request`  | bag validation worker |
| `deriv_gen.request`       | derivative generation worker |
| `e01verify.request`       | E01 verify worker  |



## Messaging Pattern
0. Message Consumer (usually a Service) subscribes to the topic exchange using   
   the appropriate binding key.  
0. Message Producer (typically a Requestor) publishes a message to the topic  
   exchange using the routing key for the desired service.
0. Message broker routes message to all matching message queue(s)
0. Consumer receives message from queue
0. Consumer may send out `status` messages during processing, e.g., when a   
   service starts work on a request it may sends a `status` of 'busy'
0. Consumer performs task
0. Consumer publishes response message to the topic exchange
0. Consumer `ACK`s request message

## Consumer subscriptions
* subscribe to **topic** exchange with very selective binding key

## Queue attributes
* Consumers
  * must subscribe to the topic exchange using the correct binding key
  * prefetch = 1
  * must send responses to the topic exchange
  * must `ACK` request messages after work complete

----
