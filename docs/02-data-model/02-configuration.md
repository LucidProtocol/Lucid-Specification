### Configuration

Configuration can be uploaded from or downloaded to an FD using the topics discussed below. The format of configuration information is detailed in the following sections. Errors in configuration are dealt with as detailed in Section [2.1.3.3](#configuration-errors), [Configuration errors](#configuration-errors).

#### Upload topic

\[UUID\]/UP/configuration

The complete configuration is sent from FD to SA on start-up, and when configuration changes are made locally to the FD. The version number is controlled by the FD and is increased automatically. If the SA receives a configuration version number in the status message which does not match its record of the version, then it requests the configuration using the Get Field Device Info message.

#### Download topic

\[UUID\]/DOWN/configuration

If the FD is configurable by the SA, then the FD Profile can be used to generate the FD Configuration data structure. See canConfigDown in the FD Profile which state whether download is possible. Configuration is sent from SA to FD when configuration changes are made by the user of the SA system, or if configRequired is in the status message.

On receipt and acceptance of the configuration, the FD updates the version number for the Configuration and resend the status message containing the number.

The SA may send any part or parts of the configuration payload via the Broker, applied incrementally to the FD.

The expected version number of the configuration is always in the configuration message.

#### Configuration errors

If the FD finds errors in the Configuration message, it discards the message entirely and send a new status message with the unchanged configuration version and a non-empty message within the configState. The content of the message is arbitrary and could range from \"Error\" to a description of the specific error. On a successful configuration, the status message is resent with updated version and empty or absent configState.

#### Configuration payload

Refer to the code file **Field Device Configuration.json** for an example Configuration message.

The message starts with two properties:

- The version.

- A Boolean indicating whether this is a full or a partial configuration.

A partial configuration is incremental to the existing configuration of the FD or the SA, while a full configuration implies that the previous configuration for this FD would be erased.

```json
{
  "cVer": 123,
  "full": true,
  "source": "62d18075-e5e2-49c2-89e2-abd1cc8025af",
```

The field cVer is the configuration version number.

- cVer is mandatory.

- full, when omitted, defaults to false.

The device only receives and acts on a version which is one greater than the version last received in the status message. If successful, the new version is published by the FD.

To delete items (e.g. a point) in a Field Device a full download must be performed with the deleted items no longer included in the configuration payload. Items cannot be deleted during an incremental download.

The field source is optional and sent by the SA to identify itself as the source of configuration. A GUID is used in the example, but any string could be used by the SA. It can be used by multiple SAs to determine the source of the latest configuration in the status message.

Following the properties, lists of objects are defined, as given by the Device Profile. The first is the FD. While it is an array for consistency with other object types, there is only ever one device entry:

```json
"device": [
    {
      "nid": "A1234567890",
      "name": "West Derby PRV",
      "loc": "16 Bridge St, Elton",
      "ownr": "STW",
      "asst": "ASST123456",
      "serl": "2005-12345Abc",
      "latitude": 12.4356543,
      "longitude": 76.12324,
      "scan": true
    }
  ],
```

Table 1 describes the elements within the object: **device**.

| Information | Type | JSON Name | Mandatory | Description |
|--------------|---------|------------|------------|-----------------------------------------------------|
| Node Id | String | nid | Yes | User set name of the device in its application, unique for this user's set of devices. Commonly a numeric code and used to facilitate device change. |
| Name | String | name | Yes | The name of the device |
| Location | String | loc | No | Location of the device, such as an address |
| Owner | String | ownr | No | The owner organisation of the device |
| Asset | String | asst | No | The asset code of the device |
| Serial | String | serl | No | The manufacturer's serial number of the device |
| Latitude | Number | latitude | No | WGS84 latitude. (If Location is True and this is absent then the latitude is zero). |
| Longitude | Number | longitude | No | WGS84 longitude. (If Location is True and this is absent then the longitude is zero). |
| On Scan | Boolean | scan | Yes | True to set the device active and able to return data |

: Device elements

The nid, name and scan fields are mandatory.

The nid field identifies the device to aid device replacement. While all physical FDs have unique a UUID, when replacing a device, the nid is the same.

The scan field indicates whether the device is active and returns data.

The object types which follow are linked to parent objects through the parent object type name and object index, and if necessary, a type field which, with the parent field, forms a unique reference to the parent.

For example, a point object needs a deviceLink numeric field to refer to its parent deviceLink, and an analogue object needs a point numeric field and a type field to refer to its parent point.

The format of the sections of an FD Configuration message is fully defined by the FD Profile.

#### Channel and connection information

The device defines channels which are physical communications paths from the device to the Broker. The Connection Information entry defines the details of the communications that can occur over these channels. There may be multiple connection entries over a single channel. Each connection defines a route from the device to a Broker over a single channel. The maximum number of connections is defined in the FD Profile. An FD uses its configured connection records as a prioritized list to establish a connection to the Broker.

Table 2 describes the elements within the object: **channel**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|-------------------|---------|--------|------------|----------------------------------------------------|
| Channel Number | Integer | num | Yes | Storage Index |
| Channel Name | String | name | Yes | The name of this channel |
| Connection String | String | conn | No | Optional text required by the device for this channel number. May contain device or hardware specific settings. |
| DNS Information | String | dns | No | DNS names in a comma-separated string |

: Channel elements

The numbers and names of pre-defined channels are defined in the Capability section of the FD Profile.

The following fields are defined for a connection entry:

Table 3 describes the elements within the object: **connection**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|-------------------|---------|---------|------------|---------------------------------------------------|
| Connection Number | Integer | num | Yes | Storage Index The number of the connection in the FD. |
| Connection Name | String | name | Yes | The name of this connection |
| Channel Number | Integer | channel | Yes | The channel to use |
| Network Protocol | Integer | prot | Yes | Protocol for network connection The following network protocols are defined: 0 – IPv4 (TCP) 1 – IPv6 (TCP) |
| Network Information | String | net | Yes | IP address and port number to connect to the Broker |
| Retries | Integer | retries | No | Number of retries |
| Backoff | Integer | backoff | No | Retry interval in milliseconds |

: Connection elements


**Channel Number**

The channel number defines which communications channel on the FD the connection is associated with. An FD communications channel may be physical (for example, Ethernet or serial) or virtual (for example, a modem on a serial port supporting multiplexed GSM and GPRS connections).

**Network Information**

The specific network information depends on the network protocol and is used by the FD when it needs to contact the Broker, including when it asserts a "request to connect". The following connection information is used for each network protocol:

- 0 -- IPv4 address or domain name, optionally including port number. When omitted the port number defaults to 1883. The following examples illustrate the valid formats:

<!-- -->

- 192.168.0.1

- 192.168.0.1:8883

- ms.your-company.co.uk

- ms.your-company.co.uk:8883

<!-- -->

- 1 -- IPv6 address or domain name, optionally including port number. When omitted the port number defaults to 1883. The following examples illustrate the valid formats:

<!-- -->

- ABCD:EF01:2345:6789:ABCD:EF01:2345:6789

- \[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789\]:8883

- ms.your-company.co.uk

- ms.your-company.co.uk:8883

**Retries**

If the connection attempt to the Broker fails, then it can be reattempted a number of times. This element specifies how many times the connection attempt can be retried. The number can be between 0 retries and a maximum number of retries as specified in the FD Profile.

Should all these attempts fail then the FD abandons the attempt to connect until such time as another connection is requested.

**Backoff**

If a connection attempt fails and retries are permitted, then the FD waits a number of milliseconds after the connection attempt before retrying. This element specifies how many milliseconds to wait between each connection attempt. Backoff may vary between 0 and a maximum number specified in the FD Profile. When Backoff is 0 then no delay is inserted between the failed attempt and the next attempt to connect.
