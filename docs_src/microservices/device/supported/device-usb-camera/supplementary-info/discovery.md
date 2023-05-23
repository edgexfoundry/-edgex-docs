# Dynamic Discovery
The device service supports [dynamic discovery](../../../../Ch-DeviceServices/#dynamic-provisioning).
During dynamic discovery, the device service scans all connected USB devices and sends the discovered cameras to Core Metadata.
The device name of the camera discovered by the device service is comprised of Card Name and Serial Number, and the characters colon, space and dot will be replaced with underscores as they are invalid characters for device names in EdgeX.
Take the camera Logitech C270 as an example, it's Card Name is "C270 HD WEBCAM" and the Serial Number is "B1CF0E50" hence the device name - "C270_HD_WEBCAM-B1CF0E50".

!!! Note 
    Card Name and Serial number are used by the device service to uniquely identify a camera. Some manufactures, however, may not support unique serial numbers for their cameras. Please check with your camera manufacturer.

## Dynamic Discovery function
Dynamic discovery is enabled by default to make setup easier. It can be disabled by changing the `Enabled` option to `false` as shown below.

### Disable discovery

=== "configuration.yaml"
    !!! example - "Snippet from device.yaml"
        ```yaml
        Device: 
        ...
            Discovery:
            Enabled: false
            Interval: "1h"
        ```

=== "Docker / Env Vars"
    ```shell
    export DEVICE_DISCOVERY_ENABLED=false
    export DEVICE_DISCOVERY_INTERVAL=1h
    ```

### Configure discovery interval
=== "configuration.yaml"
    !!! example - "Snippet from device.yaml"
        ```yaml
        Device: 
        ...
            Discovery:
            Enabled: true
            Interval: "1h"
        ```

=== "Docker / Env Vars"
    ```shell
    export DEVICE_DISCOVERY_ENABLED=true
    export DEVICE_DISCOVERY_INTERVAL=1h
    ```

To manually trigger a Dynamic Discovery, use this [device service API](https://app.swaggerhub.com/apis-docs/EdgeXFoundry1/device-sdk/{{version}}.0#/default/post_discovery).

```shell
 curl -X POST http://<service-host>:59983/api/v3/discovery
```

The interval value must be a [Go duration](https://pkg.go.dev/time#ParseDuration).


## Rediscovery
The device service is able to rediscover and update devices that have been discovered previously.
Nothing additional is needed to enable this. It will run whenever the discover call is sent, regardless
of whether it is a manual or automated call to discover. The steps to configure discovery or to
manually trigger discovery is explained [here](#dynamic-discovery-function)

## Configure the Provision Watchers

!!! Note
    This section is for manually adding provision watchers, one is already added by default.

The provision watcher sets up parameters for EdgeX to automatically add devices to core-metadata. They can be configured to look for certain features, as well as block features. The default provision watcher is sufficient unless you plan on having multiple different cameras with different profiles and resources. Learn more about provision watchers [here](../../../../core/metadata/Ch-Metadata.md#provision-watcher). The provision watchers are located at `./cmd/res/provision_watchers`.


!!! example - "Example Command"
    ```shell
    curl -X POST \
    -d '[
    {
        "provisionwatcher":{
            "apiVersion":"v3",
            "name":"USB-Camera-Provision-Watcher",
            "adminState":"UNLOCKED",
            "identifiers":{
                "Path": "."
            },
            "serviceName": "device-usb-camera",
            "profileName": "USB-Camera-General"
        },
        "apiVersion":"v3"
    }
    ]' http://localhost:59881/api/v3/provisionwatcher
    ```
