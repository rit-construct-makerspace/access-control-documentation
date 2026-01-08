# Bus Connection

All ACS devices use the same standard connection, implemented physically in a HD15 (High Density D-Subminiature 15 Pin) connector. Devices always have a female plug, and male-male straight-through cables are always used to interconnect devices. Cables must have a shield that fully extends between the two ends, and appropriate strain relief built into the connector head. While the standards do not specify a wire gauge or material, larger cables, such as 26AWG or 24AWG, are preferred and will permit the deployment to fully utilize the current a power provider is permitted to provide the deployment. See Amphenol *CS-DSDHD15MM0-005* as an example of an acceptable cable.

**WARNING** : HD15 cables, connectors, plugs, etc. must not be colored blue in any way, to avoid confusion with a VGA video cable or connector. 

## Pinout

### 1, 2, 6, 11 : Power

Main system power rail. Must be connected to other power pins as soon as possible within the device.

### 5, 9, 10, 14, 15 : Ground

Main system ground, all signals and power are referenced to this potential. Must be connected to other ground pins as soon as possible within the device.

### 7 : Voltage Sag Detect

The Sag pin is pulled up to 5 volts and down to ground with a 1k resistor inside each device.

The primary purpose of this pin is to monitor the current flow between devices, to watch for overcurrent events. See *Power Standards, Voltage Shift Detection* for more information.

During enumeration, this signal is asserted low as a means of peer-to-peer chained communications, to determine bus integrity and enumeration ordering.

### 12 : Access

Access is also known as the Deadman signal, it is a representation of the immediate, current state of any access-controlling Switches in the system.

The Access signal is a 5v logic level, driven with a >20mA source/sink at the Gateway. No other device is permitted to drive this signal. Devices reading this signal may not source/sink more than 0.25mA into this connection.

### 3: CAN Low

Negative side of CAN differential pair.

### 8 : CAN High

Positive side of CAN differential pair.

### 4: Heartbeat/Shutdown

This signal serves 3 primary purposes;

* Inform devices if the bus is active/suspended.
* Inform devices of the current CAN frequency.
* Shutdown all devices on the bus as needed.

This pin is driven to a 5v logic level by the Gateway, and must be monitored by all devices.

In normal operation, the Gateway will generate a square wave with a 50% +/- 10% duty cycle on this pin. Devices can determine the CAN bus frequency by the frequency of the Heartbeat, with a 1Hz heartbeat correlating to a 100KHz data rate, a 10Hz Heartbeat correlating to a 1MHz data rate, etc., permissible in increments of 50Khz. 

The Heartbeat can also be used to suspend CAN bus operation. If the Gateway drives the pin with a duty cycle lower than 30%, devices should understand that to mean no CAN activity is permitted and put their CAN transceivers into a recessive state. There is no need to read the CAN bus when the Heartbeat pin indicates it suspended.  

If the Gateway asserts the Heartbeat pin to 5v or ground for more than 1.5 seconds, all devices in the deployment should shut down their power systems. This functionality must be implemented using hardware exclusively, and not rely on code execution. See *Power Standard, System Commanded Shutdown* for more information. 

### 13 : Interrupt

Open-drain line that can be used by non-Core devices to notify the Core and all other devices of an urgent communication. This line is pulled up to 5v by the Core, with a nominally 1kΩ resistor. Devices must assert the interrupt low with a >20mA sink to ground. Devices may not add appreciable current to this pin when not interrupting. 

All devices are required to implement the interrupt line. Devices must immediately cease all communication if the interrupt is asserted and they are not the device that asserted it. Only the Core and the interrupting device may begin communications when an interrupt is asserted. The Core may also assert interrupt to cease all communication activity.

When in an interrupted state, a device is still expected to respond to and/or act on any messages it receives specifically addressed to it.

### Shield

The shield is connected to ground at the Core. All other devices cannot interact with the shield. Devices with multiple DE-9 connections must connect all shields together. 

## Bus Isolation

When a device is in a powered-off or hard shutdown state, it must electrically disconnect from all signals on the bus. This includes releasing the interrupt, if asserted. 

Devices are permitted to draw an insignificant amount of power from the bus when in a powered-off or hard shutdown state for the purpose of maintaining any isolation circuitry, or circuitry that ensures the device stays in a determinate, safe state during shutdown and startup.

Devices are permitted to weakly load the Heartbeat pin, with no more than a 40kR connection, such that they do not shutdown if not connected to a Gateway. 

The sag pin's 1k pulldown to ground should always stay connected, and the 1k pullup to the unpowered 5v rail may remain connected. 

## Ground Shift Consideration

Due to the wired, distributed nature of an ACS deployment, it is not only possible but probable that the common ground reference will become offset across devices, a phenomenon known as ground shift. 

For CAN, the ISO standard already calls out a voltage range of -2v to 7v common mode to allow for ground shifts. While this is acceptable, it is recommend to use a transceiver with an even greater rejection.

For digital signals, devices must be able to read any voltage between 3.1v and 7.1v relative to its local ground as a logical high. Voltages between -1.7v and 1.7v must be read as a logical low. 

Ground shift considerations are not necessary on the sag pin, as it is designed to read such ground shifts.

## IntraBus Connection

Some devices may want to optionally extend the bus signals to other connected devices in a standard, modular way. This can be achieved with IntraBus. 

*NOTE: IntraBus must only be followed when passing signals directly from the bus, and does not apply to generic modular devices.*

The IntraBus standard defines how a bus-connected device, known as the "host", may extend bus signals through a per-device standardized interface, to allow for more complex add-ons without the complexity of an entire additional bus-attached device. 

A host device may have any number of IntraBus devices, but each IntraBus device needs an independent connector, power and signal switching, etc.. IntraBus devices cannot be daisy-chained.

IntraBus connections are at the discretion of design engineers for each device, there is no physical connector standardization for the IntraBus. 

### IntraBus Power

There is no standard for what voltage(s) are provided to IntraBus devices, only that;

* All power for the IntraBus device comes from the host device. 
* The host device must be able to shut off all power going to the IntraBus device.
* All power switching to the IntraBus device must happen at the high side, grounds are always connected.
* The power providing circuitry for the IntraBus connector starts in the off position when the host is powered on. 
* The host must limit current going to an IntraBus device.
* The IntraBus device drawing maximum current must not negatively impact the host device's regular operation. 
* The combined current draw of the host device and the IntraBus device cannot exceed the maximum power rating of a standard device.
    * Exemptions are made for inherently powered devices or devices that power the IntraBus device independent of bus power.

### IntraBus Signals

The IntraBus implementation must default to isolating the IntraBus device from the bus signals, and can only be connected when the power to the IntraBus device is activated and receiving power. This can be implemented on the host side, with all signals not present on the connector until power is applied, or on the device side, with the device not caring about or interfering with signals when unpowered.

Signals from the bus to the IntraBus connector cannot be re-driven or otherwise modified by the host device, with the exception of passing through any bus isolators needed to comply with power-down safety. 

The IntraBus device interacts with all bus signals (CAN, Interrupt, Access, etc.) the same as a normal ACS device, with the requirements and limitations thereof. 

The total stub length on CAN signals used in IntraBus must be no more than 25 centimeters, including the length of any wire or cable connecting the host and the device. 

### IntraBus Plug Detection

IntraBus devices have their Sag pin replaced with a IntraBus Detect pin. This pin is internally connected to ground in the IntraBus device. A host device must pull this pin up to detect the presence of an IntraBus device. 

## Router Re-Driving

When a router splits the bus, it should re-drive all signals to the new branch, to minimize extreme ground shift and voltage sag on very large deployments with lots of branches.

For CAN, this is simply achieved by receiving messages on one transceiver, and re-transmitting them on another. Routers may re-transmit all messages across all branches, but is preferred if the Router keeps track of what devices are on what branch, and only routes pertinent messages down that branch to reduce congestion. 

For digital signals originating at the Gateway, the Router must re-drive the signals using a hardware-only approach, such that a misbehaving microcontroller does not interrupt signals. 