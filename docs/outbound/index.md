---
layout: default
nav_sort: 1
title: Outbound Messaging
has_children: true
---

# Outbound Messaging

Sending messages to individuals requires the integration of the messaging APIs provided by the Omni-Channel Service: this offers you a unified design, independently from the channels, and their typical idiosynchrasies, so that integrators will not have to handle multiple integration strategies when trying to reach individuals through multiple messsging channels.

For every message, applications must specify, at least the following information when sending messages:

* **Sender** - A terminal (eg. _e-mail address_, _alpha-numeric label_, _telephone number_, etc.), previously validated and authorized by Deveel, that is displayed ton the device of the receiver as the sender of the message 
* **Receiver** - The terminal belonging to an individual that is receiving the message
* **Content** - A _channel-aware_ content of the message (_for example_, it is not possible to send HTML contents throw a SMS channel)
* **Channel** - The name of one of the pre-configured channels (that can be obtained calling the [clhannel isting endpoint](#channel_getPage) ) used to transport the message (**note**: this must be previously provisioned by Deveel)

Please refer to the [Single Message Send](/api/messaging/v1/#operation/message_send){:target="_blank"} or [Batch Message Send](/api/messaging/v1/#operation/message_batchSend){:target="_blank"} operations for more details