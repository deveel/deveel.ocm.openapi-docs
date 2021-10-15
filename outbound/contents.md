---
layout: default
title: Message Contents
parent: Outbound Messaging
nav_order: 2
permalink: /outbound/contents
---

## Outbound Message Contents

Although the foundational idea of the omni-channel messaging is _to be able to reach the users with one single message throught multiple channels_, this ambition is sometimes limited by the capabilities of the channels to transport certain type of contents.

Developers who integrate the _Deveel Omni-Channel API_ should be aware of the kind of content is allowed by the channels they are using in the messaging requests.

Some channels support multiple more than one content type (even within the same message, as a _multi-part_).

| Channel Type | Text    | HTML   | Media   | Multi-Part   |
|--------------|:-------:|:------:|:-------:|:------------:|
| sms          |    √    |    x   |    x    |       x      |
| email        |    √    |    √   |    x    |       √      |
| facebook     |    √    |    x   |    √    |       x      |
| sanbox       |    √    |    √   |    √    |       √      |

This is an example of a request to deliver a textual message through a SMS channel 

```json
{
  "channel": "sms-channel-1",
  "sender": { "number": "NO-2254" },
  "receiver": { "number": "+479426775" },
  "content": { "text": "Hello! you have a special offer waiting" },
  "context": {
    "userId": "47aeg-11hde-dffe4-ga2c5"
  }
}
```

**Note**: e-mail HTML contents must be provided as _base-64_ encoded string