# Omni-Channel Messaging

The interface provided by this system supports a unified messaging experience, allowing users to send or receive messages through multiple types of messaging channels (eg. _SMS_, _WhatsApp_, _E-mail_, etc.).

Messages that go from a client application of Deveel OCM to an individual are named _outbound messages_, while the messages incoming from individuals to a client application (through webhook callbacks) are named _inbound messages_: it will be possible to find several time in these documents reference to such directions.  

## Outbound Messaging

Sending messages to individuals requires the integration of the messaging APIs provided by the Omni-Channel Service: this offers you a unified design, independently from the channels, and their typical idiosynchrasies, so that integrators will not have to handle multiple integration strategies when trying to reach individuals through multiple messsging channels.

For every message, applications must specify, at least the following information when sending messages:

* **Sender** - A terminal (eg. _e-mail address_, _alpha-numeric label_, _telephone number_, etc.), previously validated and authorized by Deveel, that is displayed ton the device of the receiver as the sender of the message 
* **Receiver** - The terminal belonging to an individual that is receiving the message
* **Content** - A _channel-aware_ content of the message (_for example_, it is not possible to send HTML contents throw a SMS channel)
* **Channel** - The name of one of the pre-configured channels (that can be obtained calling the [clhannel isting endpoint](#channel_getPage) ) used to transport the message (**note**: this must be previously provisioned by Deveel)

Please refer to the [Single Message Send](#operation/message_send) or [Batch Message Send](#operation/message_batchSend) operations for more details

### Message Terminals

The capability of establishing a messaging conversation between two or more parties is enabled by the _terminal points_ of  ommunication, that are the source and destination of messages.

It is possible to edetermine the kind of terminal that is possible to use when sending _outbound messages_ throught a type of channel, as described in this matrix

| Channel Type | Phone Number | E-Mail Address | Alpha-Numeric Label |
|--------------|--------------|----------------|---------------------|
| sms          |   √          |    x           |   √                 |
| email        |   x          |    √           |   x                 |
| whatsapp     |   √          |    x           |   x                 |
| facebook     |   √          |    x           |   x                 |
| sanbox       |   √          |    √           |   √                 |

### Message Contents

Although the foundational idea of the omni-channel messaging is _to be able to reach the users with one single message throught multiple channels_, this ambition is sometimes limited by the capabilities of the channels to transport certain type of contents.

Developers who integrate the _Deveel Omni-Channel API_ should be aware of the kind of content is allowed by the channels they are using in the messaging requests.

Some channels support multiple more than one content type (even within the same message, as a _multi-part_).

| Channel Type | Text  | HTML | Media | Multi-Part |
|--------------|-------|------|-------|------------|
| sms          |   √   |   x  |   x   |      x     |
| email        |   √   |   √  |   x   |      √     |
| facebook     |   √   |   x  |   √   |      x     |
| sanbox       |   √   |   √  |   √   |      √     |

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

## Inbound Messaging
Dealing with the other direction of the messaging scenarios, the Omni-Channel Service offers also a unified design to receive messages from individuals, through multiple channels of communication, relieving the integrators from having to deal with multiple formats and protocols to interpret the requests from the operators.

Dealing with the other direction of the messaging scenarios, the _Omni-Channel Service_ offers also a unified design to receive messages from individuals, through multiple channels of communication, relieving the integrators from having to deal with multiple formats and protocols to interpret the requests from the operators.

### Message Receivers

Customers who wish to receive messages should register the so-called _message receivers_, that are listeners of messages directed to specific terminals (eg. _telephone numbers_, _e-mail addresses_, etc.) of the customers, that Deveel OCM intercepts, to subsequently route them (in the form of webhooks) to given HTTP addresses.

_Message Receivers_ require the following information:

* **To Address** - The terminal address where messages from individuals are directed to
* **Destination URL** - The HTTP address that is invoked to route the messsge, by sending a webhook that includes information of the sender and the message contents

Additionaly to the above configurations, customers can specify furth/er optional ones

* **Filter** - An additional filter (appended to the internal filters) that allows the user to further control the routing of messages (see the "filter formats" section)
* **Secret** - A secret word used to secure the access to the request to the destination URL where the message is forwarded
* **Headers** - Additional HTTP headers that are attached to the request that is transporting the incoming message to the given destination
**Channel Name** - The name of the transportation channel instance that belonging to the user, used to filter the delivery of messages
**Channel Type** - The type of transportation channel, used to further filter the delivered messages

# Webhooks

The HTTP notifications delivered to the consumers of the system can be (at today) of two kinds:

* **Message State Change** - Indicates a change to the state of a message sent by the user
* **Inbound Message** - A message sent from an individual to a given address through a channel

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

## Inbound Messages

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

# Pre-Requisites

Accessing the outbound Messaging APIs and eventually consuming the webhooks produced by the system requires some pre-requisites, in terms of security (_customers_, _tenants_, _user credentials_) and provisioned configurations (_channels_, _terminals_, _quotas_).

## Customer and Tenants

The major pre-requisite to enable the integration of the API is the creation and assignment by **Deveel** of a _customer_ object, to hold the financial relationships (invoices, payments), and a _tenant_ within that customer, that is a space where certain resources (_terminals_ and _channels_) are made available to users

## User Credentials

Before you can start consuming the APIs provided by Deveel OCM you must obtain valid credentials for the supported authentication schemes (at today, just _OAuth 2.0 Client Credentials_ and _OAuth 2.0 Authorization Code_).

Contact _info@deveel.com_ to request for the creation of a valid user

### Authentication

Client applications and users must be authenticated using one of the following security schemes

<SecurityDefinitions />

## Channels

Messaging operations, both inbound and outbound require tenants to have instances of actve _messaging channels_, that can be either provided by an inventory owned by Deveel, or rather configured by the customers themselves.

Channels available to customers, either provisioned or ported, are instances and thus must be named to be uniquely  identified by the system: in fact a user could have multiple instances of a type of channel (eg. _email_, _sandbox_, _sms_, etc.), each configured with specific settings and quotas.

When sending messages outbound, the name if the channel instance must be specified, to drive the message through the correct route (type of channel, providers, quotas, etc.), while when receiving messages inbound the channel is identified by the system, based on subscriptions.

The system currently supports the following types of channels

| Channel Type  | Description                                            |
|---------------|--------------------------------------------------------|
| sms           | The Simple Message System channel transporting simple textual messages to mobile phones |
| email         | The channel used to transport e-mail messages          |
| facebook      | The Facebook Messenger proprietary channel             |
| sandbox       | A virtual channel that can be used for testing         |


## Terminals

Commhnication standards describe models where two (or more) parties are conversing through _terminals_, that are addresses where the messages are directed or originated.

Most common types of terminals are _e-mail addresses_, _phone numbers_ (technically namec MSISDN), _profile identifiers_ (eg. Facebook Page ID or Facebook Profile ID), or even _alpha-numeric labels_ (when sending messages one-way only).

### Server Terminals

Users must use validated and active terminals when sending messages outbound or receiving messages inbound: differently from individuals terminals, the ones used for mass messaging operations must undergo a strict rule of control, to ensure the system is not enabling activities not compliant with laws and regulations.

The system validates that every message contains a valid _sender_, owned by the tenant of the user and compatible with the transportation channel: trying to send messages from a terminal that is disabled or not under control of the tenant of the user generates an immediate validation error.

## Quotas

Messaging channels might be configured with some quotas that limit the usage, in a period of time, within a given set of boundaries: this is typically the result of a commercial agreement between **Deveel** and the customer.  

# Errors

We follow the error response format proposed in [RFC 7807](https://tools.ietf.org/html/rfc7807)
also known as Problem Details for HTTP APIs.  As with our normal API responses,
your client must be prepared to gracefully handle additional members of the response.

An example of the JSON payload of such errors is the following:

```json
{
    "type": "https://ocm.deveel.net/errors/channel-not-found",
    "status": 404,
    "title": "Channel Not Found",
    "details": "The channel 'test-channel' was not found in the context of the tenant '302a06b789b747eeb313a82ed5e6a218'",
    "code": "CHANNEL_NOT_FOUND"
}
```

# API References
