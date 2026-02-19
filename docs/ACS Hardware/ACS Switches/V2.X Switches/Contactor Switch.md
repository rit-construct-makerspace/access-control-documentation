# Contactor Switch

Version: V2.0.2

The Contactor Switch is intended for interrupting power on larger pieces of mains-powered equipment. Instead of switching power directly, the Contactor Switch is meant to interface with a contactor that then switches power for the equipment. This can be a contactor installed specifically for ACS, but in most applications it is interfacing with a contactor that already exists in the equipment's control circuitry.

The Contactor Switch has 2 inputs and 2 outputs; an input live and neutral, and an output live and neutral. The live and a neutral inputs supply the internal regulator, and when the Switch is activated, the input live is connected to the output live connection. The input neutral and output neutral are always connected, and is there for ease of wiring.

Depending on what is easier for the deployment, the live and neutral wires can be flipped and the system works electrically. By convention though, if possible it is preferred to interrupt live. 

The system has 2 board-mounted fuses, independently protecting the switching output and the power supply, such that the failure of one does not take out the other or vise-versa. 

In the event of a power failure or similar, the Contactor Switch returns to a normally open state in under 8 milliseconds.

The Contactor Switch rated to operate at voltages up to 250VAC, and can switch sustained loads of up to 2A. The relay is UL rated for transients up to 8A at 250VAC, or a motor of 1/10HP at 120VAC or 1/6HP at 250VAC.

## Example Deployment: SawStop Table Saw

The SawStop logical circuitry is powered by the input power wired from L1 through A1 to the electronics. By interrupting this connection with the Contactor Switch, power is interrupted to the control electronics. The SawStop was found in testing to recover elegantly from power being cut, and will require the main blade switch to be cycled before it will start.

![SawStop Wiring](assets/acs%20sawstop%20wiring.png)