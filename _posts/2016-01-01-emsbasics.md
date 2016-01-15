---
title: 
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: emsbasics
---

# EMS Basics

There are a number of things that are good to keep in mind when interacting with the EvoStream Media Server.

## Streams

**Stream directionality** is always from the perspective of the server itself. So when a pullstream is executed, you are always telling the server to go get a stream to bring into it. Conversely pushstream implies taking a stream that is already within the EMS and forcibly sending it to an external destination.

When you pull, push, or create a stream, the command is logged in the `config/pushPullSetup.xml` configuration file. This is the default behavior and allows commands to be persistent if you stop the server and then restart it. In other words, if you pull in two streams, and then stop the server, the next time you start the server it will try to reconnect those two streams.

The logging of commands can be skipped by changing the "keepAlive" parameter in pullstream and pushstream. By setting keepalive=0, the command will not be logged, and if the stream disconnects the server will not try to reconnect to it.

If you wish to "start clean" the pushPullSetup.xml file can simply be deleted prior to starting the EMS.

All in-bound streams have a `localStreamName` that is used to uniquely identify that stream. It is used in play requests and can be used to identify streams in some API calls. It is important to note that no two streams may have the same localStreamName. The EMS will return an error if you try to "pull" a second stream with a localStreamName that has already been used.

## Config Files

The Configuration files are described in better detail later in this document. This will section serve as more of an introduction to the configuration files used by the EvoStream Media Server.

### LUA

The EMS uses the LUA scripting language for many of its configuration files. LUA is an extremely powerful scripting language that allows you to do many things from executing programs to interacting with databases. Typically, the EMS configuration files only trivially use LUA. The configuration files are no more than a collection of statically defined LUA variables.

The use of LUA provides users with a unique ability to dynamically configure the EvoStream Media Server. For example, if you wanted to pull authentication information from a database that is regularly updated you would simply need to replace the contents of the _users.lua_ file with the LUA script to query your authentication database. The EMS will then automatically query your database for authentication details at runtime!

The LUA scripting language is easy to learn and has had excellent acceptance in the software community. The game World of Warcraft® relies heavily on the LUA scripting language. You will be able to clearly understand the contents of the EMS configuration files even if you have never seen a LUA script before.

### EMS Configuration Overview

**config.lua** – This is the main configuration file for the EMS. This file defines all of the startup parameters used by the server, including the location and names of all of the other configuration files. If you wish to change the name of any of the subsequent configuration files, you can do so here. This file is also just a command-line parameter to the EMS executable. The run-scripts provided with the EMS distribution use this file by default. If you want to change the location or name of this file you can simply modify the run scripts to use a different file.

If you modify this file and the server then fails to start, you have made an error. You can either roll-back your changes or you can use the `--use-implicit-console-appender` command line parameter to get extra debug information about what failed during startup.

**pushPullSetup.xml** – The most important thing to know about the pushPullSetup.xml file is that it **should not be modified manually**! This file is used for internal purposes only. If, during startup, the EMS detects that changes have been made to the pushPullSetup.xml file, it will rename the existing pushPullSetup.xml file and start with a fresh configuration.

Now that the disclaimer is out of the way, it is important to understand how this file is used. When a pullStream, pushStream, createHLSStream, createHDSStream, createMSSStream, createDASHStream, record, or launchProcess, or startWebRtc command is executed, that command is logged to this file (assuming the keepAlive flag is 1, which it is by default). When the EMS is started, it parses this file and attempts to recreate all of the connections. These configuration entries can be removed by issuing `removeConfig` commands, or by setting the keepAlive flag to 0 when the initial command is made.

**Note:** If you wish to have a "clean start" of the server, with no previous streams, you may delete this file before starting the EMS.

### EWS Configuration Overview

**webconfig.lua** – This is the main configuration file of the EvoStream Web Server (EWS). This file sets the locations of important web server files/folders such as the web root folder, server key & certificate, white list, black list, logs, etc. This is where other web server settings are defined, including HTTP port, group name aliasing, SSL mode, connection/memory limits, mime types, etc.

Some of the more critical settings and their defaults:

| Settings | Default Value | Notes |
| --- | --- | --- |
| Port | 8888 | - |
| webRootFolder | Linux Installer: /var/evo-webroot/Linux .tar: [EMSInstallDir]/evo-webrootWindows: C:\EvoStream\evo-webroot | - |
| hasGroupNameAliases | False | This parameter turns on or off (off is default) Group Name Aliasing, a powerful security feature. Group Name Aliasing does change the behavior of the EWS drastically, so please make sure to read up on Group Name Aliasing before enabling! |
| threadPoolSize | 4 | Depending on your planned HTTP load and use case, increasing the thread pool might be necessary. |

## Video Compression

The EvoStream Media Server requires that the video streams be encoded as H.264 data. H.264 has many different options and configurations. The EMS can support virtually every _valid_ H2.64 stream with a few exceptions:

**Widely Varying GOP Sizes** – The EMS works best when there are a consistent number of P-Frames per I-Frame. This is particularly true when creating file-based outbound streams like HLS.

## Production Logging

The EMS provides production logging through the Event Notification System which you can read more about below. The production logs allow you to track streaming sessions, data transfers, invalid requests and more. Logging can be done in XML, JSON or formatted for W3C.

- Log HTTP streaming sessions for HLS, DASH, MSS and HDS. Get a SINGLE log entry for the streaming session across the multiple file requests inherent to HTTP streaming.
- Log live stream and file accesses
- Log recording events
- Log both start and end record and streaming events to take action in real-time

## Debug Logging

The EMS provides system level logging which is turned on by default. This logging assists in integration and debugging efforts. The logs can be found in the `logs/` directory in either the main EMS distribution (from archive installation) or in the `/var/log/evostreamms/` directory when using the Linux installer and in C:\EvoStream\logs\ in Windows®.

### Log Accumulation

The EMS logs constantly while it is running, which may have negative impacts on disk usage over the course of time. This can be mitigated by either quieting the logs, or disabling logging all-together. To quiet logs, edit the `logLevel` configuration value in config.lua. 6 is the highest (most prolific) level of logging, 0 is the lowest.

To disable logging completely, remove or comment out any "file" type `logAppender` in the config.lua file. Alternatively, set logLevel to -1 to disable logging without removing the logAppender entry.

See the configuration section of this document for more information on manipulating the config.lua file.


