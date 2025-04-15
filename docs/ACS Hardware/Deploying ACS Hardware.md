# Deploying ACS Hardware

This page will provide a generalized guide for deploying new ACS hardware, including identifying the ideal switch type, setting parameters, etc.

Make sure to also check out the "Example ACS Deployments" page for inspiration.

## Determining Switch Type

One of the major benefits of the SHED's ACS design is the different switch types available. This enables integration with more complex equipment and in ways that are more compatible with tempermental devices.

Things to Consider:

* **DO NOT DISABLE EMERGENCY STOPS, LIMIT SWITCHES, INTERLOCKS, ETC**
    * Ex: If a CNC router has a wireless pendant that connects to a USB receiver, do not use a USB Hub Switch to disable this receiver if there's an emergency stop button on the pendant.
* Do not cut power to only a portion of the equipment's control electronics that may cause it to respond in an unknown way.
    * Ex: Cutting power to a CNC's controller but not the motor drivers may cause the motors to act unpredictably.
    * The opposite is fine though, since random commands from the controller can't move un-powered motors.
* If your Switch wiring interfaces with a safety component like an emergency stop, it should be wired in **series**, and in a way that the Switch doesn't impact the operation of the emergency stop during normal use. 
* How does a device deal with an abrupt power-off?
    * Bad Example: Suddenly cutting power to a ProtoTrak controller causers it to lose all data such as coordinates, tool tables, etc.
    * Bad Example: Epilog lasers need to go through a 60-second startup every time you turn power back on.
    * Good Example: Laguna power tools can work fine immediately after power is restored.
    * If a device doesn't handle power-off well, may not be ideal for an AC Power Switch, Contactor Switch, etc.
    * How does the device deal with power-on as well?
        * Ex: Laguna belt sanders have a power switch instead of a button. So, if it was left on when someone removed their card, the machine immediately starts spinning when a new card is inserted.
* Are there easily accessible control wires or interfaces?
    * Ex: Many Laguna bandsaws have a low-current control switch on the front door, that cuts power to the motor, with easy-to-access wires for a Contactor Switch.
    * Ex: Haas CNC controllers have a screw terminal for an auxiliary emergency stop connection.
* What's the current draw of the device (if considering interrupting power)?
    * See [ACS Switches](ACS Switches.md) page for maximum ratings.

When are certain switches best?

* AC Power Switch
    * 115V devices with current draw <15A
    * Device responds well to being powered off abruptly (like simple power tools).
    * No easily accessible control interfaces.
* Contactor Switch
    * Mains-voltage control loop to low-current interlocks, switches, etc.
    * Equipment already has an integrated contactor for main power.
    * Device is very high-power and there are no control interfaces (install your own contactor + Contactor Switch).
* Signal Relay Switch
    * Device has low-voltage/very low-current control signals that can be interrupted (like an Emergency Stop).
    * There is a source of power nearby for the Switch.
* USB Hub Switch
    * Device uses USB for its primary control interface.
    * Loading files via USB is required for the equipment to be used.
    * Device is powered via USB and draws under 500mA.
    * No emergency stops run via USB.