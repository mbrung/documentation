---
title: Routing
category: internals
tags: internals, reverse-proxy, routing, http, timeout, websocket
date: 22/03/2015
---

# HTTP Routing in Scalingo

## Requests scheduling

Each of our front web servers is applying a strict round-robin scheduling over the different
containers of your application.

## Headers

* `X-Forwarded-For`: IP from the end user
* `X-Real-Ip`: IP from the end user
* `X-Forwarded-Proto`: Either `http` or `https`
* `X-Forwarded-Port`: Either 80 or 443

### Example

If you want to have a look to those headers: here is an application which dumps
them: https://http-env.scalingo.io/

## Long running connections - SSE - Websockets

Long running connections and websockets are completely supported, but they are under
the same constraint timeout as any other kind of request.

## Sticky Sessions

Sticky session is currently only enabled for Meteor applications, it will be
available as an option for any application in the future.

## Timeouts

Your application has to accept the connection within the 30 seconds and has 30
seconds to send the first data to the client in the 30 seconds. If one of these
conditions is not respected, our servers will return a 504 Gateway Timeout
error to the end user.

* Connection timeout: 30s
* Read timeout: 30s

These rules also apply to the long running connections like websockets. Ensure
your application is sending at least one ping every 29 seconds to keep the connection
open, otherwise it will be stopped. 