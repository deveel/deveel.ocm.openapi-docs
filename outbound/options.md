---
layout: default
title: Message Options
parent: Outbound Messaging
nav_order: 2
permalink: /outbound/options
---

Users can control the behavior of the system for each single outbound message, through the specification of well-known _options_ that are interpreted by each channel.

Some options are generic to control the overall behavior of the process, while some others are specific to the type of channel used to send the message, to accommodate its _idiosyncrasies_

This set is represented as a key/value pair within the _message_ object

## All Channels

| Options             | Data Type       | Description                                                  |
|---------------------|-----------------|--------------------------------------------------------------|
| retry.count         | integer number  | The number of tries the system should attempt before failing |

## SMS

| Option              | Description                                    |
|---------------------|------------------------------------------------|
|

## E-mail

| Option              | Data Type       |  Description                                                                          |
|---------------------|-----------------|---------------------------------------------------------------------------------------|
| priority            | string          | The priority to give to the message (either 'high', 'normal', 'below normal', 'low' ) |

## Sandbox

| Option                     | Description                                                                     |
|----------------------------|---------------------------------------------------------------------------------|
| _test.send.result_         | The send result for the message in a given test scenario ('fail' or 'success')  |
| _test.send.errorCode_      | The error code returned, if 'test.send.result' is set to 'fail')                |
| _test.delivery.statusCode_ | A status code to be returned for the message (see the Delivery Codes)           |
| _test.delivery.errorCode_  | An code returned if the status code is an error                                 |
| _test.delivery.sleepTime_  | A number of seconds to wait before returning the status of the message          |