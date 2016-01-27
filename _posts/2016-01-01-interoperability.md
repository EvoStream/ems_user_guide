---
title: Interoperability
layout: post
date:   2016-01-01 00:00:00 +0000
categories: jekyll update
permalink: interoperability
---

## Stream Sources

    Flash Media Live Encoder (FMLE) – RTSP, RTMP, MPEG-TS
    Flash Media Server (FMS) – RTSP, RTMP, MPEG-TS
    Discover Video Multimedia Encoder (DVME) – RTSP, RTMP, MPEG-TS
    VLC – RTSP, RTMP, Mpeg-TS
    Wowza – RTSP, RTMP, Mpeg-TS
    FFMpeg – MPEG-TS, RTSP
    BRIA SIP Server – RTSP
    IPCamera – RTSP
    Wirecast – RTMP

## Stream Players

    RTMP (Flash) – Adobe Flash Player, JW Player, ffPlay, Flowplayer
    RTSP – Android phones (v2.3.5 or later),VLC, QuickTime, ffPlay
    MPEG-TS – VLC, ffPlay
    HLS – All iOS devices, iPhone, iPad, iPod Touch, JW Player
    HDS – OSMF
    MSS – SilverLight
    DASH – GPAC, Digital Primates, castLabs DASHas

## Akamai

Akamai requires very specific settings when pushing a stream to your account. The `pushStream` command for pushing to Akamai must look like the following:

    pushStream uri=rtmp://AkamaiUserName:AkamaiPass@YOUR.akamaientrypoint.net/EntryPoint localStreamname=YourLocalStream targetStreamName=XX_YY_ZZ@WW emulateUserAgent=FMLE/3.0\ (compatible;\ FMSc/1.0)

AkamaiuserName, AkamaiPass, `YOUR.akamaientrypoint.net` all must be the values assigned to you by Akamai.

For the `targetStreamName`, xx, yy, zz are arbitrary strings, but Akamai requires there to be exactly two "_" in the stream name. @ww is a unique number used in combination with username/password to allow/disallow the publish operation. It is mandatory and is provided to you by Akamai.

The EMS can also push to the new RTMP HD publishing points in Akamai. You need to set the parameter `sendChunkSizeRequest` to `0` for Akamai to accept the connection. The `pushStream` command for this looks like the following:

    pushStream uri=rtmp://AkamaiUserName:AkamaiPass@YOUR.akamaientrypoint.net/EntryPoint localStreamname=YourLocalStream targetStreamName=XX_YY_ZZ@WW emulateUserAgent=FMLE/3.0\ (compatible;\ FMSc/1.0) sendChunkSizeRequest=0

## Other CDNs

The EMS allows you to publish your streams to a wide variety of CDNs. These include:

- YouTubeLive
- Limelight
- Twitch.tv
- EdgeCast
- And many more!

Often times pushing streams to these CDNs is very simple and only requires you to add your username and password to the RTMP pushStream command (See RTMP section above). For many of these CDNs, you will need to specify emulateUserAgent in your pushStream command. An example pushStream command is as follows:

    pushStream uri=rtmp://UserName:Pass@EntryPoint localStreamname=YourLocalStream targetStreamName=UsuallySpecifiedInYourAccount emulateUserAgent=FMLE

## Miscellaneous Examples

To play an mpegts stream in VLC, use:

    udp://@239.1.1.1:1234

To create a stream out of a file with ffmpeg, use:

    $ ffmpeg -re -i myMovie.mp4 -acodec copy -vcodec copy -f mpegts -vbsf h264_mp4toannexb "udp://192.168.1.16:5555/"

To play HLS, send telnet command to EMS:

1. Create HLS:

        createhlsstream localstreamnames=teststream targetfolder=/var/evo-webroot groupname=testgroup playlisttype=rolling

2. Verify: Check if .ts files are generated inside targetfolder.
3. Play: In the browser, type the complete URI of the "targetfolder/groupname" where `playlist.m3u8` is located.

**PLEASE SEE THE "HOW TO" DOCUMENT FOR MORE EXCELLENT EXAMPLES!**

