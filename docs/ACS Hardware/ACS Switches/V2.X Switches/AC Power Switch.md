# AC Power Switch

Version: V2.1.1

The AC Power Switch is the most-used Switch variant. It provides a standard NEMA 5-15R (US Wall Outlet) for plugging equipment into. Input power is provided through a NEMA C14 (Computer Power Cable) Receptacle. Power for the ACS system is drawn from the C14 as well. The power regulator and the NEMA 5-15R are on independent fuses, so an overcurrent on the equipment doesn't cut power to the ACS and vice-versa. 

The heart of the switch is an Xiamen Hongfa Electroacoustic HF165FD-G high-power relay. The relay is UL rated for:

* 40A 277VAC 40C
* 1HP 120VAC 40C
* 96LRA/30FLA 40C
* TV-8 125VAC 40C

The relay is wired in a normally-open configuration, so that a lack of control signal or an internal power fault results in a shutdown within 10 milliseconds. Only the live is interrupted, neutral and earth are always connected for safety purposes. 

These ratings mean the relay (and therefore the AC Power Switch) should be able to handle just about any device you can plug into a standard outlet. This makes integration incredibly easy, simply install it inline with the power plug for the equipment and you're set!

## Example Deployment: Powermatic Drill Press

We mounted the Power Switch on the column of the drill press.

![ACS Power Switch](assets/acs%20power%20switch.jpg)