## Points

All points are referenced to a Device Link object. The configuration structure for this is as follows:

Table 7 describes the elements within the object **deviceLink**.

| Information | Type | JSON Name | Mandatory | Description |
|------------------------------|-----------|----------|------------|--------------------------------------|
| Device Link Number | Integer | num | Yes | Storage Index |
| Name | String | name | Yes | Name of this Device Link |
| Device Link Connection String | String | conn | No | Optional device-specific string |

: Configuration structure for a device link

The Device Link Connection String may contain device or hardware specific settings.

The numbers and names of pre-defined Device Links are defined in the FD Profile in the Capability section.

There is a name for internal points, those without a physical data source/target which can have values set or overridden. The name is "Internal", and support is optional within the Device Profile.

There is a name for calculated points, "Calc", and support is optional within the Device Profile. There are point fields which relate to "Calc" points that are calculated by the FD using the Polish Notation (PN) specification, see Section [2.7](14-calculated-points-and-incident-logging.md#calculated-points), [Calculated points](14-calculated-points-and-incident-logging.md#calculated-points).

Table 8 describes the elements within the object: **point**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|------------------|---------|----------------------|------------|---------------------------------------|
| Device Link | Integer | parent | Yes | A reference to the Device Link for this point. |
| Point Number | Integer | num | Yes | A unique index of the point within this point type. |
| Type | String (enum) | type | Yes | The point type code. |
| Name | String | name | No | The name is optional and is a string which can be used by the device to report data values or to display locally. Furthermore, the name string may contain a hierarchy of fields with a separator which can imply a structure. |
| Point Class | String | className | No | The class is optional and is a string which can be used by the SA to denote equipment or asset ontology. |
| Scan Flag | Boolean | scan | Yes | On-scan is set to True if the point is active (or needs to be made active). |
| Can Override | Boolean | canOverride | No | Whether local override is permitted. |
| Scan Rate | Integer | scanRate | No | Base periodic read rate of the source data/hardware. In milliseconds. Also used if present to specify the calculation rate for Calculation points. If not specified the device may use its own rate, but is the faster of the logRate or any attached incidentLog rates. |
| Scan Offset | Integer | scanOffset | No | Offset time from the start of the week that the scanning is synchronised from (Monday, 00:00:00). Measured in milliseconds. If 0, then log is offset from the last midnight. Also used if present to specify the calculation offset for Calculation points. |
| Log Rate | Integer | logRate | No | Base periodic logging rate (absent or 0=no logging). In milliseconds. |
| Log Offset | Integer | logOffset | No | Offset time from the start of the week that the log is synchronised from (Monday, 00:00:00). Measured in milliseconds. If 0, then log is offset from the last midnight. |
| When Connected | Integer | whenConnected | No | When the device has successfully contacted the Broker, the value is sent to the SA if this action is 1. (If action 2 is selected then it will not contact the SA and will behave as for action 1). |
| When Online | Integer | whenOnline | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Restarted | Integer | whenRestart | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Communications Lost | Integer | whenCommsLost | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Remotely Forced | Integer | whenRemForce | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Locally Forced | Integer | whenLocForce | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Chatter | Integer | whenChatter | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Overrange | Integer | whenOverrange | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Discontinuity | Integer | whenDiscontinuity | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Reference Error | Integer | whenRefError | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When UnableTo Read | Integer | whenUnableToRead | No | The action to be taken when this flag changes (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |
| When Changed | Integer | whenChange | No | If the point has changed value. The action may be 0=do nothing (default), 1=log event, 2=log event and trigger a connection to the SA. Behaviour varies slightly with type: Analogue and counter values change by a significant change deadband. Strings can include any change. Digital points have their own change requirement per state. |

: Configuration structure for a point


An FD can support various types of points. See Table 9 for further information.

| Point type      | Object name | Identifier |
|-----------------------------------------------------|-----------------------------------|------------|
| Analogue input  | analogue    | ai         |
| Analogue output | analogue    | ao         |
| Binary input    | binary      | bi         |
| Binary output   | binary      | bo         |
| Counter         | counter     | c          |
| String          | string      | s          |
| Extended point  | extPoint    | x          |

: Supported point types and identifiers used within the protocol

The point type identifier is combined with a number to uniquely identify every point on an FD and is an efficient way to refer to points. The number should be a whole number (0 or above). The unique identifier is known as the point identifier. For example, **ai0** is the point identifier for analogue input 0 and **s4** is the point identifier for string point 4.

Points can optionally have a point name. If defined this is a string which identifies the point and is unique on the FD. When specifying a point within the protocol either the point identifier or point name can be used.

The only way to set a point on or off scan is to use the configuration message.

The class is optional and is a string which can be used by the SA to denote equipment or asset ontology.

Point values for analogues, binaries, counters, and extended points are normally integers or reals. Binary inputs and outputs are numeric values such as 0, 1, 2, and so on. String points are represented by normal JavaScript strings. Strings encoded as non-text MIME types are represented by a value encoded as a BASE64 string.

Point values are updated at various times. For native binary and counter points these may be updated immediately their value changes. However, in some cases point values may only be updated at specific times. For example, an analogue input point may only have its value taken occasionally, or a point may be read over a Modbus interface which is only scanned at a certain rate.

The rate at which point values are taken determines the values used in point state evaluation and calculation of extended point values. To ensure users are aware of when point values are available, each point can have a default update period defined. Without this definition a point value would be updated when a log value is requested or when a read is requested via the protocol. A default update period of 0ms would mean that a point is updated immediately it changes. An optional separately specified scan rate and offset are available to specify the hardware read rate, useful for extended points such as maximum or minimum.

Extended points are time-based derivations from other points such as Minimum or Integrator.

### Point quality flags

Each point has accompanying flags providing quality information about the point. The flags accompany the point value as a bitmap and give extra information about the value, either the quality of the input or the reason the value was logged. The point quality flags are defined in Table 10.

| **Name** | **Bit Number** | **Description** |
|----------------|--------|----------------------------------------------------------------------------|
| Offline | 0 | Indicates that the point is not active and available. If offline is set, then the point is off scan. While online the point behaves normally. Offline is set if the point is not yet active, disabled, or off scan. For example, a point may not be active or be disabled because it is faulty or powered down. |
| Restart | 1 | Indicates that a point is restarting. This is set when the device is restarted. It may also be set when the point is reconfigured. For an input point, restart indicates that the point has not yet provided a first data value. Once that value has been provided, restart is reset. For an output value, restart indicates that the point cannot accept a command. Restart is reset once the output point can accept commands. |
| Comms Lost | 2 | Indicates that there is a communications failure in the path used to access data from the point by the FD. Data from the point cannot be relied on in this case and the last known value is assumed to be stale. When communications with the point is returned Comms Lost is reset. |
| Remote Forced | 3 | Indicates that read from a device connected to the FD is being overridden by that device or a device downstream from that device. |
| Local Forced | 4 | Indicates that the value of this point is being overridden by the FD. This may be due to human intervention, a diagnostic mode of the device, the point being overridden by the protocol, or some other reason. |
| Chatter | 5 | This flag applies to Binary Inputs only. This flag indicates that a chatter filter is being applied by the FD to the binary input. The chatter filter allows the FD to reduce the number of events generated by the device when the input is changing quickly. The exact definition of what changing quickly means is device dependant and not something that can be changed by the protocol. When the chatter filter is being applied the points value does not necessarily represent the true value of the point as the value may be forced by the filter. |
| Range Error | 6 | This flag applies to analogue inputs and outputs only. It indicates that the value of the analogue point has exceeded the valid range for that point. If the full-scale limits of the convertor used to produce the analogue value are not exceeded, then the value of the point reported is the actual value, not limited to the valid range of the point. If the full-scale limits of the convertor are exceeded, then the value reported is the value of the relevant full-scale limit. |
| Discontinuity | 7 | This flag applies to Counters only. It indicates that the value being reported with the flag cannot be used with the previous value to give a count difference. For example, this applies if a counter value was reset. |
| Reference Error | 8 | This flag applies to analogue inputs and outputs only. It indicates a problem with the measurement process for the point which results in the point value being inaccurate or incorrect. |
| Unable to Read | 9 | This flag applies to analogue inputs and outputs only. It indicates a problem with the measurement process for the point which results in the point value being unavailable. |

: Point quality flags


The point quality flags can be configured to generate events. Refer to the fields whenXXXX in Table 8, for more information. If a point quality flag generates an event, then the reason code will be set to "State Change" assuming there is no other valid reason behind the event. See Section [4.1](../04-data-reporting.md#data-formats) for more details of the reason codes.

The values used when quality flags are set are usually the last known value, or zero (or equivalent) if the value was never known.
