---
layout: default
nav_order: 3
title: Message States
parent: Outbound Messaging
---

## Message State Changes

The system produces a set of HTTP callbacks (_webhooks_), to inform subscribing applications of the changes of state of a message that was previously sent through the APIs.

The payload of the callback (an HTTP request) is typically composed of the following elements:

| Property    | Description                                                         |
|-------------|---------------------------------------------------------------------|
| id          | A globally unique identifier of the event                           |
| messageId   | The unique identifier provided as response from the messaging APIs  |
| statusCode  | Indiates the status of the message (see the list below)             |
| channelName | The name of the channel instance used to transport the message      |
| context     | A dictionary of contextual data set to the original message         |
| timeStamp   | The exact time-stamp of the event (as on the server side)           |

Additionally to the above ones, other event-specific fields are present, at the same level: see the list below to learn about them.

Error events (_delivery retried_ and _delivery failed_) include some additional fields for informing the user of the reason of the delivery failure state

| Property     | Description                                                        |
|--------------|--------------------------------------------------------------------|
| errorCode    | A well-known code that hints the user on the kind of error         |
| errorMessage | A message that is describing in more details the error             |

The _statusCode_ field in the payload of the event can have one of the following values:

| **Code**             | Description                                                        |
|----------------------|--------------------------------------------------------------------|
| _QUEUED_             | The system queued the message                                      |
| _SENT_               | The message was actually sent to the provider                      |
| _DELIVERED_          | The message was successfully delivered to the receiver             |
| _DELIVERY_RETRIED_   | An attept to deliver the message failed for any reason             |
| _DELIVERY_FAILED_    | It was not possible to deliver the message after a number of tries |
| _READ_               | The receiver read the message (not supported by all channels)      |
| _DELETED_            | The receiver deleted the message (not supported by all channels)   |

**Note**: Request to send a message or a batch of messages through the APIs provided by this service might not immediately return any error, since the only handling at that level is the check of the format of the request: listening to the status updates would inform on the real status of the message delivery and if any error occurred during its transportation to the receiver.

### Message Queued

This callback notifies the listening application that a message was queued and it will be processed soon.

```json
{
    "id": "9d0918f6d23c4d40bfe2ee579cb9d81d",
    "statusCode": "QUEUED",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "context": {
        "prop1": "value1",
        "prop2": 33,
        ...
    },
    "timeStamp": "2021-07-13T20:17:37+0100"
}
```

### Message Sent

This callback notifies that the given message was sent through the channel and we are waiting to know if it was _delivered_ or if its _delivery failed_.

```json
{
    "id": "6b3900f233484cfc8c82e4f629e894bb",
    "statusCode": "SENT",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "context": {
        "prop1": "value",
        ...
    },
    "timeStamp": "2021-07-13T20:17:38+0100"
}
```

### Message Delivery Retried

The delivery of a message is retried a certain number of configured times per channel, and each time a failure is notified to the system, a new attempt is performed, until the maximum number is reached, before moving into a permanent failure state (that triggers a _fallback_ to another message, if configured)

If subscribed, this event is notified to the user (as a webhook) each time the an attempt to delivery a message failed, but the maximum number of retries was not reached.

```json
{
    "id": "c65671e1d9fd4a448e3805cb200868ff",
    "statusCode": "DELIVERY_RETRIED",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "errorCode": "CHANNEL_UNKNOWN_ERROR",
    "errorMessage": "The channel suffered an internal error",
    "context": {
        "prop1": "value",
        ...
    },
    "timeStamp": "2021-07-13T20:17:56+0100"
}
```

### Message Delivery Failed

After the maximum number of delivery attempts through a channel is reached, the message enters in a permanent failure state, that means its delivery will not be attempted anymore.

If the message failed to be delivered had a fallback message configuration  then this is triggered

```json
{
    "id": "57721f483fa946ff9176ba86956057f0",
    "statusCode": "DELIVERY_FAILED",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "errorCode": "REJECTED_BY_CHANNEL",
    "errorMessage": "The channel actively rejected the delivery of the message",
    "fallbackTo": "4094c9a0dd1e4413b41ae3f35a465dae",
    "context": {
        "prop1": "value",
        ...
    },
    "timeStamp": "2021-07-13T20:18:18+0100"
}
```

Additionally to the normal fields present in the payload, if any _fallback_ was specified for the message, this event includes the reference to the next message in the chain (through the _fallbackTo_ field)

### Message Delivered

If a message is successfully delivered to the receiver, no futher attempts are done: this is the happy scenario of messaging.

``` json
{
    "id": "44c32607b09049f88e891d066ed57511",
    "statusCode": "DELIVERED",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "context": {
        "prop1": "value",
        ...
    },
    "timeStamp": "2021-07-13T20:19:31+0100"
}
```

### Message Read 

Some messaging channels are capable to provide a feedback to the sender to notify if a message was actually read by the receiver.

This feature is typically offered by _Over-the-Top_ (OTT) channels (like _WhatsApp_, _Viber_, _Facebook_ and others), although it might also fail to reach the sender with the notification, if the individual actively opted-out for such notification (to respect the personal privacy).

``` json
{
    "id": "2a3a51bb5e9c45fd8a60c01c39becb0e",
    "statusCode": "READ",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "context": {
        "prop1": "value",
        ...
    },
    "env": {
        "os.name": "ios",
        "os.version": "13.1",
        "app.name": "whatsapp",
        "app.version": "23.2",
        ...
    },
    "timeStamp": "2021-07-14T11:34:21+0100"
}
```

It is possible to receive some environment information, when the channels support such capability

### Message Deleted

```json
{
    "id": "57721f483fa946ff9176ba86956057f0",
    "statusCode": "DELETED",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "context": {
        "prop1": "value",
        ...
    },
    "timeStamp": "2021-07-15T23:17:51+0100"
}
```

**Note**: the notification of this event is not assured

* The event might never occur (the receiver doesn't care deleting the message on its device)
* The event might occur after the expiration of the message within the cache
* The receiver has configured its device to not provide such feedback