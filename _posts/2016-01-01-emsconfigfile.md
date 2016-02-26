---
title: EMS Configuration File
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: emsconfigfile
---

## config.lua

The EMS configuration file, config.lua, is a hierarchical data structure of assignments (key names with values). It is sent as a parameter when running the EvoStream server. The format is as follows:

`<keyname>= <value>`

where `<value>` could be any of the following types:

- string = series of alpha numeric characters enclosed in double quotes
- number = digits (without double quotes around it)
- array = list of values separated by comma and is grouped by braces {}
- aliases = {"flvplayback1", "vod1", "live"}

**Example:**


    aliases = {“flvplayback1”, “vod1”, “live”}


**object =** list of assignments enclosed by braces {}

**Example:**

    configurations =
    {
        daemon = "true",
        pathSeparator = "/",
        logAppenders = {...},
        applications = {...}
    }


In the example above, configurations has a value of type object. An object is a group of data inside braces (`{ }`) which may contains several assignments (`<keynames> = <values>`) separated by commas (`,`) and in turn could be another object. The assignments in the example above are `daemon`, `pathSeparator`, `logAppenders`, and `applications`. Notice that the values of `logAppenders` and applications could be another object or array recursively.



**Contents**

    configuration = 
    {
    	daemon = false,
    	pathSeparator = “/”,
    	logAppenders = 
    	{
    		-- content removed for clarity
    	},
    	applications = 
    	{
    		-- content removed for clarity
    	}
    }}

**configuration** – This is the entire structure for all configuration needed by the EMS Server.

**Configuration Structure Table:**

|       Key        |   Type    | Mandatory | Description                              |
| :--------------: | :-------: | :-------: | ---------------------------------------- |
|      daemon      |  boolean  |    yes    | **true**  means the server will start in daemon mode. **false** means it will start in console mode (nice for development). |
|  pathSeparator   | string(1) |    yes    | This value will be used by the server to compose paths (like media files paths). Examples: on UNIX-like systems this is / while on windows is \. Special care must be taken when you specify this value on windows because \ is an escape sequence for Lua so the value should be "\\". |
|   logAppenders   |  object   |    yes    | Will hold a collection of log appenders. Each log message will be sent to each of the log appenders enumerated in this configuration section. |
|   eventLogger    |  object   |    No     | Settings for the server-wide event sinks |
|   applications   |  object   |    yes    | Will hold a collection of loaded applications. Besides that, it will also hold few other values. |
|  instancesCount  |  number   |    yes    | The number of virtual instances of EMS server where load balancing will be performed. If this item is missing, it will be replaced by 0, disabling multiple instances. If its value is -1, it will be replaced by the number of CPUs, enabling one or more additional instances. |
| clientSideBuffer |  Number   |    No     | The number of seconds that the EMS will buffer when behaving as an RTMP client. |
| maxRtmpOutBuffer |  Number   |    No     | The maximum amount of bytes the EMS will store in the output RTMP buffer |
| maxRTSPOutBuffer |  Number   |    No     | The maximum amount of bytes the EMS will store in the output RTSP buffer. Only used for RTSP when the final transport is RTP over TCP |

When the server starts, the following sequence of operations is performed:

The configuration file is loaded. Part of the loading process, is the verification,. If if something is wrong with the syntax, please try this:

- For Linux Package:
  
         /usr/bin/evostreamms –use-implicit-console-appender /etc/evostreamms/config.lua

- For Linux Archive:
  
         cd EMS_INSTALL_DIRECTORY
         ./evostreamms --use-implicit-console-appender ../config/config.lua
  
- For Windows:
  
         cd EMS_INSTALL_DIRECTORY
         evostreamms --use-implicit-console-appender config\config.lua


**Note:** EMS_INSTALL_DIRECTORY is the `bin` directory within the EvoStream Media Server Archive directory.

1. The "daemon" value is read. The server now will either fork to become daemon or continue as is in console mode.
2. The "logAppenders" value is read. This is where all log appenders are configured and brought up to running state. Depending on the collection of your log appenders, you may (not) see further log messages.
3. The "applications" value is taken into consideration. Up until now, the server doesn't do much. After this stage completes, all the applications are fully functional and the server is online and ready to do stuff.




### logAppenders


    logAppenders =
    {
        {
            name="console appender",
            type="coloredConsole",
            level=6
        },
        {
            name="file appender",
            type="file",
            level=6,
            fileName="../logs/evostream",
        }
    }


This section contains a list of log appenders. The entire collection of appenders listed in this section is loaded inside the logger at config-time. All log messages will be than passed to all these log appenders. Depending on the log level, an appender may (or may not) log the message. "Logging" a message means "saving" it on the specified "media" (in the example below we have a console appender and a file).

**logAppenders Structure Table:**

|        Key        |  Type   | Mandatory | Description                              |
| :---------------: | :-----: | :-------: | ---------------------------------------- |
|       name        | string  |    yes    | The name of the appender. It is usually used inside pretty print routines. |
|       type        | string  |    yes    | The type of the appender. Types `console` and `coloredConsole` will output to the console. The difference between them is that `coloredConsole` will also apply a color to the message, depending on the log level. Quite useful when eye-balling the console. Type `file` log appender will output everything to the specified file. |
|       level       | number  |    yes    | The log level used. The values are presented just below. Any message having a log level less or equal to this value will be logged. The rest are discarded. (**Log levels:** 0 FATAL, 1 ERROR, 2 WARNING, 3 INFO, 4 DEBUG, 5 FINE, 6 FINEST, -1 disable logs) |
|     fileName      | string  |    yes    | If the type of appender is a file, this will contain the path of the file. |
| newLineCharacters | string  |    no     | Newline character used in the file appender. |
|  fileHistorySize  | number  |    no     | The maximum number of log files to be retained. The oldest log file will be deleted first if this number is exceeded. |
|    fileLength     | number  |    no     | Buffer size of the file appender.        |
|    singleLine     | boolean |    no     | If yes, multi-line log messages are merged into one line. |

**Note:** When daemon mode is set to true, all console appenders will be ignored. (Read the explanation for daemon setting above).




### applications

    applications =
    {
        rootDirectory = "./",
        {
            name="evostreamms",
            -- settings for evostreamms
        },
        {
            name="anotherApplication",
            -- settings for anotherApplication
        },
    }


This section is where all the applications inside the server are placed. It holds the attributes of each application that the server will use to launch them. Each application may have specific attributes that it requires to execute its own functionality.

**application Structure Table:**

|      Key      |  Type  | Mandatory | Description                              |
| :-----------: | :----: | :-------: | ---------------------------------------- |
| rootDirectory | string |   true    | The folder containing applications subfolders. If this path begins with a "/" or "\" (depending on the OS), then is treated as an absolute path. Otherwise is treated as a path relative to the run-time directory (the place where you started the server). |

Following the _rootDirectory_, there is a collection of applications. Each application has its properties contained in an object. See details below.




### application Definition

    {
        appDir="./",
        name="evostreamms",
        description="EVOSTREAM MEDIA SERVER",
        protocol="dynamiclinklibrary",
        default=true,
        pushPullPersistenceFile="..\\config\\pushPullSetup.xml",
        authPersistenceFile="..\\config\\auth.xml",
        connectionsLimitPersistenceFile="..\\config\\connlimits.xml",
        bandwidthLimitPersistenceFile="..\\config\\bandwidthlimits.xml",
        ingestPointsPersistenceFile="..\\config\\ingestpoints.xml",
        streamsExpireTimer=10,
        rtcpDetectionInterval=15,
        hasStreamAliases=false,
        hasIngestPoints=false,
        validateHandshake=false,
        aliases={"er", "live", "vod"},
        maxRtmpOutBuffer=512*1024,
        hlsVersion=3,
        useSourcePts=false,
        enableCheckBandwidth=true,
        vodRedirectRtmpIp="",


This is where the settings of an application are defined. We will present only the settings common to all applications. Later on, we will also explain the settings particular to certain applications.

**application Definition Table:**

|               Key               |    Type    |                Mandatory                 | Description                              |
| :-----------------------------: | :--------: | :--------------------------------------: | ---------------------------------------- |
|              name               |   string   |                  False                   | Could be the name of the company, organization etc. |
|           Description           |   String   |                  False                   | Brief description of the "name"          |
|            Protocol             |   String   |                                          |                                          |
|             Default             |  Boolean   |                   True                   |                                          |
|     pushPullPersistenceFile     |   String   |                   True                   |                                          |
| connectionsLimitPersistenceFile |   String   |                   True                   |                                          |
|  bandwidthLimitPersistenceFile  |   String   |                   True                   |                                          |
|   ingestPointsPersistenceFile   |   String   |                   True                   |                                          |
|       streamsExpireTimer        |   Number   |                   True                   |                                          |
|      rtcpDetectionInterval      |   Number   |                                          |                                          |
|        hasStreamAliases         |  Boolean   |                   True                   |                                          |
|         hasIngestPoints         |  Boolean   |                   True                   |                                          |
|        validateHandshake        |  Boolean   |                   true                   |                                          |
|             aliases             |   String   | Designed to be used to protect\/hide your source streams |                                          |
|        maxRtmpOutBuffer         |   Number   |                                          |                                          |
|           hlsVersion            |   Number   |                   True                   | The HLS version. Supported versions are from 3-6. User should make sure of the version before creating HLS files. |
|          useSourcePts           |  Boolean   |                  False                   | If set to 1, the outbound stream will use the PTS of the source stream. Otherwise, the pushed stream will start at 0 |
|      enableCheckBandwidth       |  Boolean   |                   True                   |                                          |
|        vodRedirectRtmpIp        | IP Address |                  False                   | The IP address of the other server that  |




### media

    {
    mediaStorage = {
    	recordedStreamsStorage="../media",
    	{
    	description="Default media storage",
    	mediaFolder="../media",
    	},


This is where the settings of media folder defined. There are several uses of the media folder:

- Storage of vod file that is used for `pullStream`
- Location of the created file using `generateLazyPull` command
- Storage of recorded streams using record command

**media Structure Table:**

|          Key           |  Type  | Mandatory | Description                              |
| :--------------------: | :----: | :-------: | ---------------------------------------- |
| recordedStreamsStorage | String |   True    | The path of the media folder to be used in record streams |
|      description       | String |   False   | The description of the media storage     |
|      mediafolder       | String |   True    | The path of the media storage folder     |




### acceptors

The "acceptors" block is found within the "applications" section named "evostreamms" in the configuration file. Each acceptor protocol used by applications is defined here. Some protocols may require additional parameters.

**acceptor Structure Table:**

|   Key    |  Type  | Mandatory | Description                              |
| :------: | :----: | :-------: | ---------------------------------------- |
|    ip    | string |    yes    | The IP where the service is located. A value of 0.0.0.0 means all interfaces and all IPs. |
|   port   | string |    yes    | Port number that the service will listen to. |
| protocol | string |    yes    | The protocol stack handled by the ip:port combination. |

The following acceptor types are supported by EMS:



**acceptor Protocol Table:**

| Acceptor Protocol  | Typical IP | Typical Port |       Additional Parameters (Note)       | Protocol Stack (Tags) |
| :----------------: | :--------: | :----------: | :--------------------------------------: | :-------------------: |
|    inboundRtmp     |  0.0.0.0   |     1935     |                                          |        TCP+IR         |
|    inboundRtmps    |  0.0.0.0   |     8081     | sslKey (path to SSL key file),sslCert (path to SSL certificate file) |     TCP+ISSL+IRS      |
|    inboundRtmpt    |  0.0.0.0   |     8080     |                                          |       TCP+IH4R        |
|    inboundTcpTs    |  0.0.0.0   |     9999     |                                          |        TCP+ITS        |
|    inboundUdpTs    |  0.0.0.0   |     9999     |                                          |        UDP+ITS        |
|    inboundRtsp     |  0.0.0.0   |     5544     |                                          |       TCP+RTSP        |
|   inboundLiveFlv   |  0.0.0.0   |     6666     |        waitForMetadata (boolean)         |       TCP+ILFL        |
| inboundBinVariant  | 127.0.0.1  |     1113     |           clustering (boolean)           |       TCP+BVAR        |
|   inboundJsonCli   | 127.0.0.1  |     1112     |        useLengthPadding (boolean)        |     TCP+IJSONCLI      |
| inboundHttpJsonCli | 127.0.0.1  |     7777     |                                          | TCP+IHTT+H4C+IJSONCLI |
|   inboundAsciiCli  | 127.0.0.1  |     1222     |        useLengthPadding (boolean)        |      TCP+IASCCLI      |


**Protocol Group Table:**

| Protocol Group       | Tag      | Protocol Type          |
| -------------------- | -------- | ---------------------- |
| Carrier Protocols    | TCP      | TCP                    |
|                      | UDP      | UDP                    |
| Variant Protocols    | BVAR     | Bin Variant            |
|                      | XVAR     | XML Variant            |
|                      | JVAR     | JSON Variant           |
| RTMP Protocols       | IR       | Inbound RTMP           |
|                      | IRS      | Inbound RTMPS          |
|                      | OR       | Outbound RTMP          |
|                      | RS       | RTMP Dissector         |
| Encryption Protocols | RE       | RTMPE                  |
|                      | ISSL     | Inbound SSL            |
|                      | OSSL     | Outbound SSL           |
| MPEG-TS Protocol     | ITS      | Inbound TS             |
| HTTP Protocols       | IHTT     | Inbound HTTP           |
|                      | IHTT2    | Inbound HTTP2          |
|                      | IH4R     | Inbound HTTP for RTMP  |
|                      | OHTT     | Outbound HTTP          |
|                      | OHTT2    | Outbound HTTP2         |
|                      | OH4R     | Outbound HTTP for RTMP |
| CLI Protocols        | IJSONCLI | Inbound JSON CLI       |
|                      | H4C      | HTTP for CLI           |
|                      | IASCCLI  | Inbound ASCII CLI      |
| RPC Protocols        | IRPC     | Inbound RPC            |
|                      | ORPC     | Outbound RPC           |
| Passthrough Protocol | PT       | Passthrough            |




### autoHLS/HDS/DASH/MSS

Within the "evostreamms" application section of the config.lua file, you will need to uncomment out the autoHLS group. (To uncomment it remove the "--[[" and "]]--" strings).

``` 
autoDASH=

    targetFolder= "..\\evo-webroot",
},
autoHLS=
{
    targetFolder= "..\\evo-webroot",
},
autoHDS=
{
    targetFolder= "..\\evo-webroot",
},
autoMSS=
{
    targetFolder= "..\\evo-webroot",
},
```

The `autoHLS/HDS/DASH/MSS` configuration group defines the parameter settings that will be used when the `createHLS/HDS/DASH/MSSStream` is automatically called on stream creation. Since targetFolder is the only mandatory field in create stream, that value MUST be specified in the autoHLS section. All other parameters can be specified if you want to override the default values.




### authentication

The "authentication" block is found within the "applications" section named "evostreamms" in the configuration file. Authentication settings for RTMP and RTSP protocols are defined separately. For RTMP, another file, `auth.xml`, is required to enable authentication. In addition, a users file, typically named `users.lua`, provides the user names and passwords.

**authentication Structure Table:**

| Protocol |    Parameter     | Mandatory | Typical Setting               |
| :------: | :--------------: | :-------: | ----------------------------- |
|   RTMP   |       type       |   true    | "adobe"                       |
|   RTMP   |  encoderAgents   |   true    | "FMLE…" _(see below)_         |
|   RTMP   |    usersFile     |   true    | "../config/users.lua"         |
|   RTSP   |    usersFile     |   true    | "../config/users.lua"         |
|   RTSP   | authenticatePlay |   false   | true (default value is false) |

``` 
authentication=
{
    rtmp=
    {
        type="adobe",
        encoderAgents=
        {
            "FMLE/3.0 (compatible; FMSc/1.0)",
            "Wirecast/FM 1.0 (compatible; FMSc/1.0)",
            "EvoStream Media Server (www.evostream.com)"
        },
        usersFile="..\\config\\users.lua"
    },
    rtsp=
    {
        usersFile="..\\config\\users.lua",
        --authenticatePlay=false,
    }
},
```

**Note:**

Authentication is disabled if the "authentication" block in the "config.lua" file is missing or incomplete. For RTMP protocol, authentication is disabled if the "auth.xml" file is missing or contains a "false" setting. For RTSP protocol, authentication is disabled if "authenticatePlay" in the "rtsp" block is omitted or set to "false".




### eventLogger

To enable Event Notifications you will need to enable/uncomment the eventLogger section of the config.lua file. Comments in LUA are specified by either a "--" for a single line, or denoted by a "--[[" to start a comment block and a "]]--" to end a comment block. By default the eventLogger section is commented out using the block style comments, so you will need to remove both the --[[and]]--strings.

The configuration entry must be constructed as follows:

``` 
eventLogger=
{
    sinks=
    {
        type="sink1_type",
        -- property1 of sink1
        -- property2 of sink1
    },
    enabledEvents=
    {
        "inStreamCreated",
        "inStreamClosed",
    }
},
```

The `enabledEvents` parameter is optional and allows you to specify only the events which you wish to receive. If the enabledEvents section is not specified, all events will be generated.

All event types are listed below.

**Stream Events Table:**

| Event                  | Description                              |
| ---------------------- | ---------------------------------------- |
| inStreamCreated        | A new inbound stream has been created    |
| outStreamCreated       | A new outbound stream has been created   |
| streamCreated          | A new neutral (neither in nor out) stream has been created |
| inStreamClosed         | An inbound stream has been closed        |
| outStreamClosed        | An outbound stream has been closed       |
| streamClosed           | A neutral stream has been closed         |
| inStreamCodecsUpdated  | The audio and/or video codecs for this inbound stream have been identified or changed |
| outStreamCodecsUpdated | The audio and/or video codecs for this outbound stream have been identified or changed |
| streamCodecsUpdated    | The audio and/or video codecs for this neutral stream have been identified or changed |
| audioFeedStopped       | The audio feed has stopped for an extended period |
| videoFeedStopped       | The video feed has stopped for an extended period |



**Adaptive Streaming/File-based Streaming Events Table:** 

| Event                    | Description                              |
| ------------------------ | ---------------------------------------- |
| hlsChildPlaylistUpdated  | Stream specific HLS playlist has been modified |
| hlsMasterPlaylistUpdated | HLS group playlist has been modified     |
| hlsChunkCreated          | A new HLS segment was opened on disk     |
| hlsChunkClosed           | A new HLS segment has been completed and is ready on disk |
| hlsChunkError            | A failure occurred when writing to an HLS segment file |
| hdsChildPlaylistUpdated  | Stream specific HDS manifest has been modified |
| hdsMasterPlaylistUpdated | HDS group manifest has been modified     |
| hdsChunkCreated          | A new HDS segment file has been opened   |
| hdsChunkClosed           | A new HDS segment has been completed and is ready on disk |
| hdsChunkError            | A failure occurred when writing to an HDS segment/fragment file |
| mssChunkCreated          | A new MSS fragment file has been opened  |
| mssChunkClosed           | A new MSS fragment has been completed and is ready on disk |
| mssChunkError            | A failure occurred when writing to an MSS fragment file |
| mssPlaylistUpdated       | MSS manifest has been modified           |
| dashChunkCreated         | A new DASH fragment file has been opened |
| dashChunkClosed          | A new DASH fragment has been completed and is ready on disk |
| dashChunkError           | A failure occurred when writing to an DASH fragment file |
| dashPlaylistUpdated      | DASH manifest has been modified          |
| recordChunkCreated       | A new record fragment file has been opened |
| recordChunkClosed        | A new record fragment has been completed and is ready on disk |
| recordChunkError         | A failure occurred when writing to a record fragment file |



**API Based Events Table**

| Event          | Description                              |
| -------------- | ---------------------------------------- |
| cliRequest     | The EMS has received a Runtime API command |
| cliResponse    | The response generated by the EMS for the last Runtime API command |
| processStarted | A process has been started at the request of the launchProcess API command |
| processStopped | A process started via the launchProcess API command has been stopped |
| timerCreated   | A new timer has been created via the setTimer API command |
| timerTriggered | The requested timer event                |
| timerClosed    | Indicates the timer is no longer valid and will not create any futher timerTriggered events |



**Connection Based Events Table**

| Event                       | Description                              |
| --------------------------- | ---------------------------------------- |
| protocolRegisteredToApp     | A connection has been fully established  |
| protocolUnregisteredFromApp | A connection has been disconnected       |
| carrierCreated              | Some IO handler, such as a TCP socket, has been created. This is not analogous to a connection creation. |
| carrierClosed               | Some IO handler, such as a UDP socket, has been closed. This is not analogous to a connection being closed. |



**Application Based Events Table**

| Event            | Description                              |
| ---------------- | ---------------------------------------- |
| applicationStart | The internal EMS application has started |
| applicationStop  | The internal EMS application has stopped, likely indicating a shutdown is about to occur |
| serverStarted    | The EMS has fully started                |
| serverStopping   | The EMS is about to shutdown. This is sent as late as possible, but clearly not after shutdown has been completed |



**Web Server Events Table**

| Event                   | Description                            |
| ----------------------- | -------------------------------------- |
| streamingSessionStarted | A streaming session has been started   |
| streamingSessionEnded   | A streaming session has been completed |
| mediaFileDownloaded     | A file download has been completed     |




#### Event Sinks

There are two main types of event sinks:

1\. **File Event Sink** – Event details are written to a log file located relative to the current directory. The log file is overwritten each time the EMS starts up.

    type="file",
    filename="log.txt",
    format="text",
    customData="my custom data"


File sink configuration:

The format can be one of the following types:

- "text" (plain text)
  
- "xml"
  
- "json"
  
- "w3c"
  
  A typical configuration of a file sink follows:
  
  ``` 
    eventLogger=
    {
        sinks=
        {
            {
                type="file",
                filename="../logs/events.txt",
                --format="text",
                --format="xml",
                --format="json",
                format="w3c",
                timestamp=true,
                appendTimestamp=true,
                appendInstance=true,
                fileChunkLength=43200, -- 12 hours (in seconds)
                fileChunkTime="18:00:00",
                enabledEvents=
                {
                    "inStreamCreated",
                    "outStreamCreated",
                    "streamCreated",
                    -- content removed for clarity
                },
                {
                    -- content removed for clarity
                },
            },
        },
    },
  ```



**File Sink Structure Table:**

|       Key       |  Type   | Mandatory | Description                              |
| :-------------: | :-----: | :-------: | ---------------------------------------- |
|   customData    | object  |    no     | Custom data that will be appended to all events generated by this sink. It overrides the custom data node defined on the upper level. It can also be a complex structure, see illustration above. |
|      type       | string  |    yes    | The type of sink, "file".                |
|    filename     | string  |    yes    | The base name of the file.               |
|     format      | string  |    yes    | Sets the file format. Allowed values are "text", "xml", "json" and "w3c" |
|    timestamp    | boolean |    no     |                                          |
| appendTimestamp | boolean |    no     | Sets the option to append a timestamp. If true, timestamp (YYYYMMDD_HHmmSS) is appended on every log file created. Otherwise, a 4-digit running number is appended. Default value is true. |
| appendInstance  | boolean |    no     | Appends a random 4-digit instance ID after every log file. Default is false. |
| fileChunkLength | number  |    no     | Number of seconds to create new file.    |
|  fileChunkTime  | string  |    no     | Time of the day to chunk log file, in HH:MM:SS format. _Note__: No file chunking when fileChunkLength and fileChunkTime are both present._ |
|  enabledEvents  | object  |    no     | Events that are logged. If not set, all are logged. But for W3C, non-stream-related events are ignored. |

As indicated above, the name of the file can be set using a number of options. For example,


    filename = "/var/evostreamms/logs/streams"
    appendTimestamp = true
    appendInstance = true


The log file would be `/var/evostreamms/logs/streams_0237_20140311_183046.txt`.



2\. **RPC (Remote Procedure Calls) Event Sink** – Event details are transmitted to a remote host via HTTP POST. The EMS will ignore any response from the remote host.

RPC sink configuration:

    type="RPC",
    url="http://192.168.1.5:5555/something/service",
    serializerType="JSON",
    customData="my custom data"

The `url` field specifies the destination which will be accepting the HTTP POST event notifications..

The `serializer` type can be one of the following formats:

- **JSON**

Format of JSON POST:

    {"payload":{"creationTimestamp":1349335053486.4370,"name":"","queryTimestamp":1349335053487.4370,"type":"NR","uniqueId":1,"upTime":1.0000},"type":"streamCreated"}


- **XML**

Format of XML POST:

``` 
<?xml version="1.0" ?>
<MAP isArray="false" name="">
    <MAP isArray="false" name="payload">
        <DOUBLE name="creationTimestamp">1349335287346.813</DOUBLE>
        <STR name="name"></STR>
        <DOUBLE name="queryTimestamp">1349335287346.813</DOUBLE>
        <STR name="type">NR</STR>
        <UINT64 name="uniqueId">1</UINT64>
        <DOUBLE name="upTime">0.000</DOUBLE>
    </MAP>
<STR name="type">streamCreated</STR>
</MAP>
```

- **XMLRPC**

Format of XMLRPC POST: (indented for clarity)

``` 
<?xml version="1.0"?>
<methodCall>
    <methodName>event.Log</methodName>
    <params>
        <param>
            <value>
                <struct>
                    <member>
                        <name>payload</name>
                        <value>
                            <struct>
                            <member>
                                <name>creationTimestamp</name>
                                <value><double>0.000000</double></value>
                            </member>
                            <!-- contents removed for clarity -->
                            </struct>
                        </value>
                    </member>
                    <member>
                        <name>type</name>
                        <value><string>streamCreated</string></value>
                    </member>
                </struct>
            </value>
        </param>
    </params>
</methodCall>
```

The `customData` parameter for both File and RPC Event Sinks can be _optionally_ used to extra data to each event for that sink. This could be used to identify the particular EMS instance which is generating the event, return a particular ID or Key which is pertinent to your handling of the event, or anything really! A customData parameter can be a simple sting value or a complex LUA object.

If a `customData` parameter is not specified for a node, the value of the parent eventLogger customData node will be used. If that is also not specified, the value will be V_NULL.




### Transcoder

Within the application section you can find the configuration for the EvoStream Transcoder. The default settings are generally going to be fine for all applications, but under certain circumstances they may need to be adjusted. The transcoder section looks like the following:


    transcoder = {
        scriptPath="..\\emsTranscoder.bat",
        srcUriPrefix="rtsp://localhost:5544/",
        dstUriPrefix="-f flv tcp://localhost:6666/"
    },

The `srcUriPrefix` tells the transcoder how to get the stream from the EMS. The `dstUriPrefix` tells the transcoder how to push the stream back to the EMS. The ports used in these two values must match the acceptors the EMS is actively listening on. By default this is i`5544` for RTSP and `6666` for liveFLV.

**Transcoder Structure Table:**

|     Key      |  Type  | Mandatory | Description                              |
| :----------: | :----: | :-------: | ---------------------------------------- |
|  scriptPath  | string |    yes    | The location for the helper script for the transcoder. The transcode API function calls this script instead of calling the binary directly so that the binary can be replaced should you want to use a custom transcoder. |
| srcUriPrefix | String |    Yes    | When using the transcode API function, you can specify just a localStreamName as the source stream. This is the prefix that will be pre-pended to the provided localStreamName when actually pulling that source stream. For example, if `srcUriPrefix="rtsp://localhost:5544"` and the stream name `"test1"` is given to the transcode command, the following URI will be used: `rtsp://localhost:5544/test1` |
| dstUriPrefix | String |    Yes    | This is the converse of the srcUriPrefix, in that if just a localStreamName is given as a destination in the transcode command, this is the string that will be prepended to the stream name. That complete command will then be used by the transcoder to send the stream back to the EMS. |




### drm

Also in the application section of the config.lua file, the DRM section provides the configuration values for any DRM that needs to be activated. This section is commented out by default (wrapped in "--[[" and "]]--"). It must be un-commented-out before DRM will be activated.

``` 
drm={
    type="verimatrix",
    requestTimer=1,
    reserveKeys=10,
    reserveIds=10,
    -- urlPrefix="http://server1.evostream1.com:12684/CAB/keyfile"
    urlPrefix="http://vcas3multicas1.verimatrix.com:12684/CAB/keyfile"
},
```

**drm Structure Table:**

|     Key      |  Type  |         Mandatory         | Description                              |
| :----------: | :----: | :-----------------------: | ---------------------------------------- |
|     type     | string |            yes            | The type of DRM to be used. Options are: "verimatrix" – Enables Verimatrix DRM on HLS "evo" – Enables AES encryption on HLS"none" – disables DRM. This is the same as commenting out this section of the config file. |
| requestTimer | Number | Yes, when type=verimatrix | The key request timer period in seconds. Right after startup, the EMS will request keys from the Verimatrix Key Server every timer period.Default=1, Min=1, Max=none. (If set below min, the min value will be used.) |
| reserveKeys  | Number | Yes, when type=verimatrix | The number of keys buffered per ID.Default=10, Min=5, Max=none. (If set below min, the min value will be used.) |
|  reserveIds  | Number | Yes, when type=verimatrix | The number of reserve IDs with key buffers to be filled in addition to active IDs.Default=10, Min=5, Max=none. (If set below min, the min value will be used.) |
|  urlPrefix   |        | Yes, when type=verimatrix | The location of your Verimatrix VCAS Key Server |
