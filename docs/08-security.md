# Security

The FD Profile has a security property which is a list of the level numbers supported by the device. These numbers are for reference and do not play a part in controlling communications at present. If an FD supports, for example, Levels 0 and 2, then it does not support Level 1. If unspecified the security level is 0.

The Lucid Protocol endeavours to be open and flexible. Because the protocol is used by devices, with different security needs, in applications with varying threat profiles, a choice is offered to users and implementors of the protocol. Depending on the security environment of the field device, one of six levels of security can be used ranging from Level 0: none (or external security mechanisms are already provided) to Level 5: end to end. Levels 1-4 describe uses of TLS from no authentication to full authentication.

Lucid Protocol security levels:

- **Level 0: No encryption**\
  Securing communications in this case relies on the inherent security of a communications channel or other outside IT security mechanisms. Multiple SAs are an option because message payloads are unencrypted.

- **Level 1: TLS encryption without Authentication**\
  Secures data traffic to the first hop; all traffic is encrypted between FD and MQTT Broker. No certificates are stored on the Broker or FD; device and Broker authentication are not possible. However, communications are secured against sniffing messages in transit. There is no mandated encryption of messages stored on the Broker or during onward transmission. Message encryption at rest and during onward transmission should be secured through additional IT mechanisms. TCP/IP = **TLS 1.2+**, UDP/IP = **DTLS 1.2+**. Multiple SAs are an option because message payloads are unencrypted.

- **Level 2: TLS encryption with Broker Authentication**\
  Secures data traffic to the first hop; all traffic is encrypted between FD and MQTT Broker. The TLS/DTLS Certificate of the chosen Broker is stored on the FD allowing the FD to authenticate the Broker before sending MQTT traffic. Device authentication is not possible. Communications are secured against sniffing messages in transit. There is no mandated encryption of messages stored on the Broker or during onward transmission. Message encryption at rest and during onward transmission should be secured through additional IT mechanisms. TCP/IP = **TLS 1.2+**, UDP/IP = **DTLS 1.2+**. Multiple SA are an option because message payloads are unencrypted.

- **Level 3: TLS encryption with Field Device Authentication** -- Secures data traffic to the first hop; all traffic is encrypted between FD and MQTT Broker. The TLS/DTLS Certificates of all FDs are stored on the Broker allowing the Broker to authenticate FDs during connection establishment; Broker authentication by the FD is not available. Communications are secured against sniffing messages in transit. There is no mandated encryption of messages stored on the Broker or during onward transmission. Message encryption at rest and during onward transmission should be secured through additional IT mechanisms. TCP/IP = **TLS 1.2+**, UDP/IP = **DTLS 1.2+**. Multiple SA are an option because message payloads are unencrypted.

- **Level 4: Fully Authenticated TLS encryption**\
  Secures data traffic to the first hop; all traffic is encrypted between FD and MQTT Broker. The TLS/DTLS Certificate of the chosen Broker is stored on the FD allowing the FD to authenticate the Broker before sending MQTT traffic. The certificate of the FD is stored on the Broker allowing the Broker to authenticate the FD during connection establishment. Communications are secured against sniffing messages in transit. There is no mandated encryption of messages stored on the Broker or during onward transmission. Message encryption at rest and during onward transmission should be secured through additional IT mechanisms. TCP/IP = **TLS 1.2+**, UDP/IP = **DTLS 1.2+**. Multiple SA are an option because message payloads are unencrypted.

- **Level 5: Authenticated end-to-end encryption**\
  Payload encryption of some or all messages. Message header unencrypted. The FD can authenticate the SA and the SA is able to authenticate the FD. Encrypted communications are only possible with a single SA. Messages are encrypted/decrypted by FDs and SAs, allowing them to pass through otherwise unsecured networks and Brokers. This allows for simpler network setups while providing reassurance of privacy and authenticity.
