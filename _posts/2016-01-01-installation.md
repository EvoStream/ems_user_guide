---
title: Installation and Startup
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: installation
---

## Obtain a License

A license file is required to run EMS. EvoStream offers 30-day trial licenses which can be obtained from the EvoStream website: [https://evostream.com/free-trial/](https://evostream.com/free-trial/)

The EMS itself can be downloaded here: [https://evostream.com/support/software-downloads/](https://evostream.com/support/software-downloads/).

Licenses can be purchased from EvoStream directly, contact [sales@evostream.com](mailto:sales@evostream.com), or via the EvoStream Website: [https://evostream.com/pricing/](https://evostream.com/pricing/).





## Install EMS

### A. Installation for Linux (Package)

EvoStream provides standard Linux installers for the EMS. The Linux EMS installer will install the following packages and software:

- EvoStream Media Server (EMS)
- EvoStream Web Server (EWS)

To use the Linux installers, you must follow these instructions.

**Pre-requisites:**

Administrative privileges are required. This can be accomplished in many ways.

If the sudo utility is available:

``` 
$ su –
```

If the sudo utility is not available:

``` 
$ sudo su –
```

**Note:**

The prompt changes from `$` to `#` when administrative privileges are enabled.



**Installation Procedure:**

1\. Retrieve the script used to install the EvoStream software repository and store it

- Debian based Linux distributions (Ubuntu or Debian)
  
  ``` 
  # wget http://apt.evostream.com/installkeys.sh -O /tmp/installkeys.sh
  ```
  
- RedHat based Linux distributions (CentOS, Fedora, RHEL)
  
  ``` 
  # curl http://yum.evostream.com/installkeys.sh -o /tmp/installkeys.sh
  ```

2\. Execute the script to install the EvoStream software repository and keys

``` 
# sh /tmp/installkeys.sh
```

- If successful, the following message should be printed on the console:
  
  ``` 
  "EvoStream keys installed successfully"
  ```

At this stage, the EvoStream software repository and keys are successfully installed and you can install packages from it.

**Note:** Steps 1 and 2 above must be executed only once.

The following steps are used to install the EvoStream Media Server, and can be repeated to update the EMS to the most recent release.

3\. Install EvoStream Media Server.

- Debian based Linux distributions (Ubuntu or Debian)
  
  ``` 
   # apt-get install evostream-mediaserver
  ```
  
- RedHat based Linux distributions (CentOS, Fedora, RHEL)
  
  ``` 
   # yum install evostream-mediaserver
  ```

4\. Install the license file. Copy the License.lic file into `/etc/evostream` folder

``` 
# cp /path/to/License.lic /etc/evostream/License.lic
```

5\. Run EvoStream Media Server

- To start EMS in console mode
  
  ``` 
  # service evostreamms start_console
  ```
  
- To start EMS as a daemon background process
  
  ``` 
  # service evostreamms start
  ```
  
- To restart EMS (will restart as daemon)
  
  ``` 
  # service evostreamms restart
  ```
  
- To stop EMS
  
  ``` 
  # service evostreamms stop
  ```



### B. Installation for Linux (Archive)

You can install the EMS from a simple archive file (.tar.gz). The latest EMS Release can be found on the EvoStream website: [https://evostream.com/software-downloads/](https://evostream.com/software-downloads/).

You will need to choose the most appropriate distribution for the Operating System that you are using. Once you have downloaded your distribution:

Simply extract the EMS package. The location of the installation is not important. However, for security reasons, the EvoStream Media Server should **not** be installed into the web-root of the target computer (if one exists).



### C. Installation for Windows®

The latest EMS Release can be found on the EvoStream website: [https://evostream.com/software-downloads/](https://evostream.com/software-downloads/). Choose the most appropriate distribution for the Operating System that you are using. Once you have downloaded your distribution, simply extract the EMS package to a temporary folder.

Run **setup.exe** to install EMS.

*See the Quick Start Guide document for the detailed installation instructions.*



### D. Other Installers

If you cannot find a suitable distribution, please contact us at [sales@evostream.com](mailto:sales@evostream.com), and we can possibly provide a custom compilation for the Operating System of your choice.





## Platform Verification

If you are unsure if the distribution you downloaded is appropriate for your Operating System, you can use the `platformTests` program. This program is available with all distributions and provides a suite of platform compatibility tests. It can be found in the bin directory.

On all systems, open a console or terminal (command prompt) and run the `platformTests` executable (`./platformTests`). It will print out the results of the platform compatibility tests. If the test succeeds, then you have an appropriate distribution!





## Linux Limitations

Linux systems place limits on the number of sockets and file descriptors a process may use. This will apply to the EMS as well. If you plan on using more than 1024 connections at one time for your server, you will need to modify the following configuration file: `/etc/security/limits.conf`

The following lines will need to be added/modified:

``` 
soft nofile 16384
hard nofile 65536
soft nproc 4096
hard nproc 16384
```

------



## Distribution Content

### A. Linux Package

``` 
/
├── etc
│   └── evostreamms
│       ├── blacklist.txt
│       ├── config.lua
│       ├── server.cert
│       ├── server.key
│       ├── users.lua
│       ├── webconfig.lua
│       └── whitelist.txt
├── usr
│   ├── bin
│   │   ├── evo-phpengine
│   │   │   └── php.cgi
│   │   ├── evo-avconv
│   │   ├── evo-mp4writer
│   │   ├── evostreamms
│   │   └── evo-webserver
│   └── share
│       ├── evo-avconv
│       │   └── presets
│       │       └── [30 transcode preset files]
│       └── doc
│           └── evostreamms
│               ├── API Definition.pdf
│               ├── copyright
│               ├── EMS How Tos.pdf
│               ├── EMS User Guide.pdf
│               ├── EvoStream Media Server EULA v2.pdf
│               ├── Quick_Start_Guide.txt
│               └── version
│                   ├── BUILD_DATE
│                   ├── BUILD_NUMBER
│                   ├── CODE_NAME
│                   ├── OS_NAME
│                   ├── OS_VERSION
│                   └── RELEASE_NUMBER
└── var
    ├── evostreamms
    │   ├── media
    │   └── xml
    │       ├── auth.xml
    │       ├── bandwidthlimits.xml
    │       ├── connlimits.xml
    │       ├── ingestpoints.xml
    │       └── pushPullSetup.xml
    ├── evo-webroot
    │   ├── demo
    │   │   ├── css
    │   │   ├── evo.png
    │   │   ├── evowrtcclient.html
    │   │   ├── evowsvideo.html
    │   │   ├── jsonMetaTest.html
    │   │   ├── jsonMetaWriteTest.html
    │   │   └── loading.gif
    │   ├── EMS_Web_UI
    │   │   ├── css
    │   │   ├── img
    │   │   ├── js
    │   │   ├── php
    │   │   ├── phpacct
    │   │   ├── settings
    │   │   ├── swf
    │   │   ├── evostream_copyright.txt
    │   │   ├── index.php
    │   │   ├── install_license.php
    │   │   ├── license.txt
    │   │   ├── navbar.php
    │   │   ├── README[Enable_Login_Authentication].txt
    │   │   └── style.css
    │   ├── evowebservices
    │   │   ├── config
    │   │   ├── core
    │   │   ├── plugins
    │   │   ├── EMS_Web_Services_User_Guide.pdf
    │   │   ├── evostream_copyright.txt
    │   │   └── evowebservices.php
    │   ├── clientaccesspolicy.xml
    │   └── crossdomaim.xml
    ├── log
    │   └── evostreamms
    └── run
        ├── evostreamms
        └── evostreamms.pid
```



### B. Linux Archive

``` 
./EvoStream Archive
  ├── bin
  │   ├── evo-phpengine
  │   ├── emsTranscoder.sh
  │   ├── evo-avconv
  │   ├── evo-mp4writer
  │   ├── evostreamms
  │   ├── evo-webserver
  │   ├── platformTests
  │   ├── run_console_ems.sh
  │   └── run_daemon_ems.sh
  ├── config
  │   ├── auth.xml
  │   ├── bandwidthlimits.xml
  │   ├── blacklist.txt
  │   ├── config.lua
  │   ├── connlimits.xml
  │   ├── ingestpoints.cml
  │   ├── pushPullSetup.xml
  │   ├── server.cert
  │   ├── server.key
  │   ├── users.lua
  │   ├── webconfig.lua
  │   └── whitelist.txt
  ├── demo
  │   ├── base64.js
  │   └── emsdemo.html
  ├── documents
  │   ├── API Definition.pdf
  │   ├── EMS How Tos.pdf
  │   ├── EMS User Guide.pdf
  │   ├── Evostream Media Server EULA v2.pdf
  │   ├── Quick_Start_Guide.txt
  │   └── ReadMe.txt
  ├── evo-avconv-presets
  │   └── [30 transcode preset files]
  ├── logs
  ├── media
  ├── BUILD_DATE
  ├── README.txt
  └── evo-webroot
      ├── demo
      │   ├── css
      │   ├── evo.png
      │   ├── evowrtcclient.html
      │   ├── evowsvideo.html
      │   ├── jsonMetaTest.html
      │   ├── jsonMetaWriteTest.html
      │   └── loading.gif
      ├── EMS_Web_UI
      │   ├── css
      │   ├── img
      │   ├── js
      │   ├── php
      │   ├── phpacct
      │   ├── settings
      │   ├── swf
      │   ├── evostream_copyright.txt
      │   ├── index.php
      │   ├── install_license.php
      │   ├── license.txt
      │   ├── navbar.php
      │   ├── README[Enable_Login_Authentication].txt
      │   └── style.css
      ├── evowebservices
      │   ├── config
      │   ├── core
      │   ├── plugins
      │   ├── EMS_Web_Services_User_Guide.pdf
      │   ├── evostream_copyright.txt
      │   └── evowebservices.php
      ├── clientaccesspolicy.xml
      └── crossdomain.xml
```



### C. Windows Package

``` 
C:\EvoStream
   ├── config
   │   ├── auth.xml
   │   ├── bandwidthlimits.xml
   │   ├── blacklist.txt
   │   ├── config.lua
   │   ├── connlimits.xml
   │   ├── ingestpoints.cml
   │   ├── pushPullSetup.xml
   │   ├── server.cert
   │   ├── server.key
   │   ├── users.lua
   │   ├── webconfig.lua
   │   └── whitelist.txt
   ├── demo
   │   ├── base64.js
   │   └── emsdemo.html
   ├── documents
   │   ├── API Definition.pdf
   │   ├── EMS How Tos.pdf
   │   ├── EMS User Guide.pdf
   │   ├── Evostream Media Server EULA v2.pdf
   │   ├── Quick_Start_Guide.txt
   │   └── ReadMe.txt
   ├── evo-avconv-presets
   │   └── [30 transcode preset files]
   ├── evo-phpengine
   ├── logs
   ├── media
   ├── services
   ├── emsTranscoder.bat
   ├── evo-avconv.exe
   ├── evo-mp4writer.exe
   ├── evostreamms.exe
   ├── evo-webserver.exe
   ├── libgcrypt-20.dll
   ├── libgmp-10.dll
   ├── libgnutls-28.dll
   ├── libgpg-error6-0.dll
   ├── libhogweed-2-5.dll
   ├── libiconv-2.dll
   ├── libmicrohttpd-10.dll
   ├── libiconv-2.dll
   ├── libnettle-4-7.dll
   ├── run_console_ems.bat
   ├── tests.exe
   ├── unins000.dat
   ├── unins000.exe
   └── evo-webroot
       ├── demo
       │   ├── css
       │   ├── evo.png
       │   ├── evowrtcclient.html
       │   ├── evowsvideo.html
       │   ├── jsonMetaTest.html
       │   ├── jsonMetaWriteTest.html
       │   └── loading.gif
       ├── EMS_Web_UI
       │   ├── css
       │   ├── img
       │   ├── js
       │   ├── php
       │   ├── phpacct
       │   ├── settings
       │   ├── swf
       │   ├── evostream_copyright.txt
       │   ├── index.php
       │   ├── install_license.php
       │   ├── license.txt
       │   ├── navbar.php
       │   ├── README[Enable_Login_Authentication].txt
       │   └── style.css
       ├── evowebservices
       │   ├── config
       │   ├── core
       │   ├── plugins
       │   ├── EMS_Web_Services_User_Guide.pdf
       │   ├── evostream_copyright.txt
       │   └── evowebservices.php
       ├── clientaccesspolicy.xml
       └── crossdomain.xml
```

------



## File Descriptions

### A. Command Files

| File                | Description                              |
| ------------------- | ---------------------------------------- |
| evostreamms(.exe)   | The EvoStream binary itself              |
| evo-webserver(.exe) | The EvoStream Web Server (EWS) binary. This handles the serving of all files over HTTP |
| evo-mp4writer(.exe) | This binary application combines the various pieces of each MP4 recording once the file is ready to be finalized |
| evo-avconv(.exe)    | The EvoStream Transcoder binary. This gets called in reaction to the `transcode` API command |
| run_daemon_ems.sh   | Shell script used on **Linux/Unix** environments to start the EMS as a background application. Use this for production deployments. It requires that the user "evostream" exists, the script will not work without it. Please feel free to modify this script to use a different user. When using the daemon script, to validate that the server is running, you can issue the command `ps –ef` |
| run_console_ems.bat | Batch file/script on **Windows** which runs the EMS as a console application. This is useful for new users as it provides instant feedback on the console when commands are entered and shows errors if they occur in new streams. |
| create.bat          | Script to create a **Windows** service for the EMS. This will also start the EMS as a background process. This must be run with Administrative privileges as it writes to the Windows Registry. _This only needs to be run once._ |
| remove.bat          | Script to remove the **Windows** service for the EMS and remove the relevant Windows Registry entries. This must be run with Administrative privileges. |
| start.bat           | Script to start the EMS **Windows** service. This script will not work if create.bat has not been run first. |
| stop.bat            | Script to stop the EMS **Windows** Service. |
| Srvany.exe          | This is a binary provided by Microsoft and is used to create the Windows Service. |



### B. Configuration Files

| File                | Description                              |
| ------------------- | ---------------------------------------- |
| config.lua          | The main configuration file used by the EMS. The contents of this file are detailed later in this document. |
| webconfig.lua       | The EvoStream Web Server (EWS) configuration file. The contents of this file are detailed later in this document. |
| users.lua           | Defines the valid authentication the server will require when streams are pushed into the EMS. |
| pushPullSetup.xml   | This file is used by the EMS to store stream action commands that are made through the Runtime API. This file may not be modified. At startup, if the EMS detects that the file has been modified it will rename the file and start with a blank/fresh copy. |
| connlimits.xml      | Defines the maximum number of concurrent connections you want the EMS to accept |
| bandwidthlimits.xml | Defines the maximum amount of bandwidth you want the server to be able to use (set the instantaneous bandwidth cap). |



### C. Documentation

| File                               | Description                              |
| ---------------------------------- | ---------------------------------------- |
| EMS User Guide.pdf                 | This Document                            |
| API Definition.pdf                 | Provides descriptions for all of the EMS's Runtime APIs and Event Notifications |
| EMS How Tos.pdf                    | Provides example commands for performing basic tasks with the EMS |
| EvoStream Media Server EULA v2.pdf | The End User License Agreement for the EMS |



### D. Miscellaneous

| File                            | Description                              |
| ------------------------------- | ---------------------------------------- |
| demo/emsdemo.htmldemo/base64.js | The emsdemo.html file can be opened directly in a web browser and provides some example commands which can be sent to the EMS. |
| media/                          | The media directory is the default location for video-on-demand files. This is where the EMS will look when VOD requests are made. This default location can be changed in the EMS main configuration file, which is typically `config/config.lua` |
| logs/                           | This is the directory that EMS will write its logs to. This default location can be changed in the EMS main configuration file, which is typically `config/config.lua` |
| License.lic                     | This is the license file required to run the EMS. It can be placed in the `config/` , `bin/` , or `/etc/evostreamms/` folders, or in whatever folder the evostreamms binary resides. |