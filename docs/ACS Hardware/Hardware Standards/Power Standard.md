# Power Standard

One of the main goals of the Access Control Interface is to distribute power to all connected hardware via a shared bus, without the need for additional power supplies or wiring. 

## Power Roles

Devices can have one of the following device roles when describing their power functionality;

* **Power Consumer** (PC) : This is a device that has no means of self-powering or supplying power to the bus, and is fully reliant on power from other components.

* **Externally Powered Consumer** (EC) : This is a device that is powered independently of the ACS deployment, but for whatever reason cannot provide power to the deployment. For example, components with a battery backup can continue to operate if the ACS deployment loses power, but do not have to power the rest of the deployment. ECs must respect power limits when/if powered from the deployment, but may be designed to require any external power deemed fit for their application.

* **Dedicated Power Provider** (DP) : This is a device whose sole purpose is to inject power into an ACS deployment.

* **Inherent Power Provider** (IP) : This is a device that can provide power to the deployment, and should always be capable of doing so as part of its intended functionality. For example, an AC power switch should always have a connection to AC power to harvest for the deployment, if there is no AC power it is not capable of doing anything anyway.

* **Optional Power Provider** (OP) : This is a device that can provide power to the deployment, but is not inherent to the device's intended functionality. For example, a signal relay switch with a barrel jack can optionally be plugged into a wall power adapter, but if not present it can operate from the deployment's power rail.

## Debug Power

Devices, no matter their type or role, are permitted to have a power input that powers the internal circuitry of the device without the use of the bus. This debug power must not reverse-feed onto the bus. 

There are no standards or conventions for the voltage, current, or physical connector medium for debug power. It is at the discretion of the hardware developer to make such decisions, as well a decisions related to protection circuitry, if needed, for this input.

If both bus power and debug power are present, the device should prioritize use of the bus power. Optionally, a device may have a means of instead choosing to prioritize debug power temporarily, but the default function must remain prioritizing bus power.

## Power Provider Requirements

Any device providing power to the deployment must be able to provide at minimum a continuous 6.5 watts of power across all ambient conditions and device states. 

Bus power is nominally defined as 24v, +/-1.5v. Devices may provide voltages as low as 7 volts onto the bus if they are not able to provide any voltage in excess of that.

Power providers are not permitted to provide more than 2 amps to the bus, and must actively limit or "circuit-break" current in at or below 2.5 amps from entering the deployment. Current consumed by the device itself may be excluded from this limit.

All power providers must connect to the bus via an always-blocking ideal diode, that can withstand reverse voltages in excess of 30 volts. This ideal diode must be implemented in a way that does not cause issues with passively OR-ing all power providers on the bus together. Current limiting or overcurrent disconnect must be implemented in such a way as to prevent damage to the power providing circuitry in the event of an overcurrent or short-circuit, and must be able to automatically recover within 10 seconds of such an event being removed from the output. 

Power providers must be implemented in a way that their output to the bus can be programmatically disabled, and defaults to on. 

Power providers must be able to monitor the current going to the bus with an accuracy of 10% or better.

**OP** devices must be able to detect when they are in a power-providing state.

## Power Consumer Requirements

*For the purpose of this section, the power-consuming half of a power provider that is bus-powered is treated as a power consumer.*

Power consumers must connect to bus voltage via a switched current-limiting connection, See *Bus Connection, Heartbeat/Shutdown* for more information.

Power consumers must have minimum communication functionality any time bus voltage exceeds 7.5 volts, although they are not required to fully operate at that low a voltage level if they are not capable of doing so. For example, a signal relay switch must begin monitoring CAN and other bus signals once the voltage rises to 7.5v, but can require a voltage greater than 14 volts to energize its relay coils.

Power consumers must operate normally if their input voltage is between 22 and 27 volts, and must be able to survive a continuous 28 volts on their input.

Power consumers that pass bus voltage through them, such as to other downstream devices or to an IntraBus device, must have active current limiting protections such as a fuse, PTC resettable fuse, or eFuse, prevent current flow in excess of 2.5A across the device. The protection must be designed to activate within 10 seconds of a 5A (200%) current flow.

Power consumers must monitor their bus voltage, even if they do not make use of bus voltage, to an accuracy of 0.1v or better. If a device requires the aforementioned fuse, the bus voltage monitoring must be on the opposite side of the fuse from where power is supplied or consumed from, allowing the device to detect a blown fuse on itself or the next downstream device. 

## Voltage Shift Detection

Since an ACS deployment will be comprised of multiple power-consuming and power-providing devices connected by long, relatively high-resistance cables, there is a significant risk of voltage sag or shifting across the deployment. 

The implications of this shift on bus signals is already covered in *Bus Connection, Ground Shift Consideration*.

In the least-ideal deployment possible, all power providers are on one side of a very long cable from all the power consumers on the other side. There would be a significant amount of current going through the one device in the center, 

## System Commanded Shutdown