---
layout: default
nav_order: 2
title: Outbound Messaging
has_children: true
---

# Outbound Messaging

Sending messages to individuals requires the integration of the messaging APIs provided by the _Omni-Channel Service_: this offers a unified design, independently from the channels, and their typical idiosynchrasies, so that integrators will not have to handle multiple integration strategies when trying to reach individuals through multiple messsging channels.

For every message, applications must specify, at least the following information when sending messages:

* **Sender** - A terminal (eg. _e-mail address_, _alpha-numeric label_, _telephone number_, etc.), previously validated and authorized by Deveel, that is displayed ton the device of the receiver as the sender of the message (see at [outbound terminals](/outbound/terminals))
* **Receiver** - The terminal belonging to an individual that is receiving the message
* **Content** - Some _channel-aware_ content of the message (for example, _it is not possible to send HTML contents throw a SMS channel_)
* **Channel** - The name of one of the pre-configured channels (that can be obtained calling the [clhannel isting endpoint](/api/messaging/v1/#operation/channel_getPage) ) used to transport the message (**note**: this must be previously provisioned by Deveel)

Please refer to the [Single Message Send](/api/messaging/v1/#operation/message_send){:target="_blank"} or [Batch Message Send](/api/messaging/v1/#operation/message_batchSend){:target="_blank"} operations for more details