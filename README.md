# nyudl-mdi-guidelines


## Overview
This repo contains guidelines, best practices, and patterns as we
learn more about developing with, deploying, running, and monitoring a
Message Driven Infrastructure (MDI).

## Status
This is a work in progress.


## Version
0.1.0

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

## Routing and Binding templates

#### `key` Components
| component      | example values |
|----------------|----------------|
|`app`           | `rstar` |
| `service`      | `xip_validation`, `file_identify`, <br>`file_virus_scan`, `file_chz`, <br>`file_video_xcode`...|
| `message type` | `request`, `result`, `status` |


#### Routing Key Template
##### `<app>.<service>.<message type>`
| service             | role     | routing key                   |
|---------------------|----------|-------------------------------|
| bag validation      | producer | `rstar.xip_validate.request`  |
| video transcoding   | producer | `rstar.video_xcode.request`   |
| file identification | producer | `rstar.file_identify.request` |
|                     |          |                               |
| bag validation      | consumer | `rstar.xip_validate.result`   |
| video transcoding   | consumer | `rstar.video_xcode.result`    |
| file identification | consumer | `rstar.file_identify.result`  |


#### Binding Key Template
##### `<app>.<service>.<message type>`
| service             | role     | binding key                   |
|---------------------|----------|-------------------------------|
| event logger        | consumer | `rstar.*.result`              |
| jobs database       | consumer | `rstar.#`                     |
| bag validation      | consumer | `rstar.xip_validate.request`  |
| video transcoding   | consumer | `rstar.video_xcode.request`   |
| file identification | consumer | `rstar.file_identify.request` |


## Messaging Pattern
0. Producer publishes a request message to a direct exchange  
   using the appropriate consumer routing key
0. Message broker routes message to matching consumer queue(s)
0. Consumer receives message from queue
0. Consumer performs task
0. Consumer publishes result to **topic** exchange
0. Consumer `ACK`s message

## Consumer subscriptions
* subscribe to direct exchange

## Queue attributes
* Consumers
  * subscribe to a direct exchange
  * prefetch = 1
  * must send results to topic exchange
  * must `ACK` message after work complete

----
