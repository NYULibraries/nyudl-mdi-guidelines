# nyudl-mdi-guidelines


## Overview
This repo contains guidelines, best practices, and patterns as we
learn more about developing with, deploying, running, and monitoring a
Message Driven Infrastructure (MDI).

## Status
This is a work in progress.


## Version
0.0.1-alpha

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
* direct
* topic

## Binding template
* `<app>.<service>.<message type>`
  * `bag validation      : rstar.xip_validate.request`
  * `video transcoding   : rstar.video_xcode.request`
  * `file identification : rstar.file_identify.request`
  * `event logger        : rstar.*.result`
  * `jobs database       : rstar.#`

## Routing Key template
* `<app>.<service>.<message type>`
  * `bag validation:      rstar.xip_validate.request`
  * `video transcoding:   rstar.video_xcode.request`
  * `file identification: rstar.file_identify.result`

  * examples:

|field           | example values |
|----------------|----------------|
|`app`           | `rstar` |
| `service`      | `xip_validation`, `file_identify`, <br>`file_virus_scan`, `file_chz`, <br>`file_video_xcode`...|
| `message type` | `request`, `result`, `status` |
| `reference id` | `c9aebc65-e90b-43fc-b776-e52399e77223` |



## Messaging Pattern
* Producer sends a processing request message to a direct exchange
* Worker receives message from queue
* Worker performs task
* Worker publishes result to topic exchange
* Worker `ACK`s message

## Worker subscriptions
* subscribe to direct exchange

## Queue attributes
* Workers
  * subscribe to a direct exchange
  * prefetch = 1
  * must send results to topic exchange
  * must `ACK` message after work complete

----
