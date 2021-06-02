# Omni-Channel Messaging
The interface provided by this system supports a unified messaging experience, allowing users to send or receive messages through multiple types of messaging channels (eg. _SMS_, _WhatsApp_, _E-mail_, etc.).

Messages that go from a client application of Deveel OCM to an individual are named _outbound messages_, while the messages incoming from individuals to a client application (through webhook callbacks) are named _inbound messages_: it will be possible to find several time in these documents reference to such directions.  

## Outbound Messaging
Sending messages to individuals requires the integration of the messaging APIs that support a unified design, independently from the channels, and their typical idiosynchrasies.

Applications must specify, for every message, at least the following information to be able to send messages:
* **Sender** - An authorized terminal (eg. _e-mail address_, _alpha-numeric label_, _telephone number_, etc.) that is displayed to the receiver as the sender of the message 
* **Receiver** - The terminal belonging to an individual that is receiving the message
* **Content** - A channel-aware content of the message
* **Channel** - The name of the pre-configured channel used to transport the message (**note**: this must be previously provisioned by Deveel)

Please refer to the [Single Message Send](#operation/message_send) or [Batch Message Send](#operation/message_batchSend) operations for more details

## Inbound Messaging

### Message Receivers

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

## Inbound Messages

# Pre-Requisites

Accessing the Messaging APIs and eventually consuming the webhooks produced by the system requires some pre-requisites, in terms of security (_tenant_, _user credentials_) and provisioned configurations (_channels_, _quotas_).

## User Credentials

Before you can start consuming the APIs provided by Deveel OCM you must obtain valid credentials for the supported authentication schemes (at today, just _OAuth2 Client Credentials_).

Contact _info@deveel.com_ to request for the creation of a valid user

### Authentication
Client applications and users must be authenticated using one of the following security schemes

<SecurityDefinitions />

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