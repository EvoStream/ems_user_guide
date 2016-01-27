---
title: EMS Web Services 
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: emswebservices
---

The EMS Web Services provide a suite of RESTful web services that leverage the EMS Run-Time API and EMS Event Notification system to extend and enhance the EvoStream Media Server. The EMS Web Services can be used in production and/or built from to fit specific needs and requirements. These web services are included in the EMS package or distribution.

EvoStream currently provides the following web services:

- **Stream Recorder:** Tells the EMS to automatically record any stream that has a particular, customizable, naming convention.
- **Load Balancer**: Provides a mechanism for maintaining all source streams on multiple EMS instances. This allows for automatic redundancy and/or provides multiple source servers to pull from.
- **Auto Router**: Automatically forward source streams with a particular, customizable, naming convention to another server. Destination server can be another EMS, a popular CDN, or any other streaming server.
- **HLS/HDS Amazon Upload**: Automatically upload HLS and HDS streams to your Amazon S3 buckets for easy and massive distribution.
