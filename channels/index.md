---
layout: default
title: Channels
nav_order: 1
permalink: /channels
has_children: true
---

# Messaging Channels

Messaging operations, both inbound and outbound require [Tenants](/tenants) to have instances of actve _messaging channels_, that can be either provided from an inventory owned by Deveel, or rather configured by the customers themselves.

Channels available to customers (either provisioned or _ported_) are instances and thus must be named to be uniquely identified by the system: in fact a user could have multiple instances of the same type of channel (eg. _email_, _sandbox_, _sms_, etc.), each configured with different settings and quotas, for different purposes.

When sending messages [outbound](/outbound), the name of the channel instance must be specified, to drive the message through the correct route (_type of channel_, _providers_, _quotas_, etc.), while when receiving messages inbound the channel is identified by the system, based on subscriptions (see the section on [Message Receivers](/inbound/receivers)).

## Types of Channel

The system currently supports the following types of channels

| Channel Type  | Description                                                                             |                                 |
|---------------|-----------------------------------------------------------------------------------------|---------------------------------|
| sms           | The Simple Message System channel transporting simple textual messages to mobile phones | [Read More](/channels/sms)      |
| email         | The channel used to transport e-mail messages                                           | [Read More](/channels/email)    |
| facebook      | The Facebook Messenger proprietary channel                                              | [Read More](/channels/facebook) |
| sandbox       | A virtual channel that can be used for testing                                          | [Read More](/channels/sandbox)  |

## Channel Providers

Behind the scenes, the system has two different kinds of support of the messaging channels:

1. _Organic Channels_ - Implementations of channels made within the system itself
2. _External Channels_ - Implementations of _plugins_ towards external providers of messaging services

This kind of classification is not directly related to the _type of channel_, and in fact there can be more implementations of the same type of channel available (either _organic_ or _external_): for example, the SMS channel can be provided by three different vendors ([Twilio](https://twilio.com), [Link Mobility](https://linkmobility.com), etc.), while the [Facebook](https://facebook.com/messenger) channel is implemented by Deveel itself.

The ability to have multiple providers of the same type of channel is also functional to the _portability_ of customer's channel instances (see the section on [Porting Channels](/channels/port))