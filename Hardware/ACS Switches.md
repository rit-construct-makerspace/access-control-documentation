# ACS Switches

## Overview

The Switch is the portion of an ACS deployment that is responsible for directly interfacing with the equipment, somehow interfering with its normal operation.

Switches are also generally the component responsible for powering an ACS deployment, harvesting some power from whatever it is switching on/off in most applications.

## AC Power Switch

The AC Power Switch is the most-used Switch variant. It provides a standard NEMA 5-15R (US Wall Outlet) for plugging equipment into. Input power is provided through a NEMA C14 (Computer Power Cable) Receptacle. Power for the ACS system is drawn from the C14 as well. The power regulator and the NEMA 5-15R are on independent fuses, so an overcurrent on the equipment doesn't cut power to the ACS and vice-versa. 

The heart of the switch is an Xiamen Hongfa Electroacoustic HF165FD-G high-power relay. The relay is UL rated for:
* 40A 277VAC 40C
* 1HP 120VAC 40C
* 96LRA/30FLA 40C
* TV-8 125VAC 40C

The relay is wired in a normally-open configuration, so that a lack of control signal or an internal power fault results in a shutdown within 10 milliseconds. Only the live is interrupted, neutral and earth are always connected for safety purposes. 

These ratings mean the relay (and therefore the AC Power Switch) should be able to handle just about any device you can plug into a standard outlet. This makes integration incredibly easy, simply install it inline with the power plug for the equipment and you're set!

### Example Deployment: Powermatic Drill Press

TODO

## Contactor Switch

The Contactor Switch is intended for interrupting power on larger pieces of mains-powered equipment. Instead of switchng power directly, the Contactor Switch is meant to interface with a contactor that then switches power for the equipment. This can be a contactor installed specifically for ACS, but in most applications it is interfacing with a contactor that already exists in the equipment's control circuitry.

The Contactor Switch has 2 inputs and 2 outputs; an input live and neutral, and an output live and neutral. The live and a neutral inputs supply the internal regulator, and when the Switch is activated, the input live is connected to the output live connection. The input neutral and output neutral are always connected, and is there for ease of wiring.

Depending on what is easier for the deployment, the live and neutral wires can be flipped and the system works electrically. By convention though, if possible it is preferred to interrupt live. 

The system has 2 board-mounted fuses, independently protecting the switching output and the power supply, such that the failure of one does not take out the other or vise-versa. 

The Contactor Switch rated to operate at voltages up to 250VAC, and can switch sustained loads of up to 2A. The relay is UL rated for transients up to 8A at 250VAC, or motor of 1/10HP at 120VAC or 1/6HP at 250VAC.

### Example Deployment: SawStop Table Saw

TODO

## Signal Relay Switch

### Signal Relay Switch Variant: KMX Switch

The KMX Switch is a variant of the Signal Relay Switch, specifically designed for use with ProtoTrak KMX machine controllers. 

## USB Hub Switch
