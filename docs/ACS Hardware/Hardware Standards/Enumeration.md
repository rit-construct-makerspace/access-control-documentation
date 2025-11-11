# Enumeration

## Overview

Upon the Core initializing or after the Core commands a hard shutdown of all devices on the bus, all devices must be re-enumerated. Enumeration is used by the Core to understand the type, limitations, and needs of devices on the bus. It is also how all devices receive their unique IDs for CAN FD messaging. 

The geneal process for enumeration is as follows;

### Step 1: Branch Loop

The first step in enumeration is determining that a branch is properly terminated. This ensures that the CAN bus can be used for the following steps. 

*Note* : If a Core implements multiple HD15 ports, it must have a way to isolate CAN on just the HD15 it is performing a Branch Loop test on. It performs the following process on each HD15 port in a deterministic order.

This is started by the Core pulling low the Voltage Sag (VS) pin on its HD15 connector. The next device connected will see this, and pull its VS pin on the other HD15 low. When a device pulls VS low, it also pulls the interrupt line low for 100-250mS, to indicate to the device(s) upstream that there is another device present. This process repeats until one of the following;

* An LT or Router is reached, so the line is definitely sealed.
* An OT does not hear an interrupt for >500ms after pulling VS low, indicating there is not another device downstream. It shuts down that HD15 port and terminates the bus. 
* A PT device does not hear an interrupt for >500ms after pulling VS low, indicating there is not another device downstream. Since a PT device cannot terminate the bus, it asserts interrupt low for >1 second. This indicates to the Core that the bus could not be closed, and it should report as an error. 

Once the device on the end of the branch is reached, it informs the Core via CAN message *BRANCH-DONE*. If the device is a Router, it notifies the Core of this in the message, and then the Router begins Branch Loop testing on its downstream (i.e. not including the Core) branches. **TODO** finalize how the Router plays into this.

### Step 2: CAN Enumeration

After a deployment has been confirmed to be properly terminated, The Core can begin the enumeration process. The Core de-asserts the VS pin to the first device, which then triggers the device to send its device information. See [Enumeration Information](#Enumeration-Information) for more information. After all information is sent to the Core, the Core responds with *SET-ADDR* to give the device its new address for future CAN communication. Upon recieving this message, the device de-asserts its VS pin to the next device, and the process repeats. 

When the terminator is reached, the Core will know this due to the serial number matching what was sent in the previous *BRANCH-DONE* message. 

### Step 3: Bus Release

When the Core has repeated the Branch Loop and CAN Enumeration routine for every branch of the deployment, it can release the devices to normal operation with *CAN-START*. The Core should also begin pulsing the heartbeat at this time. 

## Hot-Plug

This standard does not officially support the hot-plugging of devices, but there are situations where a device may join the bus after enumeration (slow to boot, crashed and is restarting, etc.). 

A device can determine if it is joining an active bus by monitoring the heartbeat pin. If it is pulsing once or more per second, the bus is currently active. The device can discern the bus frequency by the heartbeat frequency, which are related 100,000:1. So for instance, a device detecting a 4Hz heartbeat knows the bus is running at 400KHz. Once matching bus frequency, the device can send a *NOT-ENUM* message to be enumerated by the Core.

If the device activates and sees the heartbeat pin held high, that means the entire deployment is in a suspended or startup state, and it should wait for enumeration. If the hearbeat starts pulsing before the device is enumerated, it may join the bus and use the *NOT-ENUM* message as above.

## Enumeration Information

The following is the order of messages sent in enumeration: 

**TODO**