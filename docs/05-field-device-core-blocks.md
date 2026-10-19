# Field device core blocks

These are one or more data blocks or files which can optionally be uploaded and downloaded to set manufacturer-specific configurations and behaviours. See **canCoreUp** and **canCoreDown** in the FD Profile.

These are typically downloaded to the device either because the SA is instructed by users to do so, or the Required flag is set in **core** in the status message.

When successfully downloaded to the FD, the status message is updated with the new version. This indicates that the download is successful.

\[UUID\]/DOWN/core/block/\<name\>/\<version-text\>

The \<name\> is the text name given in the core structure. It could be Firmware. After a Core block is transmitted in the message content, the SA can, at that time or later, request that the block is made active. This typically applies to a logic sequence. The command is:

\[UUID\]/DOWN/core/request

With content:

```json
{ "activate" : ["<name1>", "<name2>"] }
```

And to deactivate, use the same topic with content (if applicable, e.g. to Logic programs):

```json
{ "deactivate" : ["<name1>", "<name2>"] }
```

If the SA requires that a core block is uploaded (if supported by the device), the request is:

\[UUID\]/DOWN/core/request

With content:

```json
{ "upload" : ["name1", "name2"] }
```

And the device responds with the content topic:

\[UUID\]/UP/core/block/\<name\>/\<version-text\>

And the data in the message content.

The core block data format is.

```json
{ "blockPart" : 1, "totalParts" : 1, "data" : "TWFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5IGhpcyByZWFzb24sIGJ1dCBieSB0aGlzIHNpbmd1bGFyIHBhc3Npb24gZnJvbSBvdGhlciBhbmltYWxzLCB3aGljaCBpcyBhIGx1c3Qgb2YgdGhlIG1pbmQsIHRoYXQgYnkgYSBwZXJzZXZlcmFuY2Ugb2YgZGVsaWdodCBpbiB0aGUgY29udGludWVkIGFuZCBpbmRlZmF0aWdhYmxlIGdlbmVyYXRpb24gb2Yga25vd2xlZGdlLCBleGNlZWRzIHRoZSBzaG9ydCB2ZWhlbWVuY2Ugb2YgYW55IGNhcm5hbCBwbGVhc3VyZS4=" }
```

The data is BASE64 encoded. This allows the block to be composed of multiple messages, by dividing the block into multiple parts. A block is only complete with all parts downloaded/uploaded. The JSON message may be compressed, see Section [6.3.1](06-use-of-mqtt.md#payload-compression), [Payload compression](06-use-of-mqtt.md#payload-compression).

| 🛈 | This may be necessary for transports which support limited message sizes. |
|--------------|-------------------------------------------------|
