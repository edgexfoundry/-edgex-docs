# Go Device SDK API

The `DeviceServiceSDK` API provides the following APIs for the device service developer to use.

```go
type DeviceServiceSDK interface {
	AddDeviceAutoEvent(deviceName string, event models.AutoEvent) error
	RemoveDeviceAutoEvent(deviceName string, event models.AutoEvent) error
	AddDevice(device models.Device) (string, error)
	Devices() []models.Device
	GetDeviceByName(name string) (models.Device, error)
	RemoveDeviceByName(name string) error
	UpdateDeviceOperatingState(deviceName string, state string) error
	AddDeviceProfile(profile models.DeviceProfile) (string, error)
	DeviceProfiles() []models.DeviceProfile
	GetProfileByName(name string) (models.DeviceProfile, error)
	RemoveDeviceProfileByName(name string) error
	UpdateDeviceProfile(profile models.DeviceProfile) error
	DeviceCommand(deviceName string, commandName string) (models.DeviceCommand, bool)
	DeviceResource(deviceName string, deviceResource string) (models.DeviceResource, bool)
	AddProvisionWatcher(watcher models.ProvisionWatcher) (string, error)
	ProvisionWatchers() []models.ProvisionWatcher
	GetProvisionWatcherByName(name string) (models.ProvisionWatcher, error)
	RemoveProvisionWatcher(name string) error
	UpdateProvisionWatcher(watcher models.ProvisionWatcher) error
	Name() string
	Version() string
	AsyncReadings() bool
	DeviceDiscovery() bool
	AddRoute(route string, handler func(http.ResponseWriter, *http.Request), methods ...string) error
	Stop(force bool)
	LoadCustomConfig(customConfig service.UpdatableConfig, sectionName string) error
	ListenForCustomConfigChanges(configToWatch interface{}, sectionName string, changedCallback func(interface{})) error
	GetLoggingClient() logger.LoggingClient
	GetSecretProvider() interfaces.SecretProvider
}
```



## Access Functions

### Service()

!!! edgex - "Edgex 2.3"
    This function is new in Edgex 2.3 allowing for better unit test coverage via the use of the mockable interface.

`interfaces.Service() DeviceServiceSDK`

Returns reference to the `DeviceServiceSDK` instance as a mockable interface. Mock of interfaces is available at `mocks.DeviceServiceSDK`

!!! example - "Example - Service()"
    ```go
    service := interfaces.Service()
    lc := service.GetLoggingClient()
    ```

### RunningService()

`service.RunningService() *DeviceService`

Returns concrete instance of `DeviceService` which is not a mockable interface

!!! note
    **DEPRECATED** as of Edgex 2.3. Please use `interfaces.Service()`

## APIs

### Auto Event

#### AddDeviceAutoEvent

`AddDeviceAutoEvent(deviceName string, event models.AutoEvent) error`

This API adds a new AutoEvent to the Device with given name. An error is returned if not able to add AutoEvent

#### RemoveDeviceAutoEvent 

`RemoveDeviceAutoEvent(deviceName string, event models.AutoEvent) error`

This API removes an AutoEvent from the Device with given name. An error is returned if not able to remove AutoEvent

### Device

#### AddDevice

`AddDevice(device models.Device) (string, error)`

This API adds a new Device to Core Metadata and device service's cache. Returns new Device id or an error.

#### UpdateDevice

`UpdateDevice(device models.Device) error`

This API updates the Device in Core Metadata and device service's cache. An error is returned if the Device can not be updated.

#### UpdateDeviceOperatingState

`UpdateDeviceOperatingState(deviceName string, state string) error`

This API updates the Device's operating state for the given name in Core Metadata and device service's cache. An error is return if the operating state can not be updated.

#### RemoveDeviceByName

`RemoveDeviceByName(name string) error`

This API removes the specified Device by name from Core Metadata and device service cache. An error is return if the Device can not be removed.

#### Devices

`Devices() []models.Device`

This API returns all managed Devices from the device service's cache

#### GetDeviceByName

`GetDeviceByName(name string) (models.Device, error)`

This API returns the Device by its name if it exists in the device service's cache, or returns an error.

### Device Profile

#### AddDeviceProfile

`AddDeviceProfile(profile models.DeviceProfile) (string, error)`

This API adds a new DeviceProfile to Core Metadata and device service's cache. Returns new DeviceProfile id or error

#### UpdateDeviceProfile

`UpdateDeviceProfile(profile models.DeviceProfile) error`

This API updates the DeviceProfile in Core Metadata and device service's cache. An error is returned if the DeviceProfile can not be updated.

#### RemoveDeviceProfileByName

`RemoveDeviceProfileByName(name string) error`

This API removes the specified DeviceProfile by name from Core Metadata and device service's cache. An error is return if the DeviceProfile can not be removed.

#### DeviceProfiles

`DeviceProfiles() []models.DeviceProfile`

This API returns all managed DeviceProfiles from device service's cache.

#### GetProfileByName

`GetProfileByName(name string) (models.DeviceProfile, error)`

This API returns the DeviceProfile by its name if it exists in the cache, or returns an error.

### Provision Watcher

#### AddProvisionWatcher

`AddProvisionWatcher(watcher models.ProvisionWatcher) (string, error)`

This API adds a new Watcher to Core Metadata and device service's cache. Returns new ProvisionWatcherid or error.

#### UpdateProvisionWatcher

`UpdateProvisionWatcher(watcher models.ProvisionWatcher) error`

This API updates the ProvisionWatcherin in Core Metadata and device service's cache. An error is returned if the ProvisionWatcher can not be updated.

#### RemoveProvisionWatcher

`RemoveProvisionWatcher(name string) error`

This API removes the specified ProvisionWatcherby name from Core Metadata and device service's cache. An error is return if the ProvisionWatcher can not be removed.

#### ProvisionWatchers

`ProvisionWatchers() []models.ProvisionWatcher`

This API returns all managed ProvisionWatchers from device service's cache.

#### GetProvisionWatcherByName

`GetProvisionWatcherByName(name string) (models.ProvisionWatcher, error)`

This API returns the ProvisionWatcher by its name if it exists in the device service's , or returns an error.

### Resource & Command

#### DeviceResource

`DeviceResource(deviceName string, deviceResource string) (models.DeviceResource, bool)`

This API retrieves the specific DeviceResource instance from device service's cache for the specified Device name and Resource name. Returns the DeviceResource and true if found in device service's cache or false if not found.

#### DeviceCommand

`DeviceCommand(deviceName string, commandName string) (models.DeviceCommand, bool)`

This API retrieves the specific DeviceCommand instance from device service's cache for the specified Device name and Command name. Returns the DeviceCommand  and true if found in device service's cache or false if not found.

### Custom Configuration

#### LoadCustomConfig

`LoadCustomConfig(customConfig service.UpdatableConfig, sectionName string) error`

This API attempts to load service's custom configuration. It uses the same command line flags to process the custom config in the same manner
 as the standard configuration. Returns an error is custom configuration can not be loaded. See [Custom Structured Configuration](../../../getting-started/Ch-GettingStartedSDK-Go/#custom-structured-configuration) section for more details.

#### ListenForCustomConfigChanges

`ListenForCustomConfigChanges(configToWatch interface{}, sectionName string, changedCallback func(interface{})) error`

This API attempts to start listening for changes to the specified custom configuration section. LoadCustomConfig API must be called before this API. See [Custom Structured Configuration](../../../getting-started/Ch-GettingStartedSDK-Go/#custom-structured-configuration) section for more details.

### Miscellaneous

#### Name

`Name() string`

This API returns the name of the Device Service.

####  Version

`Version() string`

This API returns the version number of the Device Service.

#### AsyncReadings

` AsyncReadings() bool`

This API returns a bool value to indicate whether the asynchronous reading is enabled via configuration.

#### DeviceDiscovery

`   DeviceDiscovery() bool`

This API returns a bool value to indicate whether the device discovery is enabled via configuration.

#### AddRoute

`AddRoute(route string, handler func(http.ResponseWriter, *http.Request), methods ...string) error`

This API allows leveraging the existing internal web server to add routes specific to the Device Service. Returns error is route could not be added.

#### GetLoggingClient

`GetLoggingClient() logger.LoggingClient`

This API returns the `LoggingClient` used to log messages.


#### GetSecretProvider 

`GetSecretProvider() interfaces.SecretProvider`

This API returns the SecretProvider used to get/save secrets

#### Stop 

`Stop(force bool)`

This API shuts down the device service gracefully.
