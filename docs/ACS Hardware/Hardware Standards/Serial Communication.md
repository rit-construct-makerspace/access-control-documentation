# Serial Communication

To permit more verbose communication between devices, ACS Interface Standard 3.0 includes a CAN FD shared bus. The CAN FD bus cannot supersede other sources of truth in the system, such as the *Access* signal. 

## Message ID Formatting

CAN FD payloads have a 29 bit message ID, that both identifies the message and determines priority in arbitration. For messaging in ACS, the address is used to both identify the message and indicate to and from whom the message is from or to, respectively.

Devices are given a 6 bit address during enumeration, that is used to identify the device in messages. Address 0x00 is reserved for messages with no specific target (intended for anyone or everyone), and address 0x01 is always reserved for the Core. Routers and devices that are not yet enumerated use the address 0x00 to identify themselves.

The first 6 bits of the message ID represent the "to" section, what device the message is intended to go to. The following 6 bits of the message ID represent the "from" section, what device is sending the message.

The next 16 bits represent the actual message type, which is drawn from a pre-defined list of types that a device can understand. Types are defined sequentially, so a device can quickly check if a message is outside of its observable range. 

The last bit is an acknowledgement bit, if set to 1 the recipient of the message should reply back to indicate a message receipt. If the message type is something that already would require a response (e.g. asking a device for its current state), a separate acknowledgement message is still needed. 

## Communication Frequency

On initialization, reset, etc., devices always connect to the bus at a baud rate of 100Kbps. After enumeration, the Core may define a new baud rate if all attached devices report they support it, using the *Can-Freq* message. 

After sending the message, the Core will adjust its Heartbeat frequency, go to this new frequency, and iterate through all addresses with the *Ack* message to ensure all devices made the change. If they did not, the Core will use *Can-Freq* to return to the original frequency, and revert the change to the Heartbeat.

## Message Definitions

Messages are defined sequentially, and given a human-readable name generally takeing the format of "Word" or Word1-Word2". 
