# Example use cases

This section details typical use cases to demonstrate the range of functionality.

| 🛈 | These cases are examples only. The text is formatted for clarity, but there will be no additional non-printing characters in the deployed messages. The \[UUID\] text would be replaced by the actual Field Device UUID. |
|--------------|-------------------------------------------------|

## Simplest case

This case is explained by the protocol messages and content. Note that in this case messages are sent from FD to SA only.

FD sends Status message:

\[UUID\]/UP/status

```json
{ "pRef": "ACME_RTUOne_1-23",
     "cVer": 1,
     "cReq": false,
     "time": 1694695035000
}
```

It contains a reference to the FD Profile, and the SA is understood to have this already, so that it does not need to be sent.

1.  FD sends Configuration to the Broker as a retained message on first connection. It could be that the device and points have been pre-configured in the SA, in which case no configuration needs to be sent. If the device is set up or designed to do so, on first contact it sends all configuration to the SA.

\[UUID\]/UP/configuration

```json
{ "cVer":1,
... <etc>
}
```

2.  FD now sends data:

\[UUID\]/UP/data

```json
{"p":[{"ai":1,"v":[12],"q":[192],"ts":[123456789321]}]}
```

## A configurable field device

1.  This device extends the above by allowing the SA to send configuration to it.

In the FD Profile this line is present:

```json
"canConfigDown":true
```

Configuration download:

\[UUID\]/DOWN/configuration

```json
{ "cVer":2,
... <etc>
}
```

3.  On receipt of new configuration, or any changed Bulk file, the FD sends a revised Status message back to the SA confirming the version.

\[UUID\]/UP/status

```json
{ "pRef": "ACME_RTUOne_1-23",
     "cVer": 2,
     "cReq": false,
     "time": 1694695035000
}
```
