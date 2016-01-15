---
title: 
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: startingserver
---

## Starting the Server

### Linux Distributions (Linux apt/yum Installer)

Running the EMS after installation is as simple as starting the EMS service:

    # service evostreamms start
    # service evostreamms stop
    # service evostreamms restart

The EMS can also be run in console mode:

    # service evostreamms start_console

### Linux, Mac OSX and BSD Distributions (.tar.gz Distribution)

There are two "run" scripts that can be used to start the EvoStream Media Server:

1. Run EMS with console logs, using `config/config.lua` as the main server configuration.

        $ ./run_console_ems.sh

2. Run EMS as a background process. The script will attempt to assign the run-process to the user `evostream`.

        $ ./run_daemon_ems.sh

**Notes:**

- Either command can be directly executed.
- For `run_daemon_ems.sh`, if the `evostream` user does not exist, an error will be printed to the screen. Despite the error, the EMS will probably have been started. To check if the server is running, user can issue `ps –ef | grep evostream` in terminal.

This command will print differently on different operating systems, but it should let you know that the server is running.

- The **user** used by `run_daemon_ems.sh` can easily be modified by changing the value after the `-u` in the script itself.
- The user running the EvoStream Media Server must have sufficient permission to open and bind to network ports.

### Windows Distribution

The EMS may be started and stopped using the **Windows Services** tool in Windows.

User may directly run the EMS using the shortcut icon if added during installation, or the batch file for running the server in a command prompt:

    C:\EvoStream\> run_console_ems.bat

This script simply runs the Media Server through the command prompt, using `config/config.lua` as the main server configuration. This file can also be double-clicked to start the server. The EvoStream Media Server icon on the desktop can also be double-clicked to do the same thing.

There are other scripts that can be used to create and manipulate the server as a Windows® Service. These scripts need to be run as an administrator. User can verify they have worked by opening the Windows Services tool and looking for the EMS service.

- `C:\EvoStream\services\ems\create.bat` : Creates and starts the Windows service
- `C:\EvoStream\services\ems\remove.bat` : Removes the Windows service
- `C:\EvoStream\services\ems\start.bat` : Starts the service if it has not already been started
- `C:\EvoStream\services\ems\stop.bat` : Stops the service if it is currently running

### Startup Success

For either Windows or Linux/BSD/OSX, when you run the EMS as a console application, you should see the following screen indicating the server is up and running:

![](data:image/png;base64,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)

### EMS Command Line Definition

The evostreamms executable can be run with a few different options. The command line signature is as follows:

**Format:** evostreamms [OPTIONS] [config\_file\_path]

| Command | Function |
| --- | --- |
| --help | Prints this help and exit. |
| --version | Prints the version and exit. |
| --use-implicit-console-appender | Adds extra logging at runtime, but is only effective when the server is started as a console application. This is particularly useful when the server starts and stops immediately for an unknown reason. It will allow you users to see if something is wrong, particularly with the config file. |
| --daemon | Overrides the daemon setting inside the config file and forces the server to start in daemon mode. |
| --uid=<uid> | Run the process with the specified user id. |
| --gid=<gid> | Run the process with the specified group id. |
| --pid=<pid\_file> | Create PID file. Works only if --daemon option is specified. |


