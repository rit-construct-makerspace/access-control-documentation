# KMX Switch

Version: V2.0.1

The KMX Switch is a variant of the [Signal Relay Switch](./Signal%20Relay%20Switch.md), specifically designed for use with ProtoTrak KMX machine controllers. The KMX Controller has a DE-9 plug on the back labeled "Euro E-Stop", which is shorted with multiple jumpers for regular operation. Interrupting these jumpers puts the controller into an emergency stop condition.

The KMX Switch simply implements the Signal Relay Switch to a DE-9 connector for direct connection to the KMX controller with much cleaner wiring. Power is provided from one of the USB-A ports on the back of the controller.

By putting the equipment into an emergency stop state completely stops all usage, but does not wipe critical values stored in RAM, such as tool settings, the current program, or the XY position of the machine. 