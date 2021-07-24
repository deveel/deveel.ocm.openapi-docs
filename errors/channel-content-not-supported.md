---
title: Error - Channel Content Not Supported
parent: Errors
layout: default
permalink: /errors/channel-content-not-supported
---

# Channel Content Not Supported

The user or client application is trying a message with content that is not supported by the _[Channel](/channels)_ indicated as the transportation of the message itself.

The content can be not allowed for at least two reasons:

1. The type of channel doesn't support that kind of content (eg. _sms messages cannot contain attachments_)
2. The instance of the channel used was configured to exclude the capability to transport that kind of content type, becsause of commercial agreements