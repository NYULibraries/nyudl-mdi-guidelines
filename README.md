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
* avoid pre-mature optimization
  * slow and correct is better than fast and wrong
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

## Routing Key template
* `<app>.<service>.<message type>.<producer reference id>`
  * `bag validation:      rstar.xip_validate.request.b78bc9bf-77a1-4d16-95ef-23d1d75b060f`
  * `video transcoding:   rstar.video_xcode.req.3b7d3a3f-f4ea-408c-aedc-ba2e06471da4`
  * `file identification: rstar.file_identify.res.c9aebc65-e90b-43fc-b776-e52399e77223`

## Messaging Pattern
* Producers send a processing request message to a direct exchange
* Workers receive message from queue
* Workers perform task
* Workers `ACK` message
* Workers send results to topic exchange

## Worker subscriptions
* subscribe to direct exchange

## Queue attributes
* Workers
  * subscribe to a direct exchange
  * prefetch = 1
  * must `ACK` message after work complete
  * must send results to topic exchange

----
