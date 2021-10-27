# Device Service Support

The following table lists the EdgeX device services and protocols they support.

| Device Service Repository | Protocol | Releases | Versions | Status | Comments |
|---------------------------|----------|----------|----------|--------|----------|
| [device-camera-go]( https://github.com/edgexfoundry/device-camera-go)| ONVIF | Delhi-Jakarta| 0.7 - 2.x | Active| Not a full ONVIF implementation, but a good starter|
| [device-rest-go]( https://github.com/edgexfoundry/device-rest-go) | REST | Edinburgh-Jakarta| 1.0 - 2.x | Active| provides one-way communications only.  Allows posting of binary and JSON data via REST.  Events are single reading only.|
| [device-rfid-llrp-go]( https://github.com/edgexfoundry/device-rfid-llrp-go) | LLRP | Hanoi | 1.3 | Active| Communications with RFID readers via LLRP.  Work ongoing to update to Ireland, 2.x |
| [device-snmp-go]( https://github.com/edgexfoundry/device-snmp-go) | SNMP | Edinburgh-Jakarta| 1.0 and 2.x | Active| Basic implementation of SNMP protocol.  Async callbacks and traps not currently supported. |
| [device-virtual-go]( https://github.com/edgexfoundry/device-virtual-go) | | Edinburgh - Jakarta| 1.0 and 2.x | Active| Simulates sensor readings of type binary, Boolean, float, integer and unsigned integer |
| [device-mqtt-go]( https://github.com/edgexfoundry/device-mqtt-go) | MQTT | Fuji – Jakarta | 1.1 and 2.x | Active |  Two way communications via multiple MQTT topics |
| [device-modbus-go]( https://github.com/edgexfoundry/device-modbus-go) | Modbus | Dehli – Jakarta | 0.7 - 2.x | Active | Supports Modbus over TCP or RTU |
| [device-gpio]( https://github.com/edgexfoundry/device-gpio) | GPIO | Hanoi – Jakarta | 1.3 and 2.x | Active | Linux only; uses sysfs ABI |
| [device-grove-c]( https://github.com/edgexfoundry/device-grove-c) | | Edinburg – Jakarta | 1.0 and 2.x | Active | Connects the Grove sensor on Grove Raspberry Pi using libmraa library; Linux and ARM only |
| [device-bacnet-c]( https://github.com/edgexfoundry/device-bacnet-c) | BACnet | Edinburg – Hanoi | 1.0 and 2.x | Active | Currently being updated for Ireland and Jakarta.  Supports BACnet via ethernet (IP) or serial (MSTP).  Uses the Steve Karag BACnet stack |
| [device-coap-c]( https://github.com/edgexfoundry/device-coap-c) | CoAP | Hanoi - Ireland | 1.3 and 2.x | *Inactive* | This service is in the process of being redeveloped and expanded for Jakarta – and will support Thread as a subset of functionality.  Currently supports CoAP-based REST and is one way communications (read-only) |
| [device-uart]( https://github.com/edgexfoundry-holding/device-uart) | UART |  | 2.x | **in Development** | Linux only; for connecting serial UART devices to EdgeX|

## Device / Sensor List 
The following table lists known sensors or devices that have been successfully connected to EdgeX.

!!! Note
        If you have physically connected a sensor or device to EdgeX and can add to this list, please submit an issue in https://github.com/edgexfoundry/edgex-docs so that we can update the list.  Provide as many details as possible about the device.

| Device | Model | Device Service connectivity | Version | Reference|
|----------|-----------|-------------------------------------|------------|--------------|
|Comet Temperature Probe | T0310 | device-modbus-go | Hanoi | https://www.cometsystem.com/products/t0310-temperature-transmitter-with-rs232-output/reg-t0310 |
| DSD TECH USB to TTL Adapter Built-in FTDI FT232RL IC|SH-U09C2| device-uart | development | http://www.dsdtech-global.com/2017/07/dsd-tech-usb-to-ttl-serial-converter.html |
| GPIO Soil Moisture Sensor | unknown | device-gpio | Hanoi | https://learn.sparkfun.com/tutorials/soil-moisture-sensor-hookup-guide/all|
|Patlite Signal Tower | NHL-FB2 | device-snmp-go | Ireland | https://www.patlite.com/ |
|Trendnet Network Switch | TPE-082WS | device-snmp-go | Hanoi | https://www.trendnet.com/products/managed-switch/10-Port-Gigabit-Web-Smart-PoEplus-Switch-TPE-082WS |