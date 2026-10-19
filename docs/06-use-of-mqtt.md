# Use of MQTT

MQTT is a standards-based messaging protocol, or set of rules, used for machine-to-machine communication. Internet of Things (IoT) devices typically have to transmit and receive data over a resource-constrained network with limited bandwidth. These IoT devices use MQTT for data transmission and can communicate IoT data efficiently. MQTT supports messaging between devices to the cloud and the cloud to the device. This section provides information on the use of MQTT within the Lucid protocol.

Lucid supports MQTT versions 3.1.1 and 5.0.

## Retained messages and connection state

The protocol takes advantage of Retained messages for Configuration and Profile.

A vendor may choose to use the Clean Session or Clean Start flags for the SA or FD. The responsibility for ensuring that subscriptions are in place lies with the SA or FD individually, and the FD may need to avoid using these flags if connected periodically.

A vendor may choose whether to configure the FD so that the SA is aware, or unaware, of its connection state.

Where the FD requires the SA to be unaware of the connection state, the LWT message is optional. If the LWT is used, the SA can raise alarms or events when an unsolicited disconnection occurs. The FD sends an MQTT Disconnect message to disconnect.

Where the FD requires the SA to be aware of connection state, the LWT message is mandatory. The FD does not send an MQTT Disconnect message.

## Publish/subscribe transports

The MQTT transport requires a message Broker between the SA and the FD. The SA and the FD subscribe to Topics so that the Broker can send messages to the correct destinations.

The MQTT Broker maintains persistent stores for every FD that publishes information set as **Retain**. This includes the FD Profile and its Configuration. It is assumed that Retain messages are kept indefinitely by the MQTT Broker until deleted or overwritten by the FD.

## Payload

Settings and data are encoded and transferred within JSON messages between the SA and FD. The JSON is in UTF-8 with no Byte Order Mark (BOM).

Packet sizes are restricted by the MQTT protocol. The SA is expected to handle all packets sent by the FD, and the FD is expected to handle any packet size it requires for its own functions.

| 🛈 | This document gives JSON code, and for explanatory purposes includes comments within the code after // characters. These comments MUST NOT be included within any messages. Note also that JSON messages SHOULD not contain extraneous spaces or line breaks, but devices and SA's MUST tolerate them. |
|--------------|-------------------------------------------------|

### Payload compression

Compression of all data in a message can be used by the FD and may be supported by the SA if indicated in the SA Profile. This is optional. The names of compression algorithms are specified case insensitive in the Profiles.

Compressed payload messages are indicated with a prefix of two bytes, '\$' (hex 0x24, decimal 36) followed by a compression-specific byte ranging from 1 upwards.

Algorithms supported are:

- GZip compression, "gzip". Second byte is '1' (hex 0x31, decimal 49).

- Brotli compression, "br". Second byte is '2' (hex 0x32, decimal 50).

- Deflate compression, "deflate". Second byte is '3' (hex 0x33, decimal 51).

These bytes are followed by the compressed payload.

## Other topics

Messages from the SA to the FD are stored in Topics that start with \[UUID\]/DOWN/ where \[UUID\] is the UUID of the field device that the message is destined for. Messages from the FD to the SA are published to topics starting with \[UUID\]/UP/, where \[UUID\] is again the UUID of the FD.

| 🛈 | The FD does not need to know the identity of the SA (or Stations) that receive messages from the FD. |
|--------------|-------------------------------------------------|

### Getdeviceinfo

If supported as listed in the features section of the FD Profile, the SA may send a message to the FD to request information. This is optional.

An SA may request different elements. The responses are as defined for each of the elements above. Responses are sent on their respective Topics. There are no time constraints on the response, and other messages could be sent first.

\[UUID\]/DOWN/getdeviceinfo

The QoS is 0, and the message is:

- Cause status message in the FD to be sent:

```json
  {"config":"status"}
```

- Cause FD Profile data in the FD to be sent (if capable):

```json
  {"config":"profile"}
```

- Cause configuration data in the FD to be sent.

```json
  {"config":"configuration"}
```

### Maintaining time

Where there is no better alternative to setting the time on an FD, this optional part of the protocol provides a simple but low accuracy method.

If supported by both SA and FD (as specified in the FD Profile and SA Profile), the SA may send a message to the FD to set its clock. Alternatively, the device may request the SA time. Support is to be indicated in the FD Profile data of SA and FD.

This method uses an exchange of messages to gather a time difference between FD and SA and allow the FD to set the time if appropriate. If the FD requires its clock to be set, it can indicate this in the status message. The SA is responsible for determining when the clock is to be set, whether this is to a timed schedule or not.

There are two ways to set time described in these two following sections:

#### From the status message

When the SA receives a status message which contains the time and the timeReq value is true, the SA responds by sending a setclock message.

The FD retains the last sent time in the status message. The FD can verify its own sent time was received and then set its own clock, if satisfied that the get/set interval indicates the clock can be set reliably.

\[UUID\]/DOWN/setclock

```json
  {"sa":1694695039000,
  "fd":1694695035000}
```

#### From an SA request

If the SA is to set the clock independently of receiving a status message, it sends:

\[UUID\]/DOWN/setclock

The QoS is 0, and the message is the SA's current time:

```json
  {"sa":1694695032000}
```

The FD may receive this message at some time later, and so it would not use this time. It responds with its own clock request as below.

\[UUID\]/UP/getclock

With the content:

```json
  {"sa":1694695032000,
  "fd":1694695035000}
```

The FD uses the received SA's time within this message. If the SA receives this message and identifies the same SA time within the message, it repeats its setclock message with a new time and the last time sent by the FD. The FD can then verify its own sent time was received and then set its own clock, if satisfied that the get/set interval indicates the clock can be set reliably.

\[UUID\]/DOWN/setclock

```json
  {"sa":1694695039000,
  "fd":1694695035000}
```

In each of the above two cases the SA and FD each use their own time tolerance figures to determine whether the times are:

a.  Different enough to require a change.

b.  Received in a short time which indicates both parties and communicating in near real-time. What constitutes a sufficiently short time is implementation-specific.

## Communications, QoS, retention

Refer to Appendix [B](appendices/b-topic-summary.md#appendix-topic-summary) for a summary by topic of the names, quality of service and retention attributes to be used.
