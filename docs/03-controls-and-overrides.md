# Controls and overrides

This section provides information on the controls and overrides used in the protocol.

## Controls

| 🛈   | Applies to analogue output, binary output, and string points only. |
|-----|--------------------------------------------------------------------|

If supported as listed in the features section of the profile, the SA may send a message to the FD to control a point output. The evidence of an output can be received through the value received in the data message, as configured on the output point.

\[UUID\]/DOWN/control

The QoS is 2, and the message is:

- Control by point identifications:

```json
{
    "c":[{"ao":1,"v":1}]
}
```

- Or by name:

```json
{
    "c":[{"name":"Pump 1.Command","v":1}]
}
```

- If the value is a string:

```json
{
    "c":[{"name":"Pump 1.State","vs":"Fail"}]
}
```

Use the value property "v" only for numeric point types (Analog and Binary) and "vs" only for the string point type.

If this is a pulsed control, that is, the 1 bit binary output has a non-zero pulse duration, the value is the time in milliseconds which overrides the value specified in its configuration.

If a point attached to the "Internal" Device Link is to be modified by controls, then it must be typed as an analogue output or binary output.

## Overriding a point

Subject to the **canOverride** property of points.

Extensions cannot be overridden.

When overridden the point quality is set accordingly.

If supported as listed in the features section of the profile, the SA may send a message to the FD to override the value of an input point.

\[UUID\]/DOWN/override

The QoS is 2, and the messages are:

- Override by point identifications:

```json
{
    "o":[{"ai":1,"v":1}]
}
```

- Or by name:

```json
{
    "o":[{"name":"Pump 1.Rate","v":1}]
}
```

- If the value is a string:

```json
{
    "o":[{"ai":1,"vs":"test value"}]
}
```

- Release by point identifications:

```json
{
    "r":[{"ai":1}]
}
```

- Or by name:

```json
{
    "r":[{"name":"Pump 1.Rate"}]
}
```

Use the value property \"v\" only for numeric point types (Analog, Counter and Binary) and \"vs\" only for the string point type.
