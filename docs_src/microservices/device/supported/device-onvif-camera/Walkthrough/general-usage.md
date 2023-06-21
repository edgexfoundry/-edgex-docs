# General Usage

This document will describe how to execute some of the most important types of commands used with the device service.

## Execute GetStreamURI Command through EdgeX

!!! note
    Make sure to replace `Camera001` in all the commands below, with the proper deviceName.  


1. <a name="step1"></a>Get the profile token by executing the `GetProfiles` command:
        ```bash
        curl -s http://0.0.0.0:59882/api/{{api_version}}/device/name/Camera001/MediaProfiles | jq -r '"profileToken: " + '.event.readings[].objectValue.Profiles[].Token''
        ```
        Example Output: 
        ```bash
        profileToken: profile_1
        profileToken: profile_2
        ```

2. To get the RTSP URI from the ONVIF device, execute the `GetStreamURI` command, using a profileToken found in step 1:  
        In this example, `profile_1` is the profileToken:  
        ```bash
        curl -s "http://0.0.0.0:59882/api/{{api_version}}/device/name/Camera001/StreamUri?jsonObject=$(base64 -w 0 <<< '{
            "StreamSetup" : {
                "Stream" : "RTP-Unicast",
                "Transport" : {
                "Protocol" : "RTSP"
                }
            },
            "ProfileToken": "profile_1"
        }')" | jq -r '"streamURI: " + '.event.readings[].objectValue.MediaUri.Uri''
        ```
        Example Output:
        ```bash
        streamURI: rtsp://192.168.86.34:554/stream1
        ``` 

3. Stream the RTSP stream. 
        <div class='admonition warning'>
                <p class='admonition-title'>Warning</p>
                <p>RTSP streams are insecure, as the credentials are included in plaintext. Always keep this in mind when streaming via RTSP.</p>
        </div>
        ffplay can be used to stream. The command follows this format: 
        ```bash
        ffplay -rtsp_transport tcp "rtsp://<user>:<password>@<IP address>:<port>/<streamname>"
        ```
        Using the `streamURI` returned from the previous step, run ffplay:
        ```bash
        ffplay -rtsp_transport tcp "rtsp://admin:Password123@192.168.86.34:554/stream1"
        ```
        <div class="admonition note">
        <p class="admonition-title">
        While the `streamURI` returned did not contain the username and password, those credentials are required in order to correctly authenticate the request and play the stream. Therefore, it is included in both the VLC and ffplay streaming examples.  
        </p>
        <p class="admonition-title">If the password uses special characters, you must use percent-encoding. </p></div>

4. To shut down ffplay, use the ctrl-c command.

To learn more about the API, see [here](../swagger.md)

## Troubleshooting Guide

### Axis camera authentication failure
If while using Axis cameras you face authentication failure it might help by disabling its `replay attack protection`. For doing so please refer to
[Axis-replay-attack-protection](https://help.axis.com/en-us/axis-os-hardening-guide#onvif-replay-attack-protection). For more info on this refer to
[Axis-onvif-stackoverflow](https://stackoverflow.com/questions/5180192/onvif-authentication-failure-with-axis-p3301-using-onvif-protocol).


## License

[Apache-2.0](https://github.com/edgexfoundry-holding/device-onvif-camera/blob/{{version}}/LICENSE)
