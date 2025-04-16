# Hardware Standards 

This page documents the standards to make ACS-compliant hardware that will be compatible with the rest of the system. 

## Topology

ACS deployments are intended to usually be comprised of a single Core and a single Switch, although with the use of active splitters or combiners multiple Cores or Switches can be combined in a deployment. See [ACS Tertiary Components](ACS Tertiary Components.md) for information on such components. Unless otherwise states, assume documentation is referring to a standard Core+Switch deployment. 

Passive combination of multiple devices in a deployment is not supported.

## Inter-Component Wiring

All components of ACS connect with DE-9 straight-through cables, with the following pinout:

| Pin Number | Purpose                                  |
|------------|------------------------------------------|
| Pin 1      | ACS signal, 5v logic level.              |
| Pin 2      | OneWire bus form temperature/ID numbers. |
| Pin 3      | System ground.                           |
| Pin 4      | System power (~5v).                      |
| Pin 5      | System ground.                           |
| Pin 6      | Switch type identifier.                  |
| Pin 7      | Interrupt, open-drain only.              |
| Pin 8      | RS485 Data B / No Connect                |
| Pin 9      | RS485 Data A / No Connect                |
| Shield     | Grounded at Core for shielding.          |

### ACS Signal

ACS Signal is the standard signal that the Core uses to drive the connected Switch. It is a 20mA signal, referenced to system power. A high signal indicates to turn on. Both the Core and Switch must pull this down to system ground with at least a 10k resistor.

### OneWire Bus

All ACS components implement a Dallas DS18B20Z or similar OneWire temperature sensor. This is used both for monitoring temperature as well as to assign each component a globally unique ID number. By periodically polling these OneWire IDs, system integrity can be monitored. The Core is the only OneWire master allowed on the bus.

In future production runs of Switches and Cores, a plaintext identifier of the component may be written to the EEPROM of the DS18B20Z for easier identification.

Any device that splits the ACS connection must route OneWire to all connections.

### System Power & Ground

All ACS components share a common ground.

Switches must be capable of providing >500mA at nominal 5v to the system power pin, with some form of reverse-bias protection (like a diode) to allow for power supply OR-ing. To account for this, the output is allowed to drop to 4.5v under load. The output of the Switch must be current-limited to protect against perpetual short-circuits with automatic recovery in under 10 seconds once the short-circuit is removed.

The Core must be capable of running at an input voltage down to 4.2v, with a current draw of no more than 400mA. It can be powered from the DE-9 or the USB input with a diode OR-ing configuration.

### Switch Type Identifier

To identify what kind of Switch is connected, the Core pulls up the Switch Type Identifier pin with a 4.7k resistor to 3.3v. Each Switch type then pulls it to ground with a unique-value resistor. The resulting voltage is read by the Core's ADC to determine the Switch type present. 

Devices that go between a single Switch and the Core should pass this signal through without interference. Devices that connect multiple Switches should tie this pin directly to ground to indicate it is not possible to read the Switch type.

### Interrupt

To allow Switches or other devices to notify the Core of something notable, an interrupt pin is implemented. There's no standard for what the interrupt means. Devices that do not otherwise communicate with the Core must clear the interrupt within 1 second. Devices that communicate with the Core can assert the interrupt until acknowledged. 

The interrupt pin is pulled to 3.3v by the Core, and the other device(s) can ground it to trigger the interrupt. This allows for different logic levels, as well as multiple devices to interrupt at once without bus contention.

Like OneWire, the interrupt pin should go to all connected devices.

### RS485

To allow more complex communication between the Core and other devices in a deployment, a half-duplex RS485 interface is implemented. The Core uses a MAX13487 auto-switching and slew-rate-limited transceiver. The other device(s) should implement a compatible transceiver. 

Devices that split the ACS connection should route the RS485 pins to all devices.

Devices that make use of the RS485 connection should have considerations for termination resistors that can be configured if needed.

Devices that do not make use of RS485 should leave these pins unconnected.
