---
layout: default
title: Message Routes
parent: Inbound Messaging
---

# Message Routes

Customers who wish to receive messages should register the so-called _message route_, that are listeners of messages directed to specific terminals (eg. _telephone numbers_, _e-mail addresses_, etc.) of the customers, that Deveel OCM intercepts, and subsequently route them (in the form of webhooks) to given HTTP addresses.

_Message Routes_ require the following information:

* **To Address** - The terminal address where messages from individuals are directed to
* **Channel Name** - The name of the transportation channel instance that belonging to the user, used to filter the delivery of messages
* **Destination URL** - The HTTP address that is invoked to route the messsge, by sending a webhook that includes information of the sender and the message contents

Additionaly to the above configurations, customers can specify furth/er optional ones

* **Filter** - An additional filter (appended to the internal filters) that allows the user to further control the routing of messages (see the "filter formats" section)
* **Secret** - A secret word used to secure the access to the request to the destination URL where the message is forwarded
* **Headers** - Additional HTTP headers that are attached to the request that is transporting the incoming message to the given destination