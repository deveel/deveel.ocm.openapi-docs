---
layout: default
title: Message Terminals
parent: Outbound Messaging
nav_sort: 1
permalink: /outbound/terminals
---

# Message Terminals

The capability of establishing a messaging conversation between two or more parties is enabled by the _terminal points_ of communication, that are the source and destination of messages.

It is possible to edetermine the kind of terminal that is possible to use when sending _outbound messages_ throught a type of channel, as described in this matrix

| Channel Type | Phone Number | E-Mail Address | Alpha-Numeric Label |
|--------------|:------------:|:--------------:|:-------------------:|
| sms          |   √          |    x           |   √                 |
| email        |   x          |    √           |   x                 |
| whatsapp     |   √          |    x           |   x                 |
| facebook     |   √          |    x           |   x                 |
| sanbox       |   √          |    √           |   √                 |

## Sender Terminals

Users and applications can send messages outbound towards receivers using instances of [Channel](/channels) assigned: the messages require the specification of a _sender terminal_ (or simply _sender_) that represents one of the two points of communication

The platform requires these terminals to be _pre-registered_, _authorized_ and _active_ to allow messages to be processed, and any request of outbound messaging that includes invalid senders will fail.

Deveel owns an inventory of available Terminals that users can request to be assigned, compatibly with the channels already owned: in fact, as described in the matrix above, as an example, _it would not be possible to use an e-mail address to send a SMS message_.

## Porting Terminals

When a user _ports_ its own channels use them within the platform, it is possible to associate to those one or more _senders_: it is typical that specific terminals (such as phone numbers, e-mail addresses, etc.) to be controlled by providers of messaging services.

In such cases the source of the terminal would be the customer, who would be responsible of the validation and activation of such terminal on the provider of messaging services of the channel: the typical effect (_depending on the policies applied by the specific messaging providers_) of an invalid configuration of a ported terminal would be the rejection by the provider of messages sent using such invalid terminal as originator.