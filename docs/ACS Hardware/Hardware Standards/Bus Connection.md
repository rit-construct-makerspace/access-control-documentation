# Bus Connection

All ACS devices use the same standard connection, implemented physically in a DE-9 connector. Devices always have a female plug, and male-male straight-through cables are always used to interconnect devices. Cables must have a shield that fully extends between the two ends, and appropriate strain relief built into the connector head. While the standards do not specify a wire gauge or material, larger cables, such as 26AWG or 24AWG, are preferred and will permit the deployment to fully utilize the current a power provider is permitted to prove the deployment. See Amphenol *CS-DSDMDB09MM* as an example of an acceptable cable.

## Pinout

### 1. Access

Being an access control system, one of the most important signals is the one that determines if the equipment a deployment is connected to should be accessible currently. 

The Access signal is a 5v logic level, driven with a >20mA source/sink at the Core. No other device is permitted to drive this signal. Devices reading this signal may not source/sink more than 0.5mA into this connection.

### 2. CAN Low

Negative side of CAN differential pair.

### 3. Ground

Main system ground, all signals and power are referenced to this potential. Must be connected to other ground pin as soon as possible within the device.

### 4. Power

Main system power rail. Must be connected to other power pin as soon as possible within the device.

### 5. Shutdown

This signal commands all power-consuming circuitry to enter a hard-shutdown state. This signal is normally high, at a 5v logic level. This signal can only be driven by the Core. All devices must implement the use of this signal in hardware. This signal can be held low by the core indefinitely to act as a shutdown pin, or driven low for more than 500 milliseconds then high to reset all devices.

### 6. Ground

Main system ground, all signals and power are referenced to this potential. Must be connected to other ground pin as soon as possible within the device.

### 7. CAN High

Positive side of CAN differential pair.

### 8. Interrupt

Open-drain line that can be used by non-Core devices to notify the Core and all other devices of an urgent communication. This line is pulled up to 5v by the Core, with a nominally 1kΩ resistor. Devices must assert the interrupt low with a >10mA sink to ground. Devices may not add appreciable current to this pin when not interrupting. 

All devices are required to implement the interrupt line. Devices must immediately cease all communication if the interrupt is asserted and they are not the device that asserted it. Only the Core and the interrupting device may begin communications when an interrupt is asserted. The Core may also assert interrupt to cease all communication activity.

When in an interrupted state, a device is still expected to respond to any messages it receives specifically addressed to it.

When in an interrupted state, devices should still listed to messages on the bus, and act accordingly.

### 9. Power

Main system power rail. Must be connected to other power pin as soon as possible within the device.

### Shield

The shield is connected to ground at the Core. All other devices cannot interact with the shield. Devices with multiple DE-9 connections must connect all shields together. 

## Bus Isolation

When a device is in a powered-off or hard shutdown state, it must electrically disconnect from all signals on the bus. This includes releasing the interrupt, if asserted. A device cannot connect to the bus unless its microcontroller or similar is properly executing code, and a hardware-generated power-good signal is asserted, such as from a regulator.

## Plug Detection

Some devices need to be able to detect if a device is connected, such as a Router or an OT. Plug detection must be implemented by attaching a weak pull up, no stronger than 10kΩ at 5 volts, on either pin 4 or 9. The other can be read for this weak pull up.

Devices must electrically connect pins 4 and 9 at the connector, so the lack of a short between these pins can be treated as there not being a device downstream.