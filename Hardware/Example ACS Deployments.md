# Example ACS Deployments

Below is a list of the machines and systems in which Access Control was integrated in the SHED, and notes on how that was achieved.

For the sake of readability, each section has been collapsed. Click on the name of a system to expand and view the contents. 

<details>
  <summary>Electronics Benches</summary>

## Electronics Benches

The Electronics Benches in the SHED Makerspace already had a convenient single power strip running all equipment on the bench - soldering iron, electrical test equipment, overhead lights, etc., so a mains power switch was simply put in series with the entire bench. This also provides the benefit that the overhead lighting on the bench can be wired "always on" and serve as a visual indicator of if a bench is activated or not. 

Full Bench Max Current Draw: 

</details>

<details>
  <summary>Bernina Sewing Machine, Embroiderer, Serger</summary>

## Bernina Sewing Machine, Embroiderer, Serger

All Bernina textiles equipment worked well with a simple power interruption. The machines are able to gracefully recover from being powered down and up by a mains disconnect, and it was the least invasive way to interact with the machine. 

This switch setup is used on Bernina models 475, 590, E16, Q20, and L460.

</details>

<details>
  <summary>Roland Vinyl Cutter</summary>

## Roland Viny Cutter

The Roland Vinyl Cutter is another machine where a simple power interruption was fine. While we implemented this switch on the AC side so we can also siphon off power to run the access control system, the system's low-current external DC power supply lends itself to easily using the signal relay switch. 

</details>

<details>
  <summary>Epilog Fusion Edge Laser Cutter</summary>

## Epilog Fusion Edge Laser Cutter

For the Fusion Edge and Pro laser cutters, we decided to use a USB switch to turn off the computer's keyboard and mouse. The benefit of doing this is that the laser cutter will not be interrupted mid-cut if you bump your ID, and one person can be laser cutting while another preps their files to be on deck. 

</details>

<details>
  <summary>Epilog Fusion Pro Laser Cutter</summary>

## Epilog Fusion Pro Laser Cutter

For the Fusion Edge and Pro laser cutters, we decided to use a USB switch to turn off the computer's keyboard and mouse. The benefit of doing this is that the laser cutter will not be interrupted mid-cut if you bump your ID, and one person can be laser cutting while another preps their files to be on deck. 

</details>

<details>
  <summary>Bantam CNC Mill</summary>

## Bantam CNC Mill

The Bantam CNC Mill is small enough that directly interrupting its AC power is sufficient to turn it off. If needed, you could alternatively interrupt the USB connection from the computer to the mill, since it drip-feeds G-Code. 

</details>

<details>
  <summary>ProtoTrak ELX Lathe</summary>

## ProtoTrak ELX Lathe

The ELX controller has a 1 pin connector out the back labeled "E-stop", that is used to indicate to the spindle motor that the e-stop has been pressed and to brake. Interrupting this wire with a Signal Relay will make it so the spindle cannot be activated without a key card, but the controller and X/Y axes can still be used as normal. The second channel of the signal relay is put in series with the door switch, so that CNC operation of the axes is not possible without a keycard either.

</details>

<details>
  <summary>ProtoTrak RLX Lathe</summary>

## ProtoTrak RLX Lathe

The specific RLX-equipped lathe we have in the SHED has a secondary e-stop already installed in the front of the machine, that is easily intercepted with a Signal Relay. This will put the machine into an e-stop state, stopping all exes and the spindle from moving. The controller will also go into an obvious error state until the e-stop is rectified.

</details>

<details>
  <summary>ProtoTrak KMX Mill</summary>

## ProtoTrak KMX Mill

The K3 KMS mill we have at the SHED has a "EURO E-STOP" DB9 connector on it, that is normally bypassed by a jumper head. This jumper head connects pin 1-6, 2-4, 3-5, and 7-8-9. Removing this plug puts the KMX into an e-stop state. In testing, we found that severing just the 2-4 connection is sufficient for triggering the e-stop state. A custom variant of the Signal Relay switch, called the KMX switch, was developed for this. The switch has a DB9 with the matching bypass plug wiring, and a relay between pins 2 and 4. So, a DB9 cable can be used to connect this to the EURO plug, and trigger the machine in an e-stop state to turn it off. The switch pulls power from the USB port on the back of the TRAK controller. When in an e-stop error state, the controller stops the X and Y axes and the spindle from moving. The controller will also go into an obvious error state until the e-stop is rectified.

</details>

<details>
  <summary>Laguna 14CX Bandsaw</summary>

## Laguna 14CX Bandsaw

All of our Laguna bandsaws have motors too large to AC switch. Luckily, they all also have a limit switch connected to the brake pedal, that turns off the motor when it is pressed. this is a low-current AC connection, that we interrupt with a contactor switch, to both power ACS and to toggle the motor's contactor. 

</details>

<details>
  <summary>Powermatic PM2820EVS Variable Speed Drill Press </summary>

## Powermatic PM2820EVS Variable Speed Drill Press

Our PowerMatic drill presses have at most a 1HP motor, so the AC switch can be used to toggle them. 

</details>

<details>
  <summary>Jet 8x14 Horizontal Bandsaw</summary>

## Jet 8x14 Horizontal Bandsaw

The Jet 8x14 is a 1HP motor, so it can be interrupted with the AC switch. 

</details>

<details>
  <summary>SawStop Pro 3HP</summary>

## SawStop Pro 3HP

The Sawstop was the inspiration for developing the Contactor Switch. It is implemented as shown in the wiring diagram below. The Switch receives power any time the system is plugged in, but without a keycard the control electronics do not get power, so the devices acts like it is shut off. Removing the keycard while the saw is on will also make the system act like it was unplugged/plugged in with the blade on, and will enter an error state on next power-up. 

![image](https://github.com/user-attachments/assets/13039857-7e39-4d9e-adc6-4adb568b7a55)

</details>

<details>
  <summary>Powermatic PM2800B Drill Press</summary>

## Powermatic PM2800B Drill Press

Our PowerMatic drill presses have at most a 1HP motor, so the AC switch can be used to toggle them. 

</details>

<details>
  <summary>Laguna DB126 Sander</summary>

## Laguna DB126 Sander

the DB126 has a motor too large for an AC switch. There are no interlocks or secondary ways to internally shut it off. A custom solution may be needed for this device. The most likely solution is to integrate an AC switch and a contactor to make a bigger/stronger AC switch for these devices.

</details>

<details>
  <summary>Laguna 186BX Bandsaw</summary>

## Laguna 186BX Bandsaw

The 186BX and many other Laguna tools have a low-current limit switch in series with the contactor's coil power. This is used to cut motor power when the brake pedal is pressed. See the wiring diagram below for how we integrated the Contactor Switch.

On our 186BX bandsaws, we also removed the 220v accessory outlet from the system. We would never use this outlet in our application, and removing it allowed for an easy way to route wires out of the pillar for interfacing with the switch. 

![image](https://github.com/user-attachments/assets/a05c4ab5-3067-4b9b-ba9a-855a8b3c2337)

</details>

<details>
  <summary>Laguna SS24T Spindle Sander</summary>

## Laguna SS24T Spindle Sander

The SS24T has a 1HP motor, so it can be controlled with the AC switch.

</details>

<details>
  <summary>Laguna PX20 Planer</summary>

## Laguna PX20 Planer

The PX20 has a limit switch on the hood, running at 120VAC, so a contactor switch can go in series to control it as well as provide power.

</details>

<details>
  <summary>Laguna JX8 Jointer</summary>

## Laguna JX8 Jointer

The JX8 has a knee stop bar on it, that internally is a limit switch. A contactor switch in series with this will stop the motor from moving, and also powers the ACS. 

</details>

<details>
  <summary>Festool KS120 Miter Saw</summary>

## Festool KS120 Miter Saw

The KS120 has a max current draw of 13A, so the AC Power Switch can be used to run it. 

The KS120 is normally run connected to the plug on the Festool vacuum. The vacuum and the saw together would be close to the limit of the AC power switch. Therefore, the switching element should go between the vacuum and the saw. This allows the saw to still turn on the vacuum when it is used. 

</details>

<details>
  <summary>Jet Scroll Saw TSS18</summary>

## Jet Scroll Saw TSS18

The JET scroll saw is only a 1/16HP motor, so it can easily be interfaced with using an AC switch. 

</details>

<details>
  <summary>ShopSabre IS510 CNC Router</summary>

## ShopSabre IS510 CNC Router

The ShopSabre machine is an excellent candidate for a USB switch. Without the keyboard and mouse, there is no way to move/manipulate the machine. 

</details>

<details>
  <summary>ShopBot 5 Axis CNC Router</summary>

## ShopBot 5 Axis CNC Router

Unknown, todo once the machine is set up

</details>

<details>
  <summary>Panel Saw</summary>

## Panel Saw

The Panel Saw will probably just be an AC switch on the power to the saw itself, which seems to just be a normal circular saw.

</details>

<details>
  <summary>Haas TM1P CNC Mill</summary>

## Haas TM1P CNC Mill

Modern Haas machines have a secondary emergency stop on the IO PCB intended to connect to a robot cell. By connecting one channel of the Signal Relay as the NC switch, and the other as the NO switch, we can interface with this plug. There is a 120V outlet in the Haas control panel, that we can connect a USB wall wart to, to power the signal relay. 

[See this page on the Haas website for more details](https://www.haascnc.com/service/troubleshooting-and-how-to/reference-documents/robot-integration-aid---ngc.html)

</details>

<details>
  <summary>Haas ST10 CNC Lathe</summary>

## Haas ST10 CNC Lathe

Modern Haas machines have a secondary emergency stop on the IO PCB intended to connect to a robot cell. By connecting one channel of the Signal Relay as the NC switch, and the other as the NO switch, we can interface with this plug. There is a 120V outlet in the Haas control panel, that we can connect a USB wall wart to, to power the signal relay. 

[See this page on the Haas website for more details](https://www.haascnc.com/service/troubleshooting-and-how-to/reference-documents/robot-integration-aid---ngc.html)
</details>

<details>
  <summary>OMAX MicroMax Water Jet</summary>

## OMAX MicroMax Water Jet

The OMAX MicroMax is a perfect candidate for a USB switch. Without the keyboard and mouse, all you can do on the machine is turn on/off the pump's low-pressure side, or press the e-stop. 

</details>

<details>
  <summary>Fablight 4500 Laser Cutter</summary>

## Fablight 4500 Laser Cutter

The Fablight 4500 has a secondary connection for a door switch intended for use with the tube add-on. We interrupt it with a signal relay switch to make the device read as the door being open when no keycard is present. This stops the machine from moving or firing. Power is drawn from an open USB-A port on an integrated mini computer inside the Fablight.  

</details>
