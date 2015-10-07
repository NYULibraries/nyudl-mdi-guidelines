# nyudl-mdi-guidelines
#### *Version: 0.1.0-alpha*
## Overview
This repo contains guidelines, best practices, and patterns as we
learn more about developing with, deploying, running, and monitoring a
Message Driven Infrastructure (MDI).  We plan to update this repo as we learn.

## Project Guiding Principles
We want to avoid premature optimization, *slow and correct is better than fast and wrong*.
We will use iterative development and exercise the full toolchain, e.g.,
Github + Travis + Jenkins + TBD for monitoring

<br>
## Status
Under Development

<br>
## Message Attributes
* `persistent` : all messages must be `persistent`

<br>
## Queue attributes
* `durable`: all queues must be `durable`

<br>
## Exchange Attributes
* `durable`: the exchange must be `durable`
* `rstar_topic` : the topic exchange name is `rstar_topic`

<br>
## Exchange Types in scope
* `topic`: we will use one `topic` exchange for all messages

<br>
## Message Routing Key Template
#### `<service code>.<message type>[.<result code>]`

#### Routing Key Template Words:
| Word | Required? | Description |
|-----------|-----------|-------------|
| `service code` | YES | A value from the [**Service Code Controlled Vocabulary**](#service-code-controlled-vocabulary) |
| `message type` | YES | A value from the [**Message Type Controlled Vocabulary**](#message-type-controlled-vocabulary) |
| `result code` | no | A value from the **Result Code Controlled Vocabulary** (see below) |


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

<br>
## Messaging Pattern
0. Message Consumer (usually a Service) subscribes to the topic exchange using   
   the appropriate binding key.  
0. Message Producer (typically a Requestor) publishes a message to the topic  
   exchange using the routing key for the desired service.
0. Message broker routes message to all matching message queue(s)
0. Consumer receives message from queue
0. Consumer may send out `status` messages during processing, e.g., when a   
   service starts work on a request it may sends a `status` of 'busy'
0. Consumer processes request
0. Consumer publishes response message to the topic exchange
0. Consumer `ACK`s request message

<br>
## Message Consumer Responsibilities
  * must subscribe to the topic exchange using the correct binding key
  * must set `prefetch = 1`
  * must send `response` messages to the topic exchange
  * must `ACK` `request` messages after `request` processing is complete

<br>
## Message Formats
  * Please refer to the [NYUDL MDI Message](https://github.com/NYULibraries/nyudl-mdi-message) repository for details

<br>
## Routing Key Controlled Vocabularies
### Service Code Controlled Vocabulary
| Service Code | Description | Granularity |
|--------------|-------------|-------------|
| e01verify  ***Don, please change if desired***  | ***Don, please fill in*** | ***Don, please fill in*** |
| deriv_gen  ***Rasan, please change if desired***  | ***Rasan, please fill in*** | ***Rasan, please fill in*** |
| bag_validation | Check that directory conforms to BagIt specification | bag directory |
<br>

### Message Type Controlled Vocabulary
| Message Type | Description |
|--------------|-------------|
| `request`    | sent to a service |
| `response`   | sent in response to `request` |
| `status`     | used to broadcast the status of a request |
<br>
### Result Code Controlled Vocabulary
| Result Code  | Description |
|--------------|-------------|
| `success`    | `request` was processed successfully |
| `failure`    | `request` failed |  
<br>

----
#### ***end of document***
----
