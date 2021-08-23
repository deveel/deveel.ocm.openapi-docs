---
layout: default
title: Message Options
parent: Outbound Messaging
nav_order: 2
permalink: /outbound/options
---

Users can control the behavior of the system for each single outbound message, through the specification of well-known _options_ that are interpreted by each channel.

Indeed options are specific to the type of channels used to send the message, to accommodate their idiosyncrasies 

This set is represented as a key/value pair within the _message_ object

## SMS

| Option              | Description                                    |
|---------------------|------------------------------------------------|
|

## E-mail

| Option              | Description                                    |
|---------------------|------------------------------------------------|
|

## Sandbox

| Option                     | Description                                                                     |
|----------------------------|---------------------------------------------------------------------------------|
| _test.send.result_         | The send result for the message in a given test scenario ('fail' or 'success')  |
| _test.send.errorCode_      | The error code returned, if 'test.send.result' is set to 'fail')                |
| _test.delivery.statusCode_ | A status code to be returned for the message (see the Delivery Codes)           |
| _test.delivery.errorCode_  | An code returned if the status code is an error                                 |
| _test.delivery.sleepTime_  | A number of seconds to wait before returning the status of the message          |