# Example ACS Deployments

Below is a list of the machines and systems in which Access Control was integrated in the SHED, and notes on how that was achieved.

## Electronics Benches

The Electronics Benches in the SHED Makerspace already had a convenient single power strip running all equipment on the bench - soldering iron, electrical test equipment, overhead lights, etc., so a mains power switch was simply put in series with the entire bench. This also provides the benefit that the overhead lighting on the bench can be wired "always on" and serve as a visual indicator of if a bench is activated or not. 

## Bernina Sewing Machine, Embroiderer, Serger

All Bernina textiles equipment worked well with a simple power interruption. The machines are able to gracefully recover from being powered down and up by a mains disconnect, and it was the least invasive way to interact with the machine. The only downside to this approach is it takes about 30 seconds for the machines to power up.

We keep the machine's power switch always in the on position, so the machine immediately powers up when a card is inserted.

This switch setup is used on Bernina models 475, 590, E16, Q20, and L460.

## Roland Viny Cutter

The Roland Vinyl Cutter is another machine where a simple power interruption was fine. While we implemented this switch on the AC side so we can also siphon off power to run the access control system, the system's low-current external DC power supply lends itself to easily using the signal relay switch. 

## Epilog Fusion Edge & Fusion Pro Laser Cutters

For the Fusion Edge and Pro laser cutters, we decided to use a USB switch to turn off the computer's keyboard and mouse. The benefit of doing this is that the laser cutter will not be interrupted mid-cut if you bump your ID, and one person can be laser cutting while another preps their files to be on deck. 

## Bantam CNC Mill

The Bantam CNC Mill is small enough that directly interrupting its AC power is sufficient to turn it off. If needed, you could alternatively interrupt the USB connection from the computer to the mill, since it drip-feeds G-Code from a connected computer, including all movement commands. 

## ProtoTrak ELX Lathe

The ELX controller has a 1 pin connector out the back labeled "E-stop", that is used to indicate to the spindle motor that the e-stop has been pressed and to brake. Interrupting this wire with a Signal Relay will make it so the spindle cannot be activated without a key card, but the controller and X/Y axes can still be used as normal. The second channel of the signal relay is put in series with the door switch, so that CNC operation of the axes is not possible without a keycard either.

Power for the Signal Relay Switch is drawn from a "wall wart" supply plugged into a standard AC outlet inside the electrical cabinet.

## ProtoTrak RLX Lathe

The specific RLX-equipped lathe we have in the SHED has a secondary e-stop already installed in the front of the machine, that is easily intercepted with a Signal Relay. This will put the machine into an e-stop state, stopping all exes and the spindle from moving. The controller will also go into an obvious error state until the e-stop is rectified.

## ProtoTrak KMX Mill

The K3 KMS mill we have at the SHED has a "EURO E-STOP" DB9 connector on it, that is normally bypassed by a jumper head. This jumper head connects pin 1-6, 2-4, 3-5, and 7-8-9. Removing this plug puts the KMX into an e-stop state. In testing, we found that severing just the 2-4 connection is sufficient for triggering the e-stop state. A custom variant of the Signal Relay switch, called the KMX switch, was developed for this. The switch has a DE-9 with the matching bypass plug wiring, and a relay between pins 2 and 4. So, a DB9 cable can be used to connect this to the EURO plug, and trigger the machine in an e-stop state to turn it off. 

The switch pulls power from the USB port on the back of the TRAK controller. When in an e-stop error state, the controller stops the X and Y axes and the spindle from moving. 

The controller will go into an obvious error state until the e-stop is rectified.

See [KMX Switch](ACS Switches.md#kmx-switch) for more information.

## Laguna 14CX Bandsaw

All of our Laguna bandsaws have motors too large to AC switch. Luckily, they all also have a limit switch connected to the brake pedal, that turns off the motor when it is pressed. this is a low-current AC connection, that we interrupt with a Contactor switch, to both power ACS and to toggle the motor's contactor. 

## Powermatic PM2820EVS Variable Speed Drill Press

Our PowerMatic drill presses have at most a 1HP motor, so the AC Power Switch can be used to toggle them. 

## Jet 8x14 Horizontal Bandsaw

The Jet 8x14 is a 1HP motor, so it can be interrupted with the AC Power Switch.

## SawStop Pro 3HP

The SawStop was the inspiration for developing the Contactor Switch. It is implemented as shown in the wiring diagram below. The Switch receives power any time the system is plugged in, but without a keycard the control electronics do not get power, so the devices acts like it is shut off. Removing the keycard while the saw is on will also make the system act like it was unplugged/plugged in with the blade on, and will enter an error state on next power-up. 

![image](assets/acs sawstop wiring.png)

## Powermatic PM2800B Drill Press

Our PowerMatic drill presses have at most a 1HP motor, so the AC Power Switch can be used to toggle them. 

## Laguna DB126 Sander

The DB126 motor is slightly too powerful for the AC Power Switch, but we still use it. A slow-blow fuse was installed so that the inrush startup current of the motor does not cause issues. In steady-state the system operates perfectly fine.

## Laguna 186BX Bandsaw

The 186BX and many other Laguna tools have a low-current limit switch in series with the contactor's coil power. This is used to cut motor power when the brake pedal is pressed. See the wiring diagram below for how we integrated the Contactor Switch.

On our 186BX bandsaws, we also removed the 220v accessory outlet from the system. We would never use this outlet in our application, and removing it allowed for an easy way to route wires out of the pillar for interfacing with the switch. 

![Laguna Wiring](assets/acs%20bandsaw%20wiring.png)

## Laguna SS24T Spindle Sander

The SS24T has a 1HP motor, so it can be controlled with the AC Power Switch.

## Laguna PX20 Planer

The PX20 has a limit switch on the hood, running at 120VAC, so a Contactor Switch can go in series to control it as well as provide power.

## Laguna JX8 Jointer

The JX8 has a knee stop bar on it, that internally is a limit switch. A Contactor Switch in series with this will stop the motor from moving, and also powers the ACS. 

## Festool KS120 Miter Saw

The KS120 has a max current draw of 13A, so the AC Power Switch can be used to run it. 

The KS120 is normally run connected to the plug on the Festool vacuum. The vacuum and the saw together would be close to the limit of the AC power switch. Therefore, the switching element should go between the vacuum and the saw. This allows the saw to still turn on the vacuum when it is used, or alternatively the vacuum can be used normally without the miter saw activated.

## Jet Scroll Saw TSS18

The JET scroll saw is only a 1/16HP motor, so it can easily be interfaced with using an AC Power Switch. 

## ShopSabre IS510 CNC Router

For the IS510, we decided to put the open/available USB ports on a USB Hub Switch, but not the keyboard and mouse. This means that the machine can operate without a card present. By controlling the USB ports though, it makes it so only authorized users can load programs. This was chosen since the IS510 will often run for hours on its own, and we don't want to tie up an ID and operator waiting with it. 

## ShopBot 5 Axis CNC Router

We are still setting up the ShopBot, so we will update this once we complete that.

## Panel Saw

The Panel Saw is just a normal off-the-shelf circular saw, so we simply installed an AC Power Switch on it.

## Haas TM-1P CNC Mill & Haas ST-10 CNC Lathe

Modern Haas machines have a secondary emergency stop on the IO PCB intended to connect to a robot cell. By connecting one channel of the Signal Relay as the NC switch, and the other as the NO switch, we can interface with this plug. There is a 120V outlet in the Haas control panel, that we can connect a USB wall wart to, to power the signal relay. 

![Haas Wiring](assets/acs%20haas%20wiring.webp)

[See this page on the Haas website for more details](https://www.haascnc.com/service/troubleshooting-and-how-to/reference-documents/robot-integration-aid---ngc.html)

## OMAX MicroMax Water Jet

The OMAX MicroMax is a perfect candidate for a USB Hub Switch. Without the keyboard and mouse, all you can do on the machine is turn on/off the pump's low-pressure side, or press the e-stop. 

## Fablight 4500 Laser Cutter

The Fablight 4500 has a secondary connection for a door switch intended for use with the tube add-on. We interrupt it with a signal relay switch to make the device read as the door being open when no keycard is present. This stops the machine from moving or firing. Power is drawn from an open USB-A port on an integrated mini computer inside the Fablight.  
