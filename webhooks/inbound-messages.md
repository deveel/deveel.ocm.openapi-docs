---
layout: default
title: Inbound Messages
parent: Webhooks
nav_sort: 2
---

# Inbound Messages

The system allows to subscribe to the routing of messages sent from individuals to a **Terminal** (eg. a _telephone number_ or an _email address_) that is registered by a tenant (see the paragraph below).

These messages are transported to the consumer through a _webhook_ of a specific kind, named **Message Route**: users of the customers can subscribe to such delivert ahead of time (see the paragraph below).

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

## Receiving Terminals

To be able to subscribe to the notification of incoming messages, a user must have been approved a terminal (e.g. _telephone number_, _e-mail address_, etc.), that is a point of contact from individuals.

The system is capable of tracing back the terminal of the user back to all the subscriptions for the notifications, activating the transfer of the messages to the destination points

**Note**: Remember that in this context the _sender_ identifies the individual that is sending the message to the customer, while the _receiver_ is a registered terminal of the customer that is the destination of the message

## Examples

### E-Mail

An example of an inbound e-mail message

``` json
{
    "id": "02ee4cc9b4e845e8b19a524907f6fb9c",
    "channel": {
        "name": "email-ch1",
        "type": "email",
        "id": "fe87e49d297c45848a6eeae06f733a78"
    },
    "sender": {
        "email": "someuser@example.com"
    },
    "receiver": {
        "email": "info@company.com"
    },
    "content": {
        "html": "<base64-encoded>"
    },
    "timeStamp": "2021-05-01 22:32:11.665+02",
    "context": {
        "campaignId": "11266355",
        ".event.id": "7b1cdc7997ce4f58afedd60b71a56ef6",
        ".event.timeStamp": "2021-05-01 22:32:11.665+02",
        ".webhook": "incoming e-mails"
    }
}
```

### SMS

``` json
{
    "id": "3a6a7ae211ab44208c4384924ebd4157",
    "channel": {
        "name": "test-sms-1",
        "type": "sms",
        "id": "abc443f877f3498aac303c07df042238"
    },
    "sender": {
        "number": "+4720852661"
    },
    "receiver": {
        "number": "+472122232"
    },
    "content": {
        "text": "STOP news"
    },
    "timeStamp": "2021-06-15 12:43:13.654+01",
    "context": {
        "product": "news",
        ".event.id": "7b1cdc7997ce4f58afedd60b71a56ef6",
        ".event.timeStamp": "2021-06-15 12:43:13.654+01",
        ".webhook": "stop messages"
    }
}
```