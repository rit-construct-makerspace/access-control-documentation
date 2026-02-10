# Bus Connection

All ACS devices use a standard connection. Devices always have a female plug, and male-male straight-through cables are always used to interconnect devices. Cables must have a shield that fully extends between the two ends, and appropriate strain relief built into the connector head. While the standards do not specify a wire gauge or material, larger cables, such as 26AWG or 24AWG, are preferred and will permit the deployment to fully utilize the current a power provider is permitted to provide the deployment. 

## Standard Interface Pinout

The standard interface uses a HD15 (High Density D-Subminiature 15 Pin) connector. See Amphenol *CS-DSDHD15MM0-005* as an example of an acceptable cable.

**WARNING** : HD15 cables, connectors, plugs, etc. must not be colored blue in any way, to avoid confusion with a VGA video cable or connector. 

### 1: Access

Push-pull 5v signal from Core, >20mA drive strength. Used to indicate to devices on the deployment if they should be active or not. 

### 2: OneWire

Shared data bus to identify devices in a deployment, read temperature of devices, etc. 

See [OneWire](#onewire) below for more information.

### 3, 10, 14: Ground

System ground, reference for all power and data. 

### 4, 5: +12v Rail

Nominal 12v power rail.

See [Power Standards](./Power%20Standard.md) for more information.

### 6, 13: +5v Rail

Nominal 5v power rail.

See [Power Standards](./Power%20Standard.md) for more information.

### 7: Interrupt

Open-drain interrupt signal, active low. Pulled to 5v by Core. Must be asserted low >5mA drive strength. 

A low signal on this line indicates to the Core that a device is not in a safe/proper operating state, and the deployment should not be activated. 

### 8: Bus GPIO 1

Application-specific input/output #1. 

See [Bus GPIO](#bus-gpio) below for more information.

### 9: Bus GPIO 2

Application-specific input/output #2. 

See [Bus GPIO](#bus-gpio) below for more information.

### 12: Bus GPIO 3

Application-specific input/output #3. 

See [Bus GPIO](#bus-gpio) below for more information.

### 15: Bus GPIO 4

Application-specific input/output #4. 

See [Bus GPIO](#bus-gpio) below for more information.

## Reduced Interface Pinout

For backwards-compatibility with ACS V2.X switches, and to permit non-Core devices with simpler interfaces, a reduced interface is also defined. THis interface uses the more common DB9 connector, with the following pinout;

### 1: Access

5v logic level push-pull signal, driven by the Core >20mA drive strength. Active high indicates the device should be unlocked/activated/etc. 

### 2: OneWire

Shared data bus used to identify devices on the bus. Also allows temperature monitoring of devices. See [OneWire](#onewire) below for more information.

### 3, 5, 9: Ground

Ground reference for all power and signals. 

Pin 9 is not used on V2.X hardware (was reserved for future use). 

### 4, 6: +5V Power

Main system power, 5v nominal. Devices supplying power must connect via an ideal diode and some sort of overcurrent protection. 

On V2.X hardware, pin 6 is "Type", which has a resistor to ground. Connecting this to the bus's 5v will be insignificant, but it does mean that V2.X devices will have more of a voltage drop than V3.X devices using the reduced interface. This is on top of the significant >500mV drop from the Schottky diode found in V2.X devices on the 5v pin.

### 7: Interrupt

Open-drain input to the Core, must be pulled low >10mA drive strength, pulled up at the Core. If any device pulls it low, this indicates the system is not in a safe/normal operating state.

### 8: Unused

This pin is unused in the reduced pinout. It must be left floating and is reserved for future use.

## Adapting Reduced and Standard Interfaces

The pinput of the standard and reduced interfaces were chosen specifically so that they could be adapted using off-the-shelf HD15 to DB9 adapters, intended for video signals, that have the following pinout; 

* DB9 (Pin) - HD15 (Pin)
* 1 (Access) - 1 (Access)
* 2 (OneWire) - 2 (OneWire)
* 3 (Ground) - 3 (Ground)
* 4 (Power) - 13 (+5V)
* 5 (Ground) - Not Connected
* 6 (V2.X: Type/Unused, V3.X: +5V) - 6 (+5V)
* 7 (Interrupt) - 7 (Interrupt)
* 8 (Unused) - Not Connected
* 9 (V2.X: Unused, V3.X: Ground) - 10, 11 (Ground)

## OneWire

Every ACS device except for the Core must implement a OneWire slave device that serves 2 purposes;

* Allow for monitoring of temperatures deployment-wide. 
* Uniquely and type-wise identify the device

The specification does permit for the OneWire bus to be used for other more complex uses, so long as the 2 above requirement are satisfied, and that there is only one OneWire slave per ACS device. 

The standard mandates that OneWire devices are powered by the dployment's 5v rail, versus using the parasitic power of the OneWire bus itself. So a OneWire IC with an independent power pin must be used.

For temperature monitoring, ACS devices must implement the standard temperature monitoring (resolution, conversion time, commands, high threshold) found in a DS18B20Z temperature sensor. The Core must monitor the temperature of each device no less frequently than once every 10 seconds. Additionally, the Core must monitor for an alarm at least 2 times per second. The high temperature alarm threshold must be set to 50C by default, with higher levels permitted based on the specifics of that device, once it has been type identified as described below. 

All OneWire devices have a globally unique 48 bit address with an 8 bit "family code" (intended use of the OneWire debice). This 64 bit address is the unique identifier of an ACS device, and should be used for any situations that require specific identification of a device (i.e. like a serial number).

The Core must monitor for deployment integrity via the OneWire bus, searching for all expected addresses at least once every 30 seconds. If a device that is expected to be present fails to respond, the deployment should go into a fault state.

The default, suggested implementation of these requirements is to use a UMW DS18B20Z or comparable. Notably, the UMW and other DS18B20Zs improve on the original Dallas device by providing bytes 6 and 7 as end-use EEPROM. 

These 2 bytes are used in conjunction with byte 3 (Low Temperature Alarm Threshold) to identify the type of debice attached. The following describes the schema: 

### Byte 3:

* Bit 7 and 6: Always 1. This effectively disables the low temperature threshold (setting it as -128C, far below when the device would stop working)

* Bit 5, 4, and 3: Device Mode. These bits represent if the device is a switch or not, if a switch, if intended to be operated in ganged or independent mode, and if independent, how many channels there are. See [Bus GPIO](#bus-gpio) below for more information. 

    * 000: Switch Independent, 4 channels
    * 001: Switch Independent, 3 channels
    * 010: Switch Independent, 2 channels
    * 011: Switch Ganged operation (Switched only using access signal)
    * 100: Bidirectional GPIO device. Makes use of the GPIO as outputs and inputs, in some application-specific way that is defined by the type of device specifically.
    * 101: Passive Device (No [Bus GPIO](#bus-gpio), switching, interrupt, etc.). Such as a power injector.
    * 110: Interruptor Device (No [Bus GPIO](#bus-gpio), switching, etc.) but can generate interrupts.
    * 111: Communicative Device. This device will use the GPIO as a communication interface (SPI, UART, etc.) to the Core for very complex devices. See [Communicative Device](#communicative-device) for more information.

* Bit 2, 1, and 0: Device Type MSB. See below for more information.

### Bytes 6 and 7

Bytes 6 and 7, along with the 3 LSBs of Byte 3, make up the type identifier for a device. Devices are given identifiers sequentially as they are produced, with minor hardware changes that do not impact end-use being wrapped under the same type ID. For instance, if the USB Hub Switch V3.0.0 has a USB-B port, and V3.0.1 swaps that for a USB Micro-B port, the end-use of the Switch itself did not change. But, changing from a USB-B port to a higher-current-capable USB-C port is a change to the hardware's function (can provide the deployment with more power), that would get a new ID.

### Communicative Device

OneWire is meant as a way for debices to be able to communicate what they are, without the complexity of a microcontroller or similar. But, if a device is already implementing a better communication interface, it makes more sense to just use that. As such, if a device has mode ID 111, the 19-bit type identifier is instead used to convey what better communication interface to use, and then all information about the device is attained over that.



## Bus GPIO

