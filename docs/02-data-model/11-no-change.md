### No change

Analogue and counter points support no change detection. The change in value over a period of time is calculated and then compared to a deadband.

No change detection is performed on the scaled value of the point.

The information in Table 16 is used to define no change for analogue and counter points within the object: **noChange**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|--------------|---------|-------------|------------|----------------------------------------------------|
| Parent Point | Integer | parent | Yes | Analogue point number. |
| Parent Point Type | String | parentType | Yes | Type of point the limit refers to, "ai", "ao", "x", "c". |
| Point Limit Index | Integer | num | Yes | Point Limit Index (PLI) for limit. |
| Name | String | name | No | A human readable name for the limit. |
| No Change Deadband | Number | deadband | No | The size of the deadband within which the point value must remain to cause the no change event. Default 0. |
| No Change Period | Integer | period | Yes | The time in milliseconds that must elapse with the value remaining within the deadband to cause the no change event (0=disable no change detection). |
| No Change Action | Integer | action | Yes | Action to be taken on no change (0=do nothing, 1=log event with reason, 2=log event and trigger a connection to the SA). |

: Information used in defining no change for analogue and counter points

The no change deadband is the non-negative value that defines the maximum amount by which the point's value can change within the change period and still be deemed to have not changed.

The no change deadband is specified in engineering units, and the FD must store it in engineer units, not just the equivalent raw value, so that it is not affected by any subsequent change to the scaling of the point. A no change deadband of 0 is valid and means that any non-zero change indicates the point's value has changed.

The no change period is the time in milliseconds that the point must remain within the deadband to cause the event to occur. A no change period of 0 turns off no change detection.

The period is started from the time the point value is first read and is reset when a point exceeds the no change threshold. At this time the value which is used to compare current values is reset to the changed value.

If a counter point rolls over during a no change period, then the FD must compensate when calculating the no change indication. The following algorithm is used to calculate if the value has not changed:

*abs (new value - old value) ≤ no change deadband*

Only valid values are included in the derivation. The source point is deemed valid if the Offline Point quality flag is clear, and the Restart and Comms lost flags are both clear. However, if the Local Forced flag is set, the point should convey valid data and may be considered in the calculation regardless of the other flags. The Remote Forced flag may also indicate valid data provided that Offline is clear and Comms Lost is clear. Other combinations of the point quality flags indicate that the data is invalid.

The action to be taken is as described in Section [2.6](13-limit-and-control-profiles.md#limit-and-control-profiles), [Limit and control profiles](13-limit-and-control-profiles.md#limit-and-control-profiles).
