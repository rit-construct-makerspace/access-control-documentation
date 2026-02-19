# Signal Relay Switch

Version: V2.0.2

The Signal Relay Switch is used to interrupt low-current signals in a wide variety of control loops. The Signal Relay can be used in more applications than a Contactor Switch, since it does not draw power from the connected circuit, thus not interfering with weak digital logic signals. Signal Relays Switches are commonly installed in series with door switches, emergency stops, cycle start buttons, etc., to disable a machine that isn't feasible to cut power to directly. 

As a result of the requirement to not draw power from the connected system, the Signal Relay Switch needs to be independently powered. 5v is supplied via a USB-B connector on the board from any available source. 

The Signal Relay Switch offers 2 independent but synchronized control channels, each with a normally open and normally closed contact. Each channel is rated for resistive loads of 2A at 30VDC or 0.5A at 125VAC.

If power is lost, the Signal Relay returns to the off state in 4 milliseconds.

## Example Deployment: HAAS TM-1P

The HAAS TM-1P is the largest CNC mill in our shop. It runs on a 440VAC 13A connection, much too large to interrupt power to. There is also not an embedded contactor that can easily be accessed to cut power. Cutting power is also not ideal, since that'd lose program parameters and other RAM-based settings as well as the machine's position if you remove your keycard. 

All HAAS machines produced after ~2015 have considerations for adding a robotic arm for automation, and part of that is a secondary E-Stop input on the control board. This Auxiliary E-Stop can be found at TB1-B, as indicated in the image below. In our application we connected pins 7 and 8 of TB1-B to the normally open connection of the Signal Relay Switch, such that when the ACS is unlocked, it released the E-Stop, but any other state (including removing ACS power) results in an E-Stop state. JP1 next to TB1-B also needs to be removed to enable this Auxiliary E-Stop.

For power, there is a standard 115VAC outlet inside the electrical cabinet of the TM-1P that we plugged a USB wall wart into. This outlet is energized any time the machine power is on. 

This implementation allows us to retain parameters and position of the machine, as well as not constantly cycle the power on and off, while still locking out all control over the equipment. It also ensures that a card (and therefore hopefully an operator) is present at all times, while also making it easy to step away for a minute or swap operators. 

![Haas Wiring](assets/acs%20haas%20wiring.webp)

[Image Source - haascnc.com](https://www.haascnc.com/service/troubleshooting-and-how-to/reference-documents/robot-integration-aid---ngc.html)