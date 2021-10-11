# Scheduler

![image](EdgeX_SupportingServicesScheduling.png)

## Introduction

The support scheduler microservice provide an internal EdgeX “clock” that can kick off operations in any EdgeX service.  At a configuration specified time (called an **interval**), the service calls on any EdgeX service API URL via REST to trigger an operation (called an **interval action**).  For example, the scheduler service periodically calls on core data APIs to clean up old sensed events that have been successfully exported out of EdgeX.

!!! edgey "Edgex 2.0"
    For EdgeX 2.0 the REST API provided by the Support Scheduler has changed to use DTOs (Data Transfer Objects) for all responses and for all POST/PUT/PATCH requests. All query APIs (GET) which return multiple objects, such as /all, provide `offset` and `limit` query parameters. 

### Default Interval Actions

Scheduled interval actions configured by default with the reference implementation of the service include:

- Clean up of Core-data events/readings that have been persisted for an extended period. In order to prevent the edge node from running out of space, these old events/readings are removed. This is the "ScrubAged" operation. Scheduler parameters around this operation determine how often and where to call into Core-data to invoke this operation to expunge of old data.

---
**NOTE**
The removal of stale records occurs on a configurable schedule. By default, the default action above is invoked once a day at midnight.

---

### Scheduler Persistence

Support scheduler uses a data store to persist the Interval(s) and IntervalAction(s). Persistence is accomplished by the Scheduler DB located
in your current configured database for EdgeX.

---
**Info**
    Redis DB is used by default to persist all scheduler service information to include intervals and interval actions.

---
### ISO 8601 Standard

The times and frequencies defined in the scheduler service's intervals are specified using the [international date/time standard - ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).  So, for example, the start of an interval would be represented in YYYYMMDD'T'HHmmss format.  20180101T000000 represents January 1, 2018 at midnight.  Frequencies are represented with ISO 8601 durations. 

## Data Model
The latest developed data model will be updated in the [Swagger API document](https://app.swaggerhub.com/apis-docs/EdgeXFoundry1/support-scheduler/2.0.0).

![image](EdgeX_SupportSchedulerModel.png)

This diagram is drawn by [diagram.net](https://app.diagrams.net/), and the source file is [here]( EdgeX_SupportSchedulerModel.xml).
## Data Dictionary

=== "Interval"
    |Property|Description|
    |---|---|
    ||An object defining a specific "period" in time|
    |id|Uniquely identifies an interval, for example a UUID|
    |name |the name of the given interval|
    |start|The start time of the given interval in ISO 8601 format|
    |end|The end time of the given interval in ISO 8601 format|
    |interval |How often the specific resource needs to be polled. It represents as a duration string. The format of this field is to be an unsigned integer followed by a unit which may be "ns", "us" (or "µs"), "ms", "s", "m", "h" representing nanoseconds, microseconds, milliseconds, seconds, minutes or hours. Eg, "100ms", "24h"|
=== "IntervalAction"
    |Property|Description|
    |---|---|
    ||The action triggered by the service when the associated interval occurs|
    |id|Uniquely identifies an interval action, for example a UUID|
    |created|A timestamp indicating when the interval action was created in the database|
    |modified|A timestamp indicating when the interval action was last modified|
    |name |the name of the interval action|
    |intervalName|associated interval that defines when the action occurs|
    |adminState|interval action state - either LOCKED or UNLOCKED|
    |address|The address shows the information indicating how to contact a specific endpoint|
    |type|Indicates which protocol should be used. |
    |host|The host targeted by the action when it activates|
    |port|The port on the targeted host|
    |httpMethod |Indicates which Http verb should be used for the REST endpoint.(Only using when type is REST|
    |path|The HTTP path at the targeted host for fulfillment of the action.(Only using when type is REST)|
    |Publisher|The client to publish the message. The client ID must be unique across all clients connecting to the same broker.(Only using when type is MQTT)|
    |topic|The topic to publish to.|
    |qos|Determines the quality of service level used to guarantee message delivery.(Only using when type is MQTT)|
    |keepAlive|Maximum time in seconds to allow the connection to the MQTT broker to remain idle.(Only using when type is MQTT)|
    |retained|Determines whether or not the MQTT broker retains the message last published.(Only using when type is MQTT)|
    |autoReconnect|Reconnect to the broker when the connection failure.(Only using when type is MQTT)|
    |connectTimeout|The time interval in seconds to allow a connect to complete.(Only using when type is MQTT)|

## High Level Interaction Diagrams

**Scheduler interval actions to expunge old and exported (pushed) records from Core Data**

![image](EdgeX_CoreDataCleanUp.png)

## Configuration Properties

Please refer to the general [Configuration documentation](../../configuration/CommonConfiguration.md) for configuration properties common to all services. Below are only the additional settings and sections that are not common to all EdgeX Services.

=== "ScheduleIntervalTime"
    |Property|Default Value|Description|
    |---|---|---|
    |ScheduleIntervalTime|500|the time, in milliseconds, to trigger any applicable interval actions|
=== "Intervals/Intervals.Midnight"
    |Property|Default Value|Description|
    |---|---|---|
    ||Default intervals for use with default interval actions|
    |Name|midnight|Name of the every day at midnight interval|
    |Start|20180101T000000|Indicates the start time for the midnight interval which is a midnight, Jan 1, 2018 which effectively sets the start time as of right now since this is in the past|
    |Interval|24h|defines a frequency of every 24 hours|
=== "IntervalActions.IntervalActions.ScrubAged"
    |Property|Default Value|Description|
    |---|---|---|
    ||Configuration of the core data clean old events operation which is to kick off every midnight|
    |Name|scrub-aged-events|name of the interval action|
    |Host|localhost|run the request on core data assumed to be on the localhost|
    |Port|59880|run the request against the default core data port|
    |Protocol|http|Make a RESTful request to core data|
    |Method|DELETE|Make a RESTful delete operation request to core data|
    |Target|core-data|target core data|
    |Path|/api/v2/event/age/604800000000000|request core data's remove old events API with parameter of 7 days |
    |Interval|midnight|run the operation every midnight as specified by the configuration defined interval|



### V2 Configuration Migration Guide

Refer to the [Common Configuration Migration Guide](../../../configuration/V2MigrationCommonConfig) for details on migrating the common configuration sections such as `Service`.

## API Reference

[Support Scheduler API Reference](../../../api/support/Ch-APISupportScheduler.md )
