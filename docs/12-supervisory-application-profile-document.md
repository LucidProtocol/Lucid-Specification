# Supervisory application profile document

This document currently plays no part in the protocol. It lists the features and characteristics of the SA to enable a user to determine how the SA interoperates with the FD.

Vendors of a Lucid SA must issue this document to the WITS PSA and to users.

The SA Profile document is in a JSON format.

```json
{
 // The Lucid version number to which this profile 
 // document relates. If more than one Lucid version is 
 // supported then multiple documents would be issues,
 // each specifying those versions would be applicable to 
 // the SA.
  "lucid": 3,
 // Identification of the Supervisory Application.
  "SAName": "ACME Lucid",
 // Version of the Supervisory Application.
  "SAVersion": "1.1",
 // Number of decimal places the SA will store and process time
 // stamps.
  "timePrecision": 3,
 // Capability for file data transfers in either direction, 
 // denotes whether each block can be uploaded or downloaded by
 // the SA Application. (Every SA must be able to receive 
 // the configuration).
 // If true, then the user needs to be able to enable or 
 // disable this functionality.
  "canCoreUp": false,
  "canCoreDown": true,
  "canConfigDown": true,
 // Capability for sending "getdata" requests.
  "canGetData": true,
 // Capability for sending "control" requests.
  "canControl": true,
 // Capability for sending "override" requests.
  "canOverride": true,
 // Capability for time setting using the protocol.
 // If true, then the user needs to be able to enable or 
 // disable this functionality
  "canSetClock": true,
 // Compression:
  "supportCompression": ["gzip"],
 // Capability for supporting MIME points and data. The MIME 
 // object will be stored by the Supervisory Application. There
 // is no obligation for any specific processing or display.
  "supportMIME": true,
 // The maximum number of limits per analogue point.
  "maxAnalogueLimits": 8,
 // The maximum number of limits per counter point.
  "maxCounterLimits": 4,
 // Capability of supporting new vendor extension properties.
  "supportVendorProperties": false,
 // Capability of supporting new vendor extension objects.
  "supportVendorObjects": false,
 // Object types supported by this SA (all are assumed if this 
 // is not present).
  "supportedObjectTypes": ["device", "channel", "connection", 
  "connectionSchedule", "deviceLink", "point", "binary", 
  "analogue", "counter", "string", "analogueLimit", 
  "counterLimit", "noChange", 
  "extPoint", "limitProfile", "controlProfile", "incidentLog"],
 // Calculated points are a feature of point objects,
 // but an SA may or may not support configuring them.
  "supportCalculatedPointConfig": false,
  
}
```
