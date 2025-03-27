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

These ratings mean the relay (and therefore the AC Power Switch) should be able to handle just about any device you can plug into a standard outlet. This makes integration incredibly easy, simply install it inline with the power plug for the equipment and you're set!

### Example Deployment: Powermatic Drill Press

TODO

## Contactor Switch

The Contactor Switch is intended for interrupting power on larger pieces of mains-powered equipment. Instead of switchng power directly, the Contactor Switch is meant to interface with a contactor that then switches power for the equipment. This can be a contactor installed specifically for ACS, but in most applications it is interfacing with a contactor that already exists in the equipment's control circuitry.



## KMX Switch

## Signal Relay Switch

## USB Hub Switch
