## Events

The Events and Actions sections introduce concepts used in Section [2.4](05-points-overview-and-quality.md#points), [Points](05-points-overview-and-quality.md#points).

During the life of an FD events occur that the SA needs to be informed about. Important events need to be sent to the SA immediately. If the event is not important it can be stored, to be sent at a later time, allowing more efficient communications.

The FD maintains an event queue which it uses as the source of information to be delivered over the protocol. The implementation of such an event queue is specific to the FD, apart from the rules relating to events specified in this document.

## Actions

It is possible to associate actions with certain changes in the FD. The changes may be things such as points changing states or point quality flags changing value. Table 6 shows the possible actions.

| Action | Description |
|--------|--------------------------------------------------------------------------------------------|
| 0 | Do nothing |
| 1 | Create an event to be stored for a point. |
| 2 | Create an event to be stored for a point and trigger a connection request. This can be called an alarm. |

: Actions

During the life of an FD certain changes in the FD can occur. Where these are important, they may cause an event to be generated. Events allow the FD to inform the SA that the change has happened. The selection changes and whether events should be raised are controlled by the configuration of the device.

For example, if the configured action for the previous state is **2** and the configured action for the new state is **1**, then the previous state action (**2**) executes. It generates an instance of an event and requests a connection.

When the point is on scan and any of the point quality flags change, the action for the flag must be performed to report the new point quality flag. This is subject to the highest permitted action. This does not apply until the Restart point quality flag is cleared, and the state of the point is determined.

For more information, see the Configuration properties whenXXX in Table 8.

In the following situations the highest action of all the states, and all of the point quality flags, must be performed regardless of the highest permitted action. This is needed to report the current state of the point.

- At device start up (and following a restart).

- When the point is put on scan.

- Following a configuration change to any of the point state actions or any of the point quality flag actions.

The action is performed once the point has completed the next or initial scan of its I/O without applying persistence. That is, when the Restart point quality flag is cleared.

When action **2** is taken and a connection request is triggered, the exact action performed by the FD depends on the capabilities, configuration, and state of the FD as follows:

- If the FD can make a connection to the MQTT Broker, and a connection is not already established, then it asserts a "request to connect". This indicates to the FD that it must make a connection to the MQTT Broker to report the event.\
  The "request to connect" is used in the algorithm which the FD uses to decide when it needs to make a connection to the MQTT Broker, along with any other factors, such as retry delays; and blacklists.

- If the FD is already connected to the MQTT Broker, then the event data is immediately published.

- Otherwise, no further action is taken, and the event data is not reported until the FD next connects to the MQTT Broker.
