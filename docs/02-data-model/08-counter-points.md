### Counter points

Table 13 describes the elements within the object: **counter**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|--------------|---------|----------------|------------|--------------------------------------------------|
| Parent Point | integer | parent | Yes | String features belong to String parent points only. |
| Reset Period | integer | resetPeriod | No | If zero or absent, then is not reset. Interval between resets of this point\'s value calculation, in milliseconds). |
| Reset Offset | integer | resetOffset | No | Offset from start of week (Monday) in milliseconds. |
| Report Period | integer | reportPeriod | No | Interval between reports of this point\'s value. In milliseconds, starting at reset with last accumulated value. |
| Scale Value | number | scale | No | Multiplies the number of counts to evaluate the value of this counter. i.e. this is the scaled value of one count. |
| Units | string | units | No | The units are a free format ASCII string that can be used when displaying the point value through local access displays etc. The FD should not interpret any meaning from this string and may choose to ignore it completely. |
| Significant Change Value | number | change | No | The value difference (a positive value) used to determine when the point has changed by a significant amount. |

: Information used in counter points

#### Units

Counter points may have units defined. See the field **units**.

#### Counter point scaling

Scaling is optional, and if these parameters are not used, then the FD reports its raw value.

1.  **Significant change**

> Counter points support the generation of events when point values differ from previous recorded values by a certain amount. These are known as significant change events.
>
> A significant change event is generated when the absolute value of the difference between the last evented value and the current value of the point exceeds a set limit. The last evented value of the point must be the last value evented for a significant change. If no such value is available then the first recorded value of the point must be used, that is, the value when the Reset point quality flag is cleared. Significant change detection is performed on the scaled value of the point.
>
> The significant change value is the non-negative change of point value that is considered to be significant. The significant change value is specified in engineering units, and the FD stores it in engineering units, not the equivalent raw value, so that it is not affected by any subsequent change to the scaling of the point.
>
> A significant change value of 0 turns off significant change detection for that point.

| NOTICE | Significant change detection must not be performed if the point has either the Restart or Range Error point quality flag asserted. |
|--------------|-------------------------------------------------|

> The action to be taken is described in Section [2.4](05-points-overview-and-quality.md#points), [Points](05-points-overview-and-quality.md#points). The **whenChange** property of the point determines the action.
