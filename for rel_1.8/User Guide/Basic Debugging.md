# BASIC DEBUGGING



## Unable to Run EMS

**Debug:**

Debug:

1. **Check your License if already installed**. Maybe you forget to place your License file before running EMS. 
2. **Check your License if already expired**. You can no longer use EMS if your license is expired.
3. **Check if there are ports used by other applications**. There are many ports used by EMS, make sure that it is dedicated to EMS otherwise, there may be some errors with the connections.
4. **Check if you have a network connection**. If your license is an online type, EMS won't be able to connect to the License Manager who will verify your license.
5. **Check if you have a running instance of EMS.** You cannot run EMS simultaneously.
6. **Check if EMS is installed properly**. There may be some components that were not installed.
7. ​




## Pulled/Pushed Stream is not Streaming

**Debug:**

1. **Do preliminary checking**. Test you stream source by playing first on a player before pulling/pushing.

2. **Check if the stream is already pulled by sending `listConfig`**. The status should be **streaming** not connecting.

   ``` 
       pull:
         --
           configId: 1
           localStreamName: testpullStream
           status:
             current:
               description: Streaming
               uniqueStreamId: 1
           uri: rtmp://s2pchzxmtymn2k.cloudfront.net/cfx/st/mp4:sintel.mp4
   ```

3. **Check if the address of the server to push in is correct**. EMS will only look for the target address so make sure that the address in the API call is correct.

4. **Check if the localStreamName is correct.** EMS will search for the localStreamName in the API call, the stream will not be pushed if localStreamName is not found.

5. ​





## Stream is Not Recording 



**Debug:**

1. **Do preliminary checking**. Test you stream source by playing first on a player before recording.
2. **Make sure your pathToFile value is writable**. EMS will not able to record if the path given is read-only.
3. **Check if your network connection is stable**. There might be a problem occur when the network connection is not stable.




## Unable to Play using Web Sockets/WebRTC

**Debug:**

1. **Do preliminary checking.** Test you stream source by playing first on a different player.

2. **Check HTML 5 browser support.** You may check the support by clicking on this [link](https://www.youtube.com/html5?gl=PH). H.264 and MSE & H.264 should be checked.

   If using Firefox, please change the following configurations:

   ```
   1. In the Firefox address field enter: about:config
   Click “I’ll be careful I promise!”

   2. Locate the following variables and set them (double-click) as necessary to match:
   media.mediasource.enabled = true
   media.mediasource.whitelist = false
   media.mediasource.mp4.enabled = true
   media.fragmented-mp4.exposed = true
   media.fragmented-mp4.ffmpeg.enabled = true
   media.fragmented-mp4.gmp.enabled = true
   media.fragmented-mp4.use-blank-decoder = false

   3. Restart Firefox to activate the changes
   ```

3. **Check if the connection has been established.** Normally, when a browser was able to establish connection with EMS, the EMS logo would appear on the HTML5 player. If no logo appeared, then that means it wasn’t able to connect with EMS because the connection was not established or EMS was not able to successfully establish a data channel. For this scenario, For WebRTC, check that EMS has started webRTC and that you are using the correct room or send the EMS logs to EvoStream support team.

4. Check if stream is available. In this case, the logo of EvoStream would appear and would just be stuck there. If you ticked on the “Show debug messages” **IMMEDIATELY RIGHT AFTER** loading the webRTC page, a couple of logs such as these can be seen:

   ```
   1453259850224: Media source is now ready.
   1453259850224: Command: {"payload":{"description":"Requested stream could not be sent!","name":"test2", "status":"FAIL"},"type":"fmp4Response"}
   1453259850222: channelOpen emsStreamChannel
   1453259850221: Sending request for fmp4...
   1453259850221: channelOpen emsCommandChannel
   1453259850219: Connection established with qKbPr2FJHicfvNx6xRP0
   ```

   The log means, although connection was indeed established, the stream that is being requested is not available.

   ​

5. ​

   ​


