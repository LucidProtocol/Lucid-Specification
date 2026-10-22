# Introduction

This document provides a comprehensive guide to the Lucid Protocol (Lucid), a standard that permits electronic devices deployed at user sites to communicate with server systems within the user's offices. The telemetry data sent using Lucid permits operational staff working for the user to visualize and control their system.

See [this missing page](no-such-page.md).

Lucid provides a rich set of functions, previously implemented in telemetry and remote SCADA systems, using manufacturer-specific protocols. Additionally, it allows for interoperability between equipment from different manufacturers. By implementing a common standard, Lucid Protocol users can purchase and operate data sources (Field Devices) and server software (Supervisory Applications) from multiple vendors and be assured that they are compatible. This encourages competition between vendors and reduces the costs of procurement, training, operations, and maintenance.

This document contains the technical detail of the protocol. Appendices contain protocol schema and samples.

## Purpose

The protocol can be used in any industry for the management of remote data gathering and control devices.

Goals for Lucid include:

- Is simple to implement.\
  Built on standard protocols, techniques, and libraries.

- Makes efficient use of network resources.\
  Needs to work in all common situations from application to application transferring thousands of packets per minute, down to tiny devices using unreliable slow connections.

- Has a flexible architecture.\
  Connects devices to single or multiple applications and a wide variety of communications scenarios.

- Maintains common outstation and data logging functionality.\
  Has the smallest cost to implement and shares the philosophy and terminology of earlier WITS systems.

- Economic to maintain validation.\
  Simple to test and assure functionality and compatibility.

- Security.\
  Security features are included in the protocol.

## Use of the protocol

Lucid is used to provide communications between a remotely deployed Field Device (FD) and one or more central Supervisory Applications (SA), and between Supervisory Applications.

| 🛈 | The communications between FD to SA go through an intermediate Broker. So, whenever the text describes communications from an FD to an SA, or an SA to an FD, the Broker should be inferred. |
|--------|--------------------------------------------------------|

Lucid supports the transmission of information from the FD to the SA. That information is generally related to points on the FD that provide values users are interested in collecting or monitoring.

The FDs may be constrained devices of a similar nature to those deployed in Internet of Things (IoT) and Industrial Internet of Things (IIoT), or as smaller devices within Operational Technology (OT), but Lucid is not limited to these styles of devices. Indeed, Lucid may be used between SAs.

The types of functions supported by Lucid are those which from normal IoT and OT operations. The functions originally rose out of requirements within the UK Water Industry and are now considered as common functions across the whole of industrial IoT. Lucid supports the following functions:

- Point information.

- Point change reporting.

- Control of point values.

- Limit and Control Profiles to be used in reporting change or controlling point values.

- Time-summarised points.

- Calculated points.

- Logging of point data for later retrieval.

- Synchronisation of time.

- Management of connections.

- Definition of device capabilities.

- Monitoring and controlling device configuration.

- Provision of security.

- Vendor customisable extensions for SA UI layout.

- Download and upgrade of device firmware.

## Protocol history

Lucid was inspired by the WITS-DNP3 protocol, which is deployed throughout the UK, largely by water companies. It was first named WITS-IoT and was rebranded to Lucid in early 2023.

## Protocol transport

Lucid uses MQTT as a transport mechanism. Messages are exchanged over several predefined Topics in an MQTT Broker.

For more information, see Section [6](06-use-of-mqtt.md#use-of-mqtt), [Use of MQTT](06-use-of-mqtt.md#use-of-mqtt).

Lucid users publish and subscribe to these Topics to exchange messages of defined formats.

Lucid formalises the definition of the FD and SA and how they are expected to operate to allow specification of the message content. Lucid provides a definition of the JSON to be used as the content of the MQTT messages. This is provided through examples and the use of a JSON schema.

For more information, see [\[JSONSchema\]](front-matter/references.md#JSONSchema).

## Supervisory application and field device relationship

There is a many-to-many relationship between SAs and FDs. That is, an FD may report and be controlled by multiple SAs via the Broker. Also, an SA may monitor and control many FDs. The exchange of a Configuration Version (cVer) between FDs and SAs provides the mechanism to manage change.

## Lucid between supervisory applications

Lucid may be used between SAs, without an FD. In this operating mode, one SA assumes the role of an FD and generates Lucid Protocol content as if it was a Lucid FD. This can be useful for sharing FD configuration, data, and control messages between Lucid SAs.

Example:\
This may be used where a Lucid SA also operates non-Lucid native FDs and needs to share the configuration, data, and control messages with a separate Lucid SA. This allows a pure Lucid SA to fully operate with non-Lucid FDs using an intermediary Lucid-to-Native SA.

## Data representation

Lucid consists of MQTT Topics and JSON Payloads:

- A Topic defines a named path to data.

- A Payload could be any data items represented in Lucid in JSON format but may be compressed as specified by the Payload content.
