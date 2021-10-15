---
layout: default
title: Extensions
parent: Outbound Messaging
nav_order: 2
permalink: /outbound/extensions
---

The general model of the omni-channel unified semantics requires the specification of some mandatory parts (_sender_, _receiver_, _channel_ and _content_), to comply with the most fundamental model of messaging.

Anyway, some channels have some _idiosynchratic_ additional elements, which extend the basic model of messages.

To accommodate these specificities, the system allows to include in the omni-channel message object _extensions_, that are not used by Deveel OCM itself to control behaviors, but simply transported to the destination channel.

An example of this concept is the _subject_ extension of the _e-mail_ channel: in fact, e-mail messages provide the possibility to include the additional field of _subject_ (although not required, it is good practice to specify it, to avoid any bounce by e-mail providers).

``` json
{
    "channel": "email-1",
    "sender": { "email": { "address": "info@deveel.com", "displayName": "Deveel" } },
    "receiver": { "emailAddress": "test@example.com" },
    "subject": "Test e-mail",
    "content": {
        "text": "Hello! This is the plain-text content of a test e-mail"
    }
}
```

**Note**: These extending elements are part of the main message and considered _additional properties_ of the protocol

Some of the _known_ extensions by channel

## SMS Channel

| Extension               | Data Type               | Description                             |
|-------------------------|-------------------------|-----------------------------------------|
|                         |                         |                                         |

## E-Mail Channel

| Extension               | Data Type               | Description                             |
|-------------------------|-------------------------|-----------------------------------------|
| subject                 | string                  |                                         |

## Push Channel

| Extension               | Data Type               | Description                             |
|-------------------------|-------------------------|-----------------------------------------|
| sound                   | string                  |                                         |
| badgeCount              | integer number          |                                         |


## WhatsApp Channel

| Extension               | Data Type               | Description                             |
|-------------------------|-------------------------|-----------------------------------------|
| namespace               | string                  |                                         |