### Connection scheduling

The FD sends information to the SA via the Broker. It also receives messages from the SA via subscriptions to messages on that same Broker. Two events cause the FD to initiate a connection to the Broker:

- An event can occur within the FD.\
  For example, the state of a point changing that is required to be published immediately.

- A scheduled connection occurs.

The scheduled connection entry defines when these scheduled connections should occur. The capability section of the Field Device Profile specifies the number of available connection schedules. The fields are as follows:

Table 4 describes the elements within the object: **connectionSchedule**.

| **Information** | **Type** | **JSON Name** | **Mandatory** | **Description** |
|----------------|---------|-----------------|------------|----------------------------------------------|
| Connection Schedule Number | Integer | num | Yes | Storage Index |
| Start Time | Integer | start | No | The start time for the scheduled connection, as an offset in milliseconds from the start of the specified boundary). |
| Repeat Interval | Integer | repeat | No | The number of milliseconds between the start of each connection attempt. |
| Repeat Count | Integer | count | No | The number of repeats made after the initial connection, during the startBoundary interval |
| Start Time Boundary | Integer | startBoundary | No | 0 = Day (default), 1 = Week, 2 = Month. |

: Information used in defining scheduled connections

**Start Time**

The start time indicates the first time that an FD must attempt to contact the Broker within a week or month.

For more information, see Section [2.1.5](#time-formats), [Time formats](#time-formats).

**Repeat Interval**

The repeat interval indicates the period in milliseconds at which the FD must re-attempt to contact the Broker, starting at the Start Time. A repeat interval of zero indicates that the scheduled connection is once per week or month. Scheduled connections repeat at the same time each week. For example, a connection at the start of Tuesday and Friday is achieved using a boundary of a week, a start time of 24 hours, and an interval of three days.

**Repeat Count**

The number of repeat connections that are made during the startBoundary interval after the initial connection has been made. For example, if the repeat count is two, then there would be three connection attempts (one initial connection plus two repeats). These would be spaced by the repeat interval.

**Start Boundary**

If 0 (default) the start boundary is a day (from midnight UTC). If 1 the start boundary is a week (from midnight UTC Monday), if 2 the start boundary is a calendar month (from midnight UTC 1^st^ day).

### Time formats

Table 5 describes the time formats used within the protocol.

| Time format | Definition |
|----------|------------------------------------------------------------------------------------------|
| Absolute time | All times are specified in UTC. Times are specified numerically, in milliseconds from 1 Jan 1970 00:00:00 in UTC, as a JSON integer. For example: 1491550944123 ([<u>https://en.wikipedia.org/wiki/Unix_time</u>](https://en.wikipedia.org/wiki/Unix_time)) |
| Relative time | All durations are specified in milliseconds as a number. For example: Connection backoff time and binary state change persistence. |
| Weeks | The week is defined to start on Monday at 00:00:00 (that is, midnight between Sunday and Monday in UTC). |

: Time formats
