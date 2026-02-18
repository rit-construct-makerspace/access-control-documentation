# Enumeration

## Overview

Upon the Core initializing or after the Core commands a hard shutdown of all devices on the bus, all devices must be re-enumerated. Enumeration is used by the Core to understand the type, limitations, and needs of devices on the bus. It is also how all devices receive their unique IDs for CAN FD messaging. 

Enumeration is handled peer-to-peer down a deployment's branch, originating from the Gateway. The first step is discovery, the process is as follows;

* If the device is a line terminator, it has no role in enumeration and immediately becomes a branch termination.
* The upstream device reads its sag pin, to see if it is connected to a passive line terminator. IF so, become the branch termination.
* If no PLT detected, the upstream device asserts its sag pin in the downstream direction.
* The downstream device reads the asserted sag pin, and acknowledges by asserting the interrupt pin for at least 100mS. 
    * The downstream device now repeats this process for its downstream port, waiting at least 100mS after it de-asserts its interrupt pin before beginning.
* During all this, the upstream device does not de-assert the interrupt pin.
* If the upstream device does not hear an interrupt within 200mS, it becomes the branch termination.
* Once a device becomes designated as the branch termination;
    * If a passthrough device (i.e. cannot terminate the bus), it begins pulsing interrupt at a 4Hz, 50% duty cycle to indicate an unstable branch.
    * If an optional terminator, enable line terminations.
    * Communicates to the Gateway back up the branch with the *LINE-END* message.

Once all devices down a branch have been discovered, the Gateway begins the second step, fact-finding. 
* The Gateway de-asserts its sag pin to the first downstream device, and sends the CAN message *ENUM-INFO*. The device then responds with the information described in [Enumeration Information](#enumeration-information)
* Once all information has been received from the device, the Gateway uses the CAN message *SET-ADDR* to designate a CAN address for the device going forward.
* Lastly, the Gateway sends the CAN message *ENUM-NEXT*, indicating to the device it should release the sag pin of its downstream device.
    * If the enumerated device was the line terminator, it instead sends *ENUM-DONE*, to indicate to all devices on the bus it is safe to begin communication.
* This process repeats down the branch, with each device being enumerated, and releasing its downstream device for enumeration.

## Multiple Branches

If a Gateway has 2 or more independent branches, it may enumerate both branches in parallel. 

When a deployment has multiple branches split by Routers, it requires special considerations in enumeration.

## Enumeration Information

The following messages, continuing the following information, are transmitted during enumeration;