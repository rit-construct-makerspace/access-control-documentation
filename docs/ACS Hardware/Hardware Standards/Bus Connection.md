# Bus Connection

All ACS devices use the same standard connection, implemented physically in a HD15 (High Density D-Subminiature 15 Pin) connector. Devices always have a female plug, and male-male straight-through cables are always used to interconnect devices. Cables must have a shield that fully extends between the two ends, and appropriate strain relief built into the connector head. While the standards do not specify a wire gauge or material, larger cables, such as 26AWG or 24AWG, are preferred and will permit the deployment to fully utilize the current a power provider is permitted to provide the deployment. See Amphenol *CS-DSDHD15MM0-005* as an example of an acceptable cable.

**WARNING** : HD15 cables, connectors, plugs, etc. must not use blue plastic, to avoid confusion with a VGA video cable or connector. 

## Pinout

### 1, 2, 6, 11 : Power

Main system power rail. Must be connected to other power pins as soon as possible within the device.

### 5, 9, 10, 14, 15 : Ground

Main system ground, all signals and power are referenced to this potential. Must be connected to other ground pins as soon as possible within the device.

### 7 : Voltage Sag Detect

This pin is pulled to the Power rail inside each device with a nominally 1kΩ resistor. 

In its primary use-case, measurement of the voltage difference between this pin and the Power pin can be used to calculate the voltage drop of the interconnecting cable between devices. 

During enumeration, this signal is asserted low as a means of peer-to-peer chained communications, to determine bus integrity and enumeration ordering.

### 12 : Access

Access is also known as the Deadman signal, it is a representation of the immediate, current state of any access-controlling Switches in the system.

The Access signal is a 5v logic level, driven with a >20mA source/sink at the Core. No other device is permitted to drive this signal. Devices reading this signal may not source/sink more than 0.25mA into this connection.

### 3: CAN Low

Negative side of CAN differential pair.

### 8 : CAN High

Positive side of CAN differential pair.

### 4: Heartbeat/Shutdown

This signal serves 3 primary purposes;

* Inform devices if the bus is active/suspended.
* Inform devices of the current CAN frequency.
* Shutdown all devices on the bus as needed.

This signal is by default pulled to 5v by the Core with a 1kΩ resistor. Devices may also pull it up to 5v with no stronger than 100kΩ. If in this high state for more than 1 second, that means that the bus is in an idle state (may be enumerating, may be the Core is dead, etc.), and devices should enter an idle state as well.

During regular system operation, this pin is a heartbeat, with a 50% duty cycle asserted by the Core. The frequency of the heartbeat correlates with bus frequency, so a device recovering from a power issue or similar knows what frequency to communicate. The bus frequency is 100,000x the heartbeat (i.e. a 400KHz bus is represented by a 4Hz heartbeat). 

If the signal remains low for greater than 1.5 seconds, devices must disconnect electrically from the bus and enter a shutdown state, until such a time as the pin returns to a logical high signal. This must be implemented in hardware and/or in a dedicated watchdog IC. This is used by the Core to trigger a restart and re-enumeration, or may be held low indefinitely to shutdown a system.

### 13 : Interrupt

Open-drain line that can be used by non-Core devices to notify the Core and all other devices of an urgent communication. This line is pulled up to 5v by the Core, with a nominally 1kΩ resistor. Devices must assert the interrupt low with a >20mA sink to ground. Devices may not add appreciable current to this pin when not interrupting. 

All devices are required to implement the interrupt line. Devices must immediately cease all communication if the interrupt is asserted and they are not the device that asserted it. Only the Core and the interrupting device may begin communications when an interrupt is asserted. The Core may also assert interrupt to cease all communication activity.

When in an interrupted state, a device is still expected to respond to and/or act on any messages it receives specifically addressed to it.

### Shield

The shield is connected to ground at the Core. All other devices cannot interact with the shield. Devices with multiple DE-9 connections must connect all shields together. 

## Bus Isolation

When a device is in a powered-off or hard shutdown state, it must electrically disconnect from all signals on the bus. This includes releasing the interrupt, if asserted. A device cannot connect to the bus unless its microcontroller or similar is properly executing code, and a hardware-generated power-good signal is asserted, such as from a regulator.
