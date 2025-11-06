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

After sending the message, the Core will go to this new frequency and iterate through all addresses with the *Ack* message to ensure all devices made the change. If they did not, the Core will use *Can-Freq* to return to the original frequency. 

## Message Definitions

Messages are defined sequentially, and given a human-readable name generally takeing the format of "Word" or Word1-Word2". 

### 0x0: Ack

Generic message for acknowledging other messages. Can be sent without a payload if only being used for the ack bit. Should contain message ID of message being responded to if an acknowledge.

This message cannot be targeted at 0x0 (all).

### 0x1: Coll-Enum

Initiates [Collision Enumeration](./Enumeration.md#id-collision-enumeration). Sent by the Core, targeted at 0x0 (all). No payload.

### 0x2: Tgt-Enum

Used for [Targeted Enumeration](./Enumeration.md#targeted-id-enumeration). Sent by the Core, targeted at 0x0 (all). 

* Bytes 0-8: Intended recipient unique ID
* Byte 9: New Device Address

### 0x3: Get-Addr

Used to get a device's address based on unique ID. Sent by the Core, targeted at 0x0 (all).

* Bytes 0-8: Intended recipient unique ID.

### 0x4: Get-UID

Used to get a device's unique identity number based on address. Cannot target 0x0. Sent with no payload.

### 0x5: Not-Enum

Tells any non-enumerated device to assert interrupt. Sent by the Core, targeted at 0x0 (all).

### 0x6: CAN-Start

Tells all devices they can begin normal use of the CAN bus, sent by the Core targeted at 0x0 (all) after enumeration completes.

### 0x7: CAN-Stop

Tells any devices they must stop using CAN normally, and removes all enumeration data. Targeted at 0x0 (all) sent by the Core.

### 0x8: Dev-Type

Returns the [device type](./Device%20Classifications.md) as a string. Can only be targeted.

### 0x9: Dev-Name

Returns the device's friendly name, a human-readable name for the device. Can only be targeted.

### 0x10: Mfg-Name

Returns the device manufacturer's friendly name, as a human-readable string. Can only be targeted.

### 0x11: Web-Site

Returns a website for the device, company, etc.. Can only be targeted.

### 0x12: Can-Freq

Returns the current and maximum frequencies of a device. Can only be targeted.

* Bytes 0-2: Current frequency (in Hz)
* Bytes 3-5: Maximum supported frequency (in Hz)

### 0x13: Power-Info

Gets information on a device's power system. Can only be targeted.

* Byte 0: Power Role
    * Bits 0, 1: Supply Role
        * 0: Power Consumer
        * 1: Optional Power Provider, not Currently Providing
        * 2: Optional Power Provider, Currently Providing
        * 3: Inherent Power Provider
    * Bits 2-4: Provider Type
        * 0: (Not a supplier)
        * 1: USB 5V Input
        * 2: USB >5v Input
        * 3: Power Harvesting from equipment
        * 4: Independently powered
        * 5: Battery Powered
    * Bits 5-7: Current Provider State
        * 0: (Not a provider)
        * 1: Below bus voltage
        * 2: At bus voltage, not providing
        * 3: At bus voltage, providing current (>200mA)
        * 4: Output in error state (overcurrent, overtemp, etc.)
* Byte 1: Current Bus Voltage, in 50mV increments, offset by 2 volts (i.e. 255 = 12.8 + 2 = 14.8 volts).
* Byte 2: Average current (Consumed/Provided) in the last 5 seconds, in 20mA increments (i.e. 255 = 5.1 amps), 0 if not capable of measurement.
* Byte 3: Maximum current the Provider can provide in its current configuration, 0 if not Provider.
* Byte 4,5: Standard power level of Consumer, 0 if not Consumer. In increments of 0.15 watts (i.e. 255 = 38.25 watts)
* Byte 6: Current set power level of Consumer, 0 if not Consumer.
    * 1: Extra Power 200%
    * 2: Extra Power 150%
    * 3: Standard Power
    * 4: Reduced Power 75%
    * 5: Reduced Power 50%
    * 6: Reduced Power 25%
    * 7: Minimum Power 0.75W
