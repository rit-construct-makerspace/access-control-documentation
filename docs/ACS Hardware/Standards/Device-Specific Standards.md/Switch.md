# Switch

The Switch is any device that physically interacts with the normal operation of a piece of equipment, restricting or mitigating access to it, and/or diminishing the operational capability of the device.

Switches must always be designed to alternative between 2 binary states; *active* (permitting normal operation of the device), and *inactive* (restricting normal operation of the device).

Switches must always be designed to fail into an inactive state within 1 second of losing power or the bus's "Access" signal being interrupted. This must be implemented in hardware, such that a malfunctioning microcontroller does not interrupt a device's ability to turn off. 

Switches may implement one or multiple *channels*, which refers to each output that can be independently controlled. For example, a switch with a DPDT relay only has 1 channel since the two throws cannot be independent. 

Each channel must have at least 2 status LEDs; one used for identification and one used for indicating the channel is active. 
* The identification LED must be able to be independently controlled from the channel being turned on, and can be commanded to blink by the Gateway.
* The active LED must always be active whenever the channel is active, implemented in hardware (e.g. the LED is parallel to the channel).

Switches may implement a test button, that will turn on the channel for as long as the button is held down. 