---
title: 
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: eventnotification
---

## Event Notification System

The EMS Event Notification System provides an extremely powerful way of interacting with the EMS. At the basic level it allows you to easily understand and monitor the usage of your server. You can either poll and parse the log file, or simply subscribe to the HTTP based notifications sent out by the EMS. **The notifications mean that you can have a fully RESTful monitor, gathering metrics in real time!**

Beyond monitoring and gathering metrics, you can **use the Event Notification System to create custom stream processing.** If you want to automatically create HLS/HDS/MSS/DASH streams out of new inbound streams, simply call createHLSStream/createHDSStream/createMSSStream/createDASHStream in response to each "new inbound stream" event. If you want to close inbound streams when the associated outbound stream is lost, call shutdownStream when you receive an "outbound stream closed" event.

### List of Events

The API Definition.pdf document provides a list and full descriptions of each event. Please consult that document to learn more about the EMS Events.

### Configuring Event Notifications

Events can be sent to multiple destinations, or "sinks", at the same time. A "sink" can be either a file or a network destination. Multiple sinks can be enabled at the same time, allowing you to both log events and receive them in your web service(s). These sinks can be configured so that only the events you will be consuming will be generated. Event Sinks are configured in the `config/config.lua` file.

**Note:** Event Notifications are **off** by default. To use the Event Notification System, you must modify the EMS configuration file.

Please see the Configuration File section below for details on enabling the Event Notification System.

### Application vs. Server Events

The config.lua file has two eventLogger sections as follows:

1. Application-owned – This is lower in the file and is "inside" the application configuration section. It configures "application level" events. **This is the recommended configuration section to modify.**
2. Server-wide (or default) – This is higher in the file and is at the outer-most variable scope level. This section configures events that are outside the application or events which the application level fails to catch. This is typically only for system events like server startup, server shutdown and application load.


