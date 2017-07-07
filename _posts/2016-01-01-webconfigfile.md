---
title: Web Server Configuration File
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: webconfigfile
---


## webconfig.lua

This file contains the EvoStream Web Server (EWS) configuration. The locations of various web server files/folders can be changed here. Various web server settings such as HTTP port, group name aliases, mime types, etc. can be modified here also.



**Contents**

- configuration – This is the entire structure for all configuration needed by the EWS Server.
  
  ``` 
    configuration =
    {
        logAppenders
        {
            -- content removed for clarity
        },
        applications =
        {
            -- content removed for clarity
        }
    }
  ```

**webServer Configuration Structure Table:**

|     Key      |  Type  | Mandatory | Description                              |
| :----------: | :----: | :-------: | ---------------------------------------- |
| logAppenders | object |    yes    | Will hold a collection of log appenders. Each log message will be sent to each of the log appenders enumerated in this configuration section. |
| applications | object |    yes    | Will hold a collection of loaded applications. Besides that, it will also hold some other values. |

When the web server starts, the following sequence of operations is performed:

- The `logAppenders` value is read. This is where all log appenders are configured and brought up to running state. Depending on the collection of your log appenders, you may (not) see further log messages.
- The `applications` valueis taken into consideration. After this stage completes, all the applications are fully functional and the server is online and ready to do stuff.





### logAppenders

``` 
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
        fileName="../logs/evo-webserver",
        newLineCharacters="\n",
        fileHistorySize=100,
        fileLength=1024*1024,
        singleLine=true,
    }
}
```

This section contains a list of log appenders. The entire collection of appenders listed in this section is loaded inside the logger at config-time. All log messages will be than passed to all these log appenders. Depending on the log level, an appender may (or may not) log the message. "Logging" a message means "saving" it on the specified "media" (in the example below we have a console appender and a file).

**webServer logAppenders Structure Table:**

|        Key        |  Type   | Mandatory | Description                              |
| :---------------: | :-----: | :-------: | ---------------------------------------- |
|       name        | string  |    yes    | The name of the appender. It is usually used inside pretty print routines. |
|       type        | string  |    yes    | The type of the appender. It can be "console", "coloredConsole" or "file". Types "console" and "coloredConsole" will output to the console. The difference between them is that "coloredConsole" will also apply a color to the message, depending on the log level. Quite useful when eye-balling the console. Type "file" log appender will output everything to the specified file. |
|       level       | number  |    yes    | The log level used. The values are presented just below. Any message having a log level less or equal to this value will be logged. The rest are discarded. (**Log levels:** 0 FATAL, 1 ERROR, 2 WARNING, 3 INFO, 4 DEBUG, 5 FINE, 6 FINEST, -1 disable logs) |
|     fileName      | string  |    yes    | If the type of appender is a file, this will contain the path of the file. |
| newLineCharacters | string  |    no     | Newline character used in the file appender. |
|  fileHistorySize  | number  |    no     | The maximum number of log files to be retained. The oldest log file will be deleted first if this number is exceeded. |
|    fileLength     | number  |    no     | Buffer size of the file appender.        |
|    singleLine     | boolean |    no     | If yes, multi-line log messages are merged into one line. |





### applications

``` 
applications =
{
    rootDirectory = "./",
    {
        name="webserver",
        -- settings for application
        -- content removed for clarity
    }
}
```

This section is where all the applications inside the server are placed. It holds the attributes of each application that the server will use to launch them. Each application may have specific attributes that it requires to execute its own functionality.

**webServer application Structure Table**

|      Key      |  Type  | Mandatory | Description                              |
| :-----------: | :----: | :-------: | ---------------------------------------- |
| rootDirectory | string |   True    | The folder containing applications subfolders. If this path begins with a "/" or "\" (depending on the OS), then is treated as an absolute path. Otherwise is treated as a path relative to the run-time directory (the place where you started the server). |

Following the rootDirectory, there is webserver application. This application has its properties contained in an object. See details below.





### webServer Application

This is where the settings of the webserver application are defined.

    applications=
    {
        rootDirectory="./",
        {
            name="webserver",
            description="Built-In Web Server",
            port=8888,
            emsPort=1113 --should match config.lua's inboundBinVariant acceptor
            bindToIP="",
            sslMode="disabled", -- always, auto, disabled
            maxMemSizePerConnection=32*1024, --32*1024,
            maxConcurrentConnections=5000,
            connectionTimeout=0, -- 0 - no timeout
            maxConcurrentConnectionsSameIP=1000,
            threadPoolSize=8,
            useIPV6=false,
            enableIPFilter=false, --if true, reads white and black lists
            whitelistFile="..\\config\\whitelist.txt",
            blacklistFile="..\\config\\blacklist.txt",
            sslKeyFile="..\\config\\server.key",
            sslCertFile="..\\config\\server.cert",
            enableCache=false,
            cacheSize=1*1024*1024*1024, --1GB
            hasGroupNameAliases=false,
            webRootFolder="..\\evo-webroot",
            enableRangeRequests=true,
            mediaFileDownloadTimeout=30,
            supportedMimeTypes=
            {
                --content removed for clarity
            }
        }

**webServer application Structure Table**

|              Key               |  Type   | Mandatory | Description                              |
| :----------------------------: | :-----: | :-------: | ---------------------------------------- |
|              name              | string  |    Yes    | Name of the web server application.      |
|          description           | string  |    No     | Describes the web server application.    |
|              port              | number  |    Yes    | The web server listens to this port.     |
|            emsPort             | number  |    Yes    | Should match inboundBinVariant acceptor in config.lua. |
|            bindToIP            | string  |    No     | The specific IP to use when the host has multiple Ethernet cards. |
|            sslMode             | string  |    Yes    | Allowed values are "always", "auto" and "disabled". "always" forces HTTPS. "auto" checks for HTTPS first, falls back to HTTPS otherwise. "disabled" uses HTTP. |
|    maxMemSizePerConnection     | number  |    No     | Allowable maximum bytes for transmission. |
|    maxConcurrentConnections    | number  |    No     | Allowable simultaneous connections.      |
|       connectionTimeout        | number  |    No     | The number of seconds before a pending request times out. This applies if the value is greater than 0. If value is 0 then there is no timeout. |
| maxConcurrentConnectionsSameIP | number  |    No     | Allowable simultaneous connections per IP. |
|         threadPoolSize         | number  |    No     | The number of threads handling the requests. It is suggested that it should be 2 times the number of physical processors. |
|            useIPV6             | boolean |    No     | Use IP v6 (true) or IP v4 (false).       |
|         enableIPFilter         | boolean |    No     | If true, reads white and black lists.    |
|         whitelistFile          | string  |    No     | Contains a list of allowed IPs. Uses new line delimiter. |
|         blacklistFile          | string  |    No     | Contains a list of blocked IPs. Uses new line delimiter. |
|           sslKeyFile           | string  |    No     | Key file used when using HTTPS.          |
|          sslCertFile           | string  |    No     | Cert file used when using HTTPS.         |
|          enableCache           | boolean |    No     | Enables internal caching of static files. |
|           cacheSize            | number  |    No     | Size of cache.                           |
|      hasGroupNameAliases       | boolean |    no     | Protects HTTP streaming variants (HLS, HDS, MSS, DASH, media files) from direct access |
|         webRootFolder          | string  |    Yes    | The web root folder.                     |
|      enableRangeRequests       | boolean |    No     | Enables range requests support (HTTP 206 Partial-Content) |
|    mediaFileDownloadTimeout    | number  |    Yes    | A media file download session is ended when there is no subsequent request after X seconds |
|     includeResponseHeaders     | object  |    No     | Additional headers to be included in the response |





### supportedMimeTypes
```
    supportedMimeTypes=
    {
        -- non-streaming
        {
            extensions="mp4,mp4v,mpg4",
            mimeType="video/mp4",
            streamType="",
            isManifest=false,
        },
        -- content removed for clarity
        -- streaming
        {
            extensions="m3u,m3u8",
            mimeType="audio/x-mpegurl",
            streamType="hls",
            isManifest=true,
        },
        -- content removed for clarity
    }
```
This section is used to indicate file extension associations to mime types.

**webServer supportedMime Types Structure Table:**

|    Key     |  Type   | Mandatory | Description                              |
| :--------: | :-----: | :-------: | ---------------------------------------- |
| extensions | string  |    yes    | File extensions to be associated.        |
|  mimeType  | string  |    yes    | The mime type associated with the specified file extensions. |
| streamType | string  |    no     | The type of HTTP stream.                 |
| isManifest | boolean |    no     | Indicates if a file is a manifest used with the HTTP streaming variant. |





### includeResponseHeaders
```
    includeResponseHeaders=
    {
        {
            header="Access-Control-Allow-Origin",
            content="*",
            override=true,
        },
        {
            header="User-Agent",
            content="Evostream",
            override=false,
        },
```
This section indicates additional headers to be included in the response.

**webServer includeResponseHeaders Structure Table:**

|   Key    |  Type   | Mandatory | Description                              |
| :------: | :-----: | :-------: | ---------------------------------------- |
|  header  | string  |    yes    | The response header.                     |
| content  | string  |    yes    | The value particular to the response header. |
| override | boolean |    No     | Indicates if the header should be overridden if the existing header has this already included. |





### apiProxy

Proxy authentication provides a way to secure the HTTP based EMS API. All API commands will first pass through the EWS, which will validate the provided username and password, and then pass the commands to the EMS for processing. API command return values will be routed back to the caller appropriately.

    apiProxy=
    {
        authentication="basic", -- none, basic
        pseudoDomain="<domain>",
        address="127.0.0.1",
        port=7777,
        userName="<username>",
        password="<password>",
    }

To enable Proxy Authentication you will open the _webconfig.lua_ config file and uncomment the "apiProxy" section near the bottom of the file.

**webServer apiProxy Structure Table:**

|      Key       |  Type  | Mandatory | Description                              |
| :------------: | :----: | :-------: | ---------------------------------------- |
| authentication | object |    yes    | The type of authentication. Currently, there are only 2 available values: "basic" which is basic HTTP authentication that uses a username and password; and "none" which disables authentication. |
|  pseudoDomain  | object |    yes    | The domain name or folder                |
|    address     | number |    yes    | The address using the inboundHTTPJsonCLI |
|      port      | number |    yes    | Port, referring to the config.lua's acceptors for inboundHTTPJsonCLI |
|    userName    | string |    No     | Basic authentication username            |
|    password    | string |    No     | Password for the userName                |

Once enabled, new API calls using Proxy Authentication will be formatted as follows:

    For EMS 1.7 series:
    http://userName:password@EWS_IP:EWS_PORT/pseudoDomain/command%3fparams=…

    For EMS 2.0 series:
    http://userName:password@EWS_IP:EWS_PORT/pseudoDomain/command?params=…

Note: The EWS_PORT above is defined in webconfig.lua under applications > rootDirectory > port.

Here's an example with parameters:

    For EMS 1.7 series:
    http://user1:pass1@localhost:8888/apiproxy/pullstream%3fparams=dXJpPXJ0bXA6Ly9zdHJlYW1pbmcuY2l0eW9mYm9zdG9uLmdvdi9saXZlL2NhYmxlIGxvY2Fsc3RyZWFtbmFtZT1zdHJlYW0x

    For EMS 2.0 series:
    http://user1:pass1@localhost:8888/apiproxy/pullstream?params=dXJpPXJ0bXA6Ly9zdHJlYW1pbmcuY2l0eW9mYm9zdG9uLmdvdi9saXZlL2NhYmxlIGxvY2Fsc3RyZWFtbmFtZT1zdHJlYW0x

Here's an example without parameters:

    http://user1:pass1@localhost:8888/apiproxy/version





### authentication
The authentication settings for the EMS Web UI. This is disabled for non-Amazon EMS packages. 

    auth=
    {
      {
        domain="EMS_Web_UI", --the domain folder
        digestFile="../evo-webroot/EMS_Web_UI/settings/passwords/.htdigest", --relative path to digest file
        enable=false,
      },
    },

To enable the EMS Web UI Authentication you will open the webconfig.lua config file and change "enable" value to "true".

|      Key       |  Type  | Mandatory | Description                              |
| :------------: | :----: | :-------: | ---------------------------------------- |
| domain | object |    yes    | The domain name or folder |
|  digestFile  | object |    yes    | The relative path to digest file |
|    enable     | boolean |    yes    | Tells if the authentication is enabled or disabled |

If enabled, the Authentication window will open if the EMS Web UI is accessed.
See http://docs.evostream.com/ems_web_ui_user_guide/authentication for more details.

------

## pushPullSetup.xml

This file is used when reconnecting to the stream after restarting the EMS server and is automatically updated when a stream is created or deleted. If the file does not exist (or when it's deleted), it will be generated automatically by EMS.

    <?xml version="1.0" ?>
    <MAP isArray="false" name="">
        <STR name="checksum">9d4782e9efeab7bd51c6f64ffcb83af3</STR>
        <MAP isArray="true" name="dash" />
        <MAP isArray="true" name="hds" />
        <MAP isArray="true" name="hls" />
        <MAP isArray="true" name="mss" />
        <MAP isArray="true" name="process" />
        <MAP isArray="true" name="pull" />
        <MAP isArray="true" name="push" />
        <MAP isArray="true" name="record" />
        <MAP isArray="false" name="serverVersion">
            <STR name="banner">EvoStream Media Server (www.evostream.com) version 1.7.0 build 4260 with hash: f72e39b26867edaeb15744390e53d2d87c34acea on branch: origin/release/1.7.0 - PacMan|m| - (built for Windows-8.1-x86_64 on 2015-11-25T07:51:34.000)</STR>
            <STR name="branchName">origin/release/1.7.0</STR>
            <STR name="buildDate">2015-11-25T07:51:34.000</STR>
            <STR name="buildNumber">4260</STR>
            <STR name="codeName">PacMan|m|</STR>
            <STR name="hash">f72e39b26867edaeb15744390e53d2d87c34acea</STR>
            <STR name="releaseNumber">1.7.0</STR>
        </MAP>
        <UINT32 name="version">26</UINT32>
        <MAP isArray="true" name="webrtc" />
        </MAP>







## connLimits.xml

This file sets the allowed maximum number of connections to EMS.

    <?xml version="1.0" ?>
    <UINT32 name="">0</UINT32>







## users.lua (Authentication)

users.lua contains the user names and passwords to be used in authentication.

``` 
users=
{
    user1="password1",
    user2="password2",
}
realms=
{
    {
        name="EVOSTREAM stream router",
        method="Digest",
        users={
            "user1",
            "user2",
        },
    },
}
```







## auth.xml

The configuration for the authentication. If true, the authentication declared in users.lua will be read before the streaming starts.

    <?xml version="1.0" ?>
    <BOOL name="">true</BOOL>







## bandwidths.xml

``` 
<?xml version="1.0" ?>
<MAP isArray="false" name="">
    <DOUBLE name="in">0.000</DOUBLE>
    <DOUBLE name="out">0.000</DOUBLE>
</MAP>
```

If `enableCheckBandwidth` in config.lua is true, automatically EMS will read the bandwidths.xml file. EMS will limit all the incoming and outgoing stream dependent to the configured bandwidth range.







## blacklist.txt

The EWS can allow or disallow access to files based upon defined white lists or black lists. If a blacklist is specified, access will only be granted to an IP if that IP address does **not** appear on the blacklist.

**Note:**

- `enableIPFilter`  should be set to `true` to be able to read the blacklist file.
  
- `blacklistFile` should not be commented to be able to honor the list of blacklisted IP address
  
- If IP address is both on whitelist and blacklist file, EMS will treat the IP address as blacklisted
  
  ``` 
    enableIPFilter=true,
    whitelistFile="..\\config\\whitelist.txt",
    blacklistFile="..\\config\\blacklist.txt",
  ```






## whitelist.txt

The EWS can allow or disallow access to files based upon defined white lists or black lists. If a whitelist is specified, access will only be granted when the HTTP request originates from an IP on the whitelist.

**Note:**

- `enableIPFilter`  should be set to `true` to be able to read the whitelist file.
  
- `whitelistFile` should not be commented to be able to honor the list of blacklisted IP address
  
- If IP address is both on whitelist and blacklist file, EMS will treat the IP address as blacklisted
  
  ``` 
    enableIPFilter=true,
    whitelistFile="..\\config\\whitelist.txt",
    blacklistFile="..\\config\\blacklist.txt",
  ```






## ingestpoints.xml

``` 
<?xml version="1.0" ?>
    <MAP isArray="false" name="">
</MAP>
```






## server.cert

    -----BEGIN CERTIFICATE-----
    MIICXAIBAAKBgQCwVGvra2hX2utJnriY89Wq0bsUrotH6wFlIoXbP7u5EEwKiqet
    KCpVM/N34MI3wiLXbbRQUmFELtLhzhp6NFZz1PIQgl67bYiYUJ1MHcbEeZMLVely
    :
    VQQGEwJVUzETMBEGA1UECAwKQ2FsaWZvcm5pYTESMBAGA1UEBwwJU2FuIERpZWdv
    sjiyBNWZUq1pE3x0RnTpUA==
    -----END CERTIFICATE-----






## server.key

    -----BEGIN RSA PRIVATE KEY-----
    MIIDDDCCAnWgAwIBAgIJAOh9kCLgEMuhMA0GCSqGSIb3DQEBBQUAMIGeMQswCQYD
    OhB70/IVC3pfS8eq9KkCQDr4ATT8i8IQyJGerJ47mx2/LhL1ZwqykqBQFW8Xyni7
    :
    vZxkUbeVxJtfdoS0OIHf+xiugYBY33G3odSL7ZISkHT5VeDbXtBJ2kaYcMXUTlh3
    GVOnuh7pX19wgj2VZv2Mz4HvKggPvXlS/WKtPFYsqsw=
    -----END RSA PRIVATE KEY-----


Note:
Scripts are available for creating certificates and keys for EMS. Please refer to our GitHub files [here](https://github.com/EvoStream/evostream_addons/tree/master/certificates_and_keys) for details.
