# Enumeration

## Overview

Upon the Core initializing or after the Core commands a hard shutdown of all devices on the bus, all devices must be re-enumerated. Enumeration is used by the Core to understand the type, limitations, and needs of devices on the bus. It is also how all devices receive their unique IDs for CAN FD messaging. 

Until enumeration is complete, devices cannot use the CAN bus as they normally would. 

## Unique Device ID

All ACS devices need a globally unique ID. This ID is a 64 bit value, that is used as a long-hand way to identify a device until the Core provides it a 6 bit address. The unique ID can also be used for deployment integrity checks, tracking of hardware lots, etc.

There is no centralized standard for the creation of ACS Unique Device IDs, rather a standard is built on pre-existing sources of uniqueness inherent to the hardware;

* **MAC-Based Unique ID** : If the device has a MAC address, it can be extended to a 64 bit unique identifier by adding 0xFFFE between the OUI and extension identifier. Devices that have multiple MAC addresses should use the lowest-value internet MAC for this.

* **Flash-Based Unique ID** : Most flash memory developed by Winbond and similar implement a JEDEC-compliant 64 bit unique ID. This will be 64 bits long by default, and can be used directly. 

* **Microcontroller Serial Number** : Many microcontrollers have a unique ID that can be used for this purpose. This is only permitted if the serial number is already 64 bits.

* **OneWire Address** : OneWire devices are identified with a 64 bit unique ID, that can be used directly. 

## ID Collision Enumeration

The standard method of enumeration is by ID Collision. This enumeration scheme takes advantage of the natural CAN arbitration. 

To begin this process, the Core sends the *Coll-Enum* message, targeted at 0x00. Upon receiving this message, all devices in the network begin attempting to transmit a CAN message containing their full unique ID in the first 8 bytes, and device priority as the 9th byte, with the message ID as the 29 LSBs of the ID. If a collision occurs, the higher-number ID will lose arbitration and attempt again later. The Core will keep track of the incoming full IDs, to use in the next step. Once sufficient quiet time has passed, the Core can assume every device has sent their ID.

## Targeted ID Enumeration

Once the Core has a list of all device unique IDs from the ID Collision Enumeration step, it will begin enumerating the devices by sending the *Tgt-Enum* message, targeted at 0x00, but containing the 64 bit address of the device that the Core wishes to address, and including the device's new address. The Core determines the order to assign device addresses based on the self-reported device priority from ID Collision Enumeration. 

Alternatively, if the Core thinks it knows the unique IDs of all devices in the deployment, it can skip ID Collision Enumeration and come directly to Targeted ID Enumeration.

After assigning an address to a device, the Core will conduct a series of messages more with the device to determine the following;

* Device type, name, manufacturer (for deployment composition reporting)
* Desired power draw or maximum power consumption (for power allocations)
* Current firmware version and URL to check for new firmware (for OTA updating)
* Maximum CAN frequency (for higher speed operation)

## Missed Device Detection

If a device is somehow not enumerated, the Core catches this when it sends the *Not-Enum* message, targeted at 0x00. This prompts any unenumerated device to assert the interrupt pin, and the Core then begins ID Collision Enumeration again to find it. Devices that have already been assigned an address will not participate in this enumeration. 

Once the Core has confirmed there are no unenumerated devices, it can use *CAN-Start* message to enable normal bus operation.

## Reverse Enumeration

In the event of the Core losing power, crashing, or similar, there is not always a need to re-enumerate devices on the bus. If the Core detects normal CAN activity within 3 seconds of boot, it knows it is (re)joining an already-running deployment. It can assert the interrupt then simply use the same messages from Targeted ID Enumeration to gather all data needed to resume regular operation. 

If the Core detects CAN activity that it cannot make sense of, it is likely at a different baud rate. In this case, the Core must assert shutdown to restart the entire deployment, and re-enumerate from scratch. 