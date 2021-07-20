---
layout: default
title: Inbound Messages
parent: Webhooks
nav_sort: 2
---

# Inbound Messages

The system allows to subscribe to the routing of messages sent from individuals to a **Terminal** (eg. a _telephone number_ or an _email address_) that is registered by a tenant (see the paragraph below).

These messages are transported to the consumer through a _webhook_ of a specific kind, named **Message Receiver**: this can be configured by the users of the customers ahead of time (see the paragraph below).

The payload of such events are typically specifying the following properties:

| Property        | Description                                                             |
|-----------------|-------------------------------------------------------------------------|
| id              | The globally unique identifier of the event                             |
| messageId       | A globally unique identifier of the message                             |
| timeStamp       | The exact time-stamp of the event                                       |
| channel         | Provides the name and type of channel that transported the message      |
| sender          | An object describing the sender (eg. the person's address)              |
| receiver        | An object describing the receiver (eg. the customer's terminal address) |
| content         | Encapsulates an eventually complex content of the message               |

The fields _channel_, _sender_, _receiver_ and _content_ are complex and respect the same _unified design_ used for the outbound messages.