## Calculated points

Calculated points are linked to a Device Link object with a specific name Calc. They are calculated by the FD using a numeric set of codes to determine the formula. The time and offset of calculation are determined by the scanRate and scanOffset. The codes and other fields are specified in Table 20 as additional items added to the object type: **point**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|--------------|---------|----------------|------------|--------------------------------------------------|
| Calc Code Data | Array of Numbers | calcCode | No | Specify this only for a calculated point (the deviceLink is \"Calc\"). The code is compiled from an expression by the configuring application. |
| Calculate On Change | Boolean | calcOnChange | No | Causes calculation when any input values or their qualities change. |

: Information used in defining a point calculation

For details on the calculation codes and its use, please refer to Appendix [C](../appendices/c-calculated-point-code/00-overview.md#appendix-calculated-point-code). Calculated points are based on the core point properties and therefore support limits, extended points, and incident logging. The quality flags also apply with some additional notes in the appendix concerning expression or calculation errors. Calculations refer to other points by an enumeration of the point type and their point number.

## Incident logging

In most applications the logging behaviour of points and extension points, as determined by their configuration, is sufficient. There are some added logging facilities provided by the incident logging object.

The incident logging object defines additional history data logging using an object which is linked to the point to be logged. Generally, the logging of the additional history is continuous but only selectively sent to the SA.

There are two circumstances where the logging is sent:

- On request by a data retrieval message from the SA

- When the value of a second "trigger" point meets a given condition

The information in Table 21 is used to define an incident log within the object: **incidentLog:**

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|--------------|---------|-----------------|------------|------------------------------------------------|
| Parent Point | Integer | parent | Yes | Parent point number. |
| Parent Type | String | parentType | Yes | Type of the source point "ai", "bi", "ao", "bo", "c" or "x". |
| Store Number | Integer | num | Yes | Index number for this profile. |
| Trigger Point | String | triggerPoint | No | Number of the point causing this log to be delivered. |
| TriggerType | String | triggerType | No | Type of the trigger point "ai", "bi", "ao", "bo", "c". |
| Trigger Mask | Integer | triggerMask | No | Trigger occurs when point is in a state matching this bit mask. Binary inputs match by value. Analogues and counters match by alarm limit number, where the limit numbers correspond to bit numbers. |
| Trigger Start | Integer | triggerStart | No | The time (relative to the start of the incident) when logging is to begin (in milliseconds). Negative values mean the log starts before the trigger time. |
| Trigger Length | Integer | triggerLength | No | The length of time (in milliseconds) to continue logging the point after the incident. |
| Action | Integer | action | No | Action to be taken on entering this state (0=do nothing \[not relevant in this object type\], 1=log event with reason, 2=log event and trigger a connection to the SA). |
| Log Rate | Integer | logRate | Yes | The rate to be used (in milliseconds) to log the point. |
| Log Offset | Integer | logOffset | No | Offset time from the start of the week that the log is synchronised from (Monday, 00:00:00). Measured in seconds. If 0, then log is offset from the last midnight. (Default 0). |

: Information used in defining an incident log

Where no Trigger fields are present, the FD logs at the rate specified, and offset if specified, and the retrieval of the log is caused by a data message request from the SA.

Where Trigger fields are used, they cause the selected data to be sent unsolicited from the FD to SA. The action field controls whether the FD immediately contacts the SA or waits for the next connection.
