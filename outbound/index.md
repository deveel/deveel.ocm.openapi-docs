---
layout: default
nav_order: 2
title: Outbound Messaging
has_children: true
---

# Outbound Messaging

Sending messages to individuals requires the integration of the [Messaging API](/api/messaging/v1) provided by the _Omni-Channel Service_: this offers a unified design, that abstracts the messaging channels and their typical idiosynchrasies, so that users will not have to handle multiple integration strategies, one for each provider of the messaging functions.

To be successfully handled, Message objects require a set of mandatory information

| Field               | Description                                     |
|---------------------|-------------------------------------------------|
| **sender**          | A terminal (eg. _e-mail address_, _alpha-numeric label_, _telephone number_, etc.), previously validated and authorized by Deveel, that is displayed ton the device of the receiver as the sender of the message (see at [outbound terminals](/outbound/terminals)) |
| **receiver**        | The terminal belonging to an individual that is receiving the message
| **content**         | The contents of the message that is typically channel-aware (see the [content types](/outbound/contents)) |
| **channel**         | The reference to one of the pre-configured [channels](/channels) used to transport the message (**note**: this must be previously provisioned by Deveel) |

Please refer to the [Single Message Send](/api/messaging/v1/#operation/message_send){:target="_blank"} or [Batch Message Send](/api/messaging/v1/#operation/message_batchSend){:target="_blank"} operations for more details