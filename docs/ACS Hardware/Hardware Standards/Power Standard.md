# Power Standard

One of the main goals of the Access Control Interface is to distribute power to all connected hardware via a shared bus, without the need for additional power supplies or wiring. 

## Power Roles

Devices can have one of the following device roles when describing their power functionality;

* **Power Consumer** (PC) : This is a device that has no means of self-powering or supplying power to the bus, and is fully reliant on power from other components.

* **Externally Powered Consumer** (EC) : This is a device that is powered independently of the ACS deployment, but for whatever reason cannot provide power to the deployment. For example, components with a battery backup can continue to operate if the ACS deployment loses power, but do not have to power the rest of the deployment. ECs must respect power limits when/if powered from the deployment, but may be designed to require any external power deemed fit for their application.

* **Dedicated Power Provider** (DP) : This is a device whose sole purpose is to inject power into an ACS deployment.

* **Inherent Power Provider** (IP) : This is a device that can provide power to the deployment, and should always be capable of doing so as part of its intended functionality. For example, an AC power switch should always have a connection to AC power to harvest for the deployment, if there is no AC power it is not capable of doing anything anyway.

* **Optional Power Provider** (OP) : This is a device that can provide power to the deployment, but is not inherent to the device's intended functionality.

## Debug Power

Devices, no matter their type or role, are permitted to have a power input that powers the internal circuitry of the device without the use of the bus. This debug power must not reverse-feed onto the bus. 

There are no standards or conventions for the voltage, current, or physical connector medium for debug power. It is at the discretion of the hardware developer to make such decisions, as well a decisions related to protection circuitry, if needed, for this input.

If both bus power and debug power are present, the device should prioritize use of the bus power. Optionally, a device may have a means of instead choosing to prioritize debug power temporarily, but the default function must remain prioritizing bus power.

## Power Provider Requirements

Any device providing power to the deployment must be able to provide a continuous 6.5 watts of power across all ambient conditions and device states. Power must be provided at a nominal voltage between 5 and 12 volts, +/- 5%. Bus voltage is allowed to change during operation, so long as they keep within this range. 

Power providers are required to provide the highest possible voltage within the range, to minimize transmission losses. While not mandated by specification, devices should attempt to provide a nominal 5, 9, or 12 volt supply as opposed to any intermediate voltages, to enable passive OR-ing of like-voltage supplies.

Power providers are not permitted to provide more than 3 amps to the bus.

All power providers must connect to the bus via an always-blocking ideal diode, that can withstand reverse voltages in excess of 18 volts. This ideal diode must be implemented in a way that does not cause issues with passively OR-ing all power providers on the bus together. Current limiting or overcurrent disconnect must be implemented in such a way as to prevent damage to the power providing circuitry in the event of an overcurrent or short-circuit, and must be able to automatically recover within 10 seconds of such an event being removed from the output. 

Power providers must be implemented in a way that their output to the bus can be programatically disabled. 

Power providers must monitor their output voltage, and report it regularly.

Power providers must power any internal electronics from the power source independent of the 6.5 watt bus power minimum. **IP** devices may permanently connect the Consumer to their Provider, and then not draw any power from the bus. **OP** devices must prioritize their internal supply over bus power. 

## Power Consumer Requirements

*For the purpose of this section, the power-consuming half of a power provider that is bus-powered is treated as a power consumer.*

Power consumers must connect to bus voltage via a current-limiting connection, whose current limit is based on the current power state.Current limits must be within 10% of the negotiated level. In the event of an overcurrent, the device 

Power consumers must have minimum functionality any time bus voltage exceeds 4.5 volts, and must be able to withstand sustained input voltages in excess of 18 volts. 

Power consumers must monitor their input voltage to an accuracy of 0.1v across the acceptable bus voltage range.

### Power States

Power consumers can operate in one of the following power states;

* **Initialization** : Upon startup, devices are expected to enter an initialization power state. In this state, the device must use as little power as possible, not permitted to exceed 0.75 watts. In this state, a device must have all bus-facing communication interfaces operational, but is not required to perform any of its intended access control functions.

* **Standard Power** : During the enumeration, if power negotiations succseed the device enters standard power mode. This is the regular operational level of the device. 

* **Reduced Power** : During the negotiation, the Core can assign a lower-than-requested power level for the device. The device must operate at this reduced power level and perform its core functionalities as best as possible. The possible reduced levels are 75%, 50%, 25% of the device's standard power level, and a static 0.75 watts. If any of the fractions of the device's standard power level are less than 0.75 watts, they can be omitted and replaced with 0.75 watts. If the device cannot operate at the reduced power level, it should enter a soft shutdown state.

* **Extra Power** : If a device is operating in Standard Power, it can request permission from the Core to exceed its standard power level, up to a level of 150% or 200% the standard level. The device must return to standard power level as promptly as possible, and inform the Core once it has released the extra power. 

* **Soft Shutdown** : In this state, the device shuts down everything except for its microcontroller and any circuitry required to read the interrupt signal from the Core. The device will exit this state and go to an initialization power state upon receiving an interrupt. The device does not have to current limit to a level below the 0.75 watts of initialization state in this level. 

* **Hard Shutdown** : In this state, the entire device is deactivated. This state can only be exited with the reset line from the Core, or a complete power cycle of the deployment.

## Bus Voltage Monitoring

In a distributed and modular system, where there are no guarantees as to the current-carrying abilities of the interconnecting cables, it is imperative that all devices in the deployment monitor for voltage drops that could indicate a damaged or overburdened cable. 

No less frequently than once a second, all devices in the deployment must report their voltage. For consumers, this is the measured voltage on the input of their device. For providers, this is the measured output voltage. All measurements should take place on the bus-side of any protection circuitry, to account for the drop of said circuitry.

In the event that any device hears a reported voltage that is 0.5v or more higher than their measured voltage, the device must immediately enter a soft shutdown, and report the potential excessive voltage drop to the Core.

