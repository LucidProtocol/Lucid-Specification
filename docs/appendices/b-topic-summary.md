# Appendix: Topic summary {#appendix-topic-summary .Appendix-H1}

This section lists the topics used by the protocol.

| Topic Name | Note | QoS | Retained |
|--------------------------------------------------------|--------------------------|--------|----------|
| \[UUID\]/UP/lwt | Last Will and Testament | 2 | 1 |
| \[UUID\]/UP/status | Status Message | 2 | 1 |
| \[UUID\]/UP/profile | FD Profile | 1 | 1 |
| \[UUID\]/DOWN/core/block/\<name\>/\<version-text\> | Data Block | 0 | 0 |
| \[UUID\]/DOWN/core/request | Request Data Block | 0 | 0 |
| \[UUID\]/UP/core/block/\<name\> | Data Block | 0 | 0 |
| \[UUID\]/DOWN/configuration | Configuration | 1 | 0 |
| \[UUID\]/UP/configuration | Configuration | 1 | 1 |
| \[UUID\]/DOWN/getdeviceinfo | Configuration | 0 | 0 |
| \[UUID\]/UP/data | Data | 1 | 0 |
| \[UUID\]/DOWN/getdata | Data | 0 | 0 |
| \[UUID\]/DOWN/setclock | Clock | 0 | 0 |
| \[UUID\]/UP/getclock | Clock | 0 | 0 |
| \[UUID\]/DOWN/control | Control | 2 | 0 |
| \[UUID\]/DOWN/override | Override | 2 | 0 |

: Topics used by the protocol

The topics are focussed on verbs, or actions, rather than a data addressing scheme.

The QoS for some messages is variable:

- The SA can, optionally, configure the QoS to be used for data, control, and override messages.

- For data, the QoS is specified in the Configuration.

- For control and override, the SA can specify the QoS to be used.
