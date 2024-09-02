--- 
title: Support Cron Scheduler - Configuration
---

# Support Cron Scheduler - Configuration

Please refer to the general [Common Configuration documentation](../../configuration/CommonConfiguration.md) for configuration settings common to all services.
Below are only the additional settings and sections that are specific to Support Cron Scheduler.

!!! edgey "Edgex 3.0"
    For EdgeX 3.0 the `MessageQueue` configuration has been moved to `MessageBus` in [Common Configuration](../../configuration/CommonConfiguration.md#commonconfigurationproperties)

=== "Writable"
    |Property|Default Value|Description|
    |---|---|---|
    ||Writable properties can be set and will dynamically take effect without service restart|
    |LogLevel|INFO|log entry [severity level](https://en.wikipedia.org/wiki/Syslog#Severity_level).  Log entries not of the default level or higher are ignored. |
=== "Writable.InsecureSecrets"
    |Property|Default Value|Description|
    |---|---|---|
    |.DB|---|Secrets for connecting to postgres when running in non-secure mode |
=== "Writable.Telemetry"
    |Property|Default Value|Description|
    |---|---|---|
    |||See `Writable.Telemetry` at [Common Configuration](../../configuration/CommonConfiguration.md#commonconfigurationproperties) for the Telemetry configuration common to all services |
    |Metrics| `TBD` |Service metrics that Support Cron Scheduler collects. Boolean value indicates if reporting of the metric is enabled.|
    |Tags|`<empty>`|List of arbitrary service level tags to included with every metric that is reported. i.e. `Gateway="my-iot-gateway"` |
=== "Service"
    |Property|Default Value|Description|
    |---|---|---|
    ||| Unique settings for Support Cron Scheduler. The common settings can be found at [Common Configuration](../../configuration/CommonConfiguration.md#commonconfigurationproperties)
    |Port|59863|Micro service port number|
    |StartupMsg |This is the Support Cron Scheduler Microservice|Message logged when service completes bootstrap start-up|
=== "Clients.core-command"
    |Property|Default Value|Description|
    |---|---|---|
    |Protocol|http|The protocol to use when building a URI to the service endpoint|
    |Host|localhost|The host name or IP address where the service is hosted|
    |Port|59882|The port exposed by the target service|
=== "MessageBus.Optional"
    |Property|Default Value|Description|
    |---|---|---|
    ||| Unique settings for Support Cron Scheduler. The common settings can be found at [Common Configuration](../../configuration/CommonConfiguration.md#commonconfigurationproperties)
    |ClientId|"support-cron-scheduler|Id used when connecting to MQTT or NATS base MessageBus|
=== "Database"
    |Property|Default Value|Description|
    |---|---|---|
    |||Unique settings for Support Cron Scheduler. The common settings can be found at [Common Configuration](../../configuration/CommonConfiguration.md#commonconfigurationproperties)
    |Host|localhost|The host name or IP address where the database is hosted|
    |Port|5432|The port exposed by the database|
    |Timeout|5s|DB connection timeout|
    |Type|postgres|Indicates the type of database to use, only postgres is supported for this release|

    !!! edgey "EdgeX 3.2"
        For EdgeX 3.2 the Support Cron Scheduler service only supports `postgres` as persistence layer.

## V3 Configuration Migration Guide
No configuration updated

See [Common Configuration Reference](../../configuration/V3MigrationCommonConfig.md) for complete details on common configuration changes.
