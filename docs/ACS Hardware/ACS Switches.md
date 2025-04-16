# ACS Switches

## Overview

The Switch is the portion of an ACS deployment that is responsible for directly interfacing with the equipment, somehow interfering with its normal operation.

Switches are also generally the component responsible for powering an ACS deployment, harvesting some power from whatever it is switching on/off in most applications.

An example is given of each below Switch, but see "Example ACS Deployments" for more example of deploying these Switches.

For generalized information on selecting a Switch for your machine, see the page "Deploying ACS Hardware".

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

We mounted the Power Switch on the column of the drill press.

![ACS Power Switch](assets/acs%20power%20switch.jpg)

## Contactor Switch

The Contactor Switch is intended for interrupting power on larger pieces of mains-powered equipment. Instead of switching power directly, the Contactor Switch is meant to interface with a contactor that then switches power for the equipment. This can be a contactor installed specifically for ACS, but in most applications it is interfacing with a contactor that already exists in the equipment's control circuitry.

The Contactor Switch has 2 inputs and 2 outputs; an input live and neutral, and an output live and neutral. The live and a neutral inputs supply the internal regulator, and when the Switch is activated, the input live is connected to the output live connection. The input neutral and output neutral are always connected, and is there for ease of wiring.

Depending on what is easier for the deployment, the live and neutral wires can be flipped and the system works electrically. By convention though, if possible it is preferred to interrupt live. 

The system has 2 board-mounted fuses, independently protecting the switching output and the power supply, such that the failure of one does not take out the other or vise-versa. 

In the event of a power failure or similar, the Contactor Switch returns to a normally open state in under 8 milliseconds.

The Contactor Switch rated to operate at voltages up to 250VAC, and can switch sustained loads of up to 2A. The relay is UL rated for transients up to 8A at 250VAC, or a motor of 1/10HP at 120VAC or 1/6HP at 250VAC.

### Example Deployment: SawStop Table Saw

The SawStop logical circuitry is powered by the input power wired from L1 through A1 to the electronics. By interrupting this connection with the Contactor Switch, power is interrupted to the control electronics. The SawStop was found in testing to recover elegantly from power being cut, and will require the main blade switch to be cycled before it will start.

![SawStop Wiring](assets/acs%20sawstop%20wiring.png)

## Signal Relay Switch

The Signal Relay Switch is used to interrupt low-current signals in a wide variety of control loops. The Signal Relay can be used in more applications than a Contactor Switch, since it does not draw power from the connected circuit, thus not interfering with weak digital logic signals. Signal Relays Switches are commonly installed in series with door switches, emergency stops, cycle start buttons, etc., to disable a machine that isn't feasible to cut power to directly. 

As a result of the requirement to not draw power from the connected system, the Signal Relay Switch needs to be independently powered. 5v is supplied via a USB-B connector on the board from any available source. 

The Signal Relay Switch offers 2 independent but synchronized control channels, each with a normally open and normally closed contact. Each channel is rated for resistive loads of 2A at 30VDC or 0.5A at 125VAC.

If power is lost, the Signal Relay returns to the off state in 4 milliseconds.

### Example Deployment: HAAS TM-1P

The HAAS TM-1P is the largest CNC mill in our shop. It runs on a 440VAC 13A connection, much too large to interrupt power to. There is also not an embedded contactor that can easily be accessed to cut power. Cutting power is also not ideal, since that'd lose program parameters and other RAM-based settings as well as the machine's position if you remove your keycard. 

All HAAS machines produced after ~2015 have considerations for adding a robotic arm for automation, and part of that is a secondary E-Stop input on the control board. This Auxiliary E-Stop can be found at TB1-B, as indicated in the image below. In our application we connected pins 7 and 8 of TB1-B to the normally open connection of the Signal Relay Switch, such that when the ACS is unlocked, it released the E-Stop, but any other state (including removing ACS power) results in an E-Stop state. JP1 next to TB1-B also needs to be removed to enable this Auxiliary E-Stop.

For power, there is a standard 115VAC outlet inside the electrical cabinet of the TM-1P that we plugged a USB wall wart into. This outlet is energized any time the machine power is on. 

This implementation allows us to retain parameters and position of the machine, as well as not constantly cycle the power on and off, while still locking out all control over the equipment. It also ensures that a card (and therefore hopefully an operator) is present at all times, while also making it easy to step away for a minute or swap operators. 

![Haas Wiring](assets/acs%20haas%20wiring.webp)

[Image Source - haascnc.com](https://www.haascnc.com/service/troubleshooting-and-how-to/reference-documents/robot-integration-aid---ngc.html)

## KMX Switch

The KMX Switch is a variant of the Signal Relay Switch, specifically designed for use with ProtoTrak KMX machine controllers. The KMX Controller has a DE-9 plug on the back labeled "Euro E-Stop", which is shorted with multiple jumpers for regular operation. Interrupting these jumpers puts the controller into an emergency stop condition.

The KMX Switch simply implements the Signal Relay Switch to a DE-9 connector for direct connection to the KMX controller with much cleaner wiring. Power is provided from one of the USB-A ports on the back of the controller.

By putting the equipment into an emergency stop state completely stops all usage, but does not wipe critical values stored in RAM, such as tool settings, the current program, or the XY position of the machine. 

## USB Hub Switch

The USB Hub Switch is ideal for equipment that is operated by an attached computer, such as laser cutters, tethered 3D printers, CNC machines, and more.

The USB Hub Switch is a 4-port USB 2.0 hub, providing 4 USB-A ports. All ports are Hi-Speed, Low-Speed, and Full-Speed compatible, with an upstream Hi-Speed or Full-Speed port. The power to the 4 downstream ports is enabled/disabled by the switching mechanism, with a USB-compliant soft start ramp-up. 

This enables you to lock out any USB bus-powered device to stop users from interacting with a machine. Common examples are USB keyboards or mice, USB connections from a computer to the equipment, or the USB port used to load programs. 

### Example Deployment: Epilog Laser Cutter

The SHED has 3 Epilog Laser Cutters. While we could interrupt AC power to them, it would require running through the 60-second startup procedure every time someone wants to use the machine. It also meant that removing your keycard would immediately and irrecoverably stop the in-progress job. Since some laser cutter jobs can be in the realm of hours, and have multiple team members swap out to watch it, this would be infeasible. 

Instead, we decided to lock out the computer that is used to prepare files. Without an authorized user's keycard present, you are unable to use the keyboard or mouse of the computer, or plug in your own USB drive or similar to import files. The end result is no modification of the laser or its operation, while stopping anyone unauthorized from using the laser, since they cannot prepare files.

This implementation allows students to "hand off" larger jobs to other students if they need to go to class without having to leave their ID, and also allows one student to be cutting something while another uses the computer to prepare their on-deck file, push it to the laser cutter, and then hang on to their ID.

Since these are shared community computers, it also provides the critical information of who was using the computer at what specific time. If we ever need to investigate a specific file or if a user attempted to load a malicious program, we will know exactly who was using the computer. 
