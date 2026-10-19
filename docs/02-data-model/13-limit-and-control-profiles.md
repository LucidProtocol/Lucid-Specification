## Limit and control profiles

Profiles are time-varying values. A profile can be attached to an analogue or counter limit to cause that limit to vary across a week. They can also be attached to a control profile object.

### Limit profiles

A Limit Profile can be attached to an analogue or counter alarm limit or be attached and used by a control profile.

The information in Table 18 is used to define limit profiles within the object: **limitProfile**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|--------------|---------|--------|------------|---------------------------------------------------------|
| Limit Profile Index | Integer | num | Yes | An index for this limit. |
| Values | Array of Numbers | values | Yes | Values array - first value applies at first time. |
| Times | Array of Integer | times | Yes | Times array - in milliseconds from start of day. |
| Type | Integer | type | No | Whether the profile is stepped (0) or linear (1). Stepped is \'step first\'. Stepped is the default. |
| Interpolate | Integer | inter | No | The interpolation time (in milliseconds) is the time period used to compute a new value for the profile, interpolating between two profile points. If set to 0 then no interpolation is performed. Applies to control values. |

: Information used in defining limit profiles for analogue and counter points

### Control profiles

The information in Table 19 is used to define limit profiles within the object: **controlProfile**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|-----------------------|-----------|-------------|------------|-----------------------------------------|
| Parent Point | Integer | parent | Yes | Analogue or binary output point number. |
| Parent Type | String | parentType | Yes | "ao" or "bo" |
| Control Profile Index | Integer | num | Yes | Control Profile Index for this profile. |
| Limit Name | String | name | No | A human readable name for the limit. |
| Monday Profile | Integer | mon | No | Profile number for Monday. |
| Tuesday Profile | Integer | tue | No | Profile number for Tuesday. |
| Wednesday Profile | Integer | wed | No | Profile number for Wednesday. |
| Thursday Profile | Integer | thu | No | Profile number for Thursday. |
| Friday Profile | Integer | fri | No | Profile number for Friday. |
| Saturday Profile | Integer | sat | No | Profile number for Saturday. |
| Sunday Profile | Integer | sun | No | Profile number for Sunday. |

: Information used in defining a control profile

The Interpolate field in the Limit Profile determines how often a value is set on the control output.
