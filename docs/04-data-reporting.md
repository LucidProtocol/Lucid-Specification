# Data reporting

The FD remembers what data has not previously been sent. That data is then fully transmitted on a connection, for example:

- On scheduled connection

- On an action which causes a connection

- On receipt of a request (as in following section)

Unsent data includes time-logged or event data.

## Data formats

The FD sends data values to the SA in a flexible data format. This supports simple data as well as full detail. While in JSON format, variable names and data layout are optimised to reduce message size.

Data is sent in the order that it is gathered. FD Configuration regulates when and how data is sent, such as on a priority basis. However, earlier data is always sent before later data unless the FD is responding to a request for data gathered in a specified period.

Reporting of data is always in ascending time order unless either of these two boolean properties are true in the FD Profile.

- If reportEventsDirectly is specified with true, then the FD may send data values where the Action type is 3, and the data values associated with that event can be sent before other data values.

- If respondCurrentDataDirectly is specified with true, then the FD will respond to SA current data request messages with the latest current data before sending older data.

\[UUID\]/UP/data

Points can be referred to by their name or by a specific point type and numeric index \":\".

Valid point types are defined in table 9.

The numeric format will reduce message size and may be preferred by field devices.

Note that it is valid to include both name and type / index, but if are included then only the type / index element is used, the \'name\' is ignored.

The QoS is 1. The JSON message is:

```json
 // All data in the block:
{
 // version of the configuration settings data
"cVer":123,
 // All data for this point
"p":[
   {
 // Identification of this point, either
 // by point type and point number:
    "ai":1,
 // or by name:
    "name":"Pump 1.Rate",
 // Value as real or integer (true or false are not 
 // used) for analogue, counter or binary points:
    "v":[12.3, ...],
 // Or the value may be a string value for text or 
 // BASE64 MIME data in string points only.
    "vs":["string content", ...],
 // Optional quality flags as bitmask, date/time of this
 // value. This can include the override flag. Default
 // quality is 0.
    "q":[1, ...],
 // Time of start and interval in milliseconds for 
 // subsequent values, 
    "t":{"s": 1700497254000,"i":900000},
 // Or individual time values as an array. 
    "ts":[ 1700497254000, ...],

 // Optional if the reason is 4 (Timed Report) 
 // reasons for logging the value: 
 // 0="No Change",
 // 1="Current", 
 // 2="Value Change", 
 // 3="State Change (i.e., Alarm)", 
 // 4="Timed Report (log)", 
 // 5="End of Period", 
 // 6="End of Period Reset"
 // Default if not specified is 4.
    "r":[4, ...],
 // Optional alarm state (default is 0), 
 // -1 is a general alarm indication, 1..2..3 etc are 
 // alarm limit numbers. 
 // 100 and 101 are under-range and over-range 
 // respectively.
    "a":[1, ...]
    }
  ]
}
```

Only Alarm, Quality and Reason can be optional, and these have default values.

All array sizes must match with the following exception. Any of the Alarm, Quality or Reason items can be an array containing a single value which then applies to all values in the block for that point.

All data blocks are processed from start to end. Data for a single point must be sent in chronological order, although data for multiple points can be interleaved or in point order.

Data messages carry the FD's configuration version.

**Note**: If the version within the data message differs from the configuration currently active within the SA, then the data must be discarded (or otherwise excluded from processing).

**Implementation Note / Future Consideration for SA\'s**

The default requirement is to for the SA to discard inconsistent messages. However, an SA may elect to provide more flexible SA specific optional behaviour and buffer these inconsistent data messages until the new configuration has been received, validated, and activated by the SA. Once alignment is confirmed, the buffered data could then be processed using the updated configuration mappings, preventing data loss during configuration transitions.

## Requesting data

If supported as listed in the features section of the profile, the SA may send a message to the FD to request data values. This is optional and a device may be designed only to report data.

Typically, a device stores data until its scheduled time to connect and send data, or until a high priority event, sometimes termed alarm, causes connection and send.

\[UUID\]/DOWN/getdata

The QoS is 0, and the message is:

- **Or** to retrieve a list of current point values, one for each configured point. To avoid issues with data ordering the device sends stored/unsent data before sending the current point data for all points, including timestamps. The Current Data Reason flag is used for these values.

```json
  {"data":"current"}
```

- **Or** to retrieve a list of current (at this time) point data, by point type / index. Again, to avoid issues with data ordering, the device sends all unsent data before then sending current point data.

```json
{
    "data":"current", 
    "p":[{"ai":1},{"ai":2}]
}
```

- **Or**:

```json
{
    "data":"current", 
    "name":["Pump 1.Rate"]
}
```

With an optional time start to (re-)retrieve historic data from this time to the current time. Historic values include data stored because of time Incident Logging:

```json
{
    "data":"history", 
    "p":[{"a":1}]
}
```

- **Or**:

```json
{
    "data":"history",
    "name":["Pump 1.Rate"], 
    "start":1694695035000, 
    "end":1694695235000 
}
```

The end time is optional.

The expected response is a standard data message.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|--------------------------|----------------|---------|------------|--------------------------------------|
| Type of Data Requested | String | data | Yes | "Current" or "History" |
| Point ID List | String Array | n | No | Array of point identifiers |
| Point Name List | String Array | name | No | Array of point names |
| Start Time | Integer | start | No | Start time (inclusive) |
| End Time | Integer | end | No | End time (inclusive) |

: Elements of the data request message
