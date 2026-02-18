# Switch

The Switch is any device that physically interacts with the normal operation of a piece of equipment, restricting or mitigating access to it, and/or diminishing the operational capability of the device.

Switches must always be designed to alternative between 2 binary states; *active* (permitting normal operation of the device), and *inactive* (restricting normal operation of the device).

Switches must always be designed to fail into an inactive state within 200 milliseconds of losing power or the bus's "Access" signal being interrupted. 

Switches may implement one or multiple *channels*, which refers to each output that can be independently controlled. For example, a switch with a DPDT relay only has 1 channel since the two throws cannot be independent. Switches that implement multiple channels must AND the channel's signal with the Access signal using purpose-made AND logic gates. 

Switches must have 2 status LEDs, indicating;
* ACCESS: Illuminated if the Access signal is high
* POWER: Illuminated if the proper power rail is present.

Switches that implement multiple channels must implement an LED for each channel in addition to the 2 above.

Switches may implement a test button, that will turn on the channel for as long as the button is held down. 