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

## Inbound Messaging
Dealing with the other direction of the messaging scenarios, the Omni-Channel Service offers also a unified design to receive messages from individuals, through multiple channels of communication, relieving the integrators from having to deal with multiple formats and protocols to interpret the requests from the operators.

### Message Receivers
Customers who wish to receive messages should register the so-called _message receivers_, that are listeners of messages directed to specific terminals (eg. _telephone numbers_, _e-mail addresses_, etc.) of the customers, that Deveel OCM intercepts, to subsequently route them (in the form of webhooks) to given HTTP addresses.

Message Listeners require the following information:
* **To Address** - The terminal address where the messages are directed to
* **Destination URL** - The HTTP address that is invoked to route the messsge, by sending a webhook that includes information of the sender and the message contents

Additionaly to the above configurations, customers can specify further optional ones
* **Filter** - An additional filter (appended to the internal filters) that allows the user to further control the routing of messages (see the "filter formats" section)
* **Secret** - A secret word used to secure the access to the request to the destination URL where the message is forwarded
* **Headers** -

# Webhooks

## Message State Changes

The system produces a set of HTTP callbacks (_webhooks_), to inform subscribing applications of the changes of state of a message that was previously sent through the APIs.

The payload of the callback (an HTTP request) is typically composed of the following elements:

| Property   | Description |
|------------|-------------|
| messageId  | The unique identifier provided as response from the messaging APIs
| statusCode | Indiates the status of the message

### Message Queued
This callback notifies the listening application that a message was queued and it will be processed soon.

```json
{
    "statusCode": "QUEUED",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "context": {
        "prop1": "value1",
        "prop2": 33,
        ...
    }
    ...
}
```

### Message Sent

This callback notifies that the given message was sent through the channel and we are waiting to know if it was _delivered_ or if its _delivery failed_.

```json
{
    "statusCode": "SENT",
    "messageId": "a28fe2bb188642c8b175f816fa32aa0e",
    "channelName": "channel-1",
    "context": {
        "prop1": "value",
        ...
    }
}
```

### Message Delivery Retried

### Message Delivery Failed

### Message Delivered

## Inbound Message


# Pre-Requisites

Accessing the outbound Messaging APIs and eventually consuming the webhooks produced by the system requires some pre-requisites, in terms of security (_customers_, _tenants_, _user credentials_) and provisioned configurations (_channels_, _terminals_, _quotas_).

## Customer and Tenants


## User Credentials

Before you can start consuming the APIs provided by Deveel OCM you must obtain valid credentials for the supported authentication schemes (at today, just _OAuth 2.0 Client Credentials_ and _OAuth 2.0 Authorization Code_).

Contact _info@deveel.com_ to request for the creation of a valid user

### Authentication
Client applications and users must be authenticated using one of the following security schemes

<SecurityDefinitions />

## Channels

## Terminals

## Quotas 


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