# ACS Configuration

For the end-user, a simple configuration interface has been developed. This allows you to set up and modify settings on an ACS deployment without needing to recompile code for the devices. 

## Method 1: Configuration Software (Preferred)

**[Click here for the Access Control Configuration GitHub](https://github.com/rit-construct-makerspace/access-control-configuration)**

Configuration software is currently under development, please check back soon!

## Method 2: JSON Configuration

JSON configuration works with any USB-enabled computer, running any operating system that supports CDC USB Devices. 

To configure the ACS Core this way, you first need a serial terminal. If you already know how to use a serial terminal and/or have a preferred program, you can use that instead.

**Suggested: Arduino Serial Terminal**

The Arduino IDE includes a Serial Terminal. Arduino IDE is free and available for all major operating systems [at this link](https://www.arduino.cc/en/software/).

!!! note 
    The Arduino IDE is intended for writing code for and upload said code to Arduino devices. We won't be using 99% of its features, so you can ignore most pop-ups, warning, menus, etc. in the software.

Once you have downloaded and installed Arduino IDE, you can configure the serial terminal. Before continuing, plug the ACS Core into your computer. To make things easier, it is recommended that you unplug any other CDC USB devices (anything except a mouse/keyboard) at this time.

In the Arduino IDE, under **Tools**, select **Port**, the only option should be your device.

!!! info
    The name of your device in the Arduino IDE will be something generic like "USB Device", "USB UART Bridge", "USB Serial Device" or something specific but unrelated like "CH340". 

Once you have selected the device, open the terminal by hitting **CTRL+SHIFT+M** or hit **Serial Monitor** under **Tools**

At the bottom of the Arduino IDE, a black window should open. On the right side, set the **Baud Rate** in the dropdown to **115200**.

You may start to see information printing in this area, depending on what the ACS Core is doing. If you see gibberish or unknown characters (squares), your baud rate is not configured.

Now all you have to do to apply settings is to paste a properly-formatted serialized JSON into the bar next to the baud rate, and press enter! The ACS Core will report if the settings have been applied, and if so, restart.

!!! warning
    Sending improperly formatted JSONs, or JSONs with improper passwords too many times can result in all settings being wiped for safety reasons! If you get an error more than 2-3 times, power cycle the ACS Core to wipe the number of errors and continue trying.

### JSON Formatting

Setting are sent via a serialized JSON file. The format of the JSON is very particular, and must be in the format;

> {"Parameter1":"Value1","Parameter2":"Value2"..."ParameterX":"ValueX}

The order of parameters in the JSON doesn't matter. You can include as many parameters as you want/need in a JSON, so you can change just the pertinent settings. The only parameter:value pair that must be there is the configuration password, called "OldPassword". 

JSON parameters and values are case sensitive. Invalid, misspelled, or unused parameters are simply ignored.

!!! warning
    While improper or unused parameters are ignored, improperly-formatted values will cause unexpected behavior!  The system will try its best to figure out minor issues (e.g. putting 1 instead of "1"), but on more complex errors it is often wrong!

!!! warning
    There is no verification or confirmation before new settings are applied, make sure to double-check spelling and configuration before sending the message.

### JSON Configuration Options

The following parameters are accepted settings in a JSON:

* **OldPassword** : The only required parameter! This password needs to match the internal saved password to continue. Forget your password? See **Wipe** below.
* **NewPassword** : Sets a new password for uploading ACS settings.
* **SSID** : WiFi network name (SSID) to attempt to connect to on startup.
* **Password** : WiFi network password, to attempt to connect with on startup. Not to be confused with the ACS password, **OldPassword** and **NewPassword**. Set to "null" (case sensitive) to not use a password.
* **Server** : The server to make API calls to. Should be formatted as a complete URL, e.g. "https://make.rit.edu"
* **Key** : The server's API key.
* **MachineID** : The machine's unique identifier name. Must be all lowercase a-z or 0-9 characters.
* **MachineType** : The numerical machine type, can be found on the website under *Manage Equipment*
* **SwitchType** :  Not currently used. Eventually will be the type of switch that should be connected, for system integrity checks.
* **Zone** : The room/area the equipment is located in. Used for sign-in authentication. Numerical representation, found on website under *Rooms*
* **NeedsWelcome** : If set to 1, will not activate the equipment if the user has not signed into the **Zone** yet today. 
* **TempLimit** : Whole degrees Celsius, if any part of the ACS gets above this temperature it locks out the equipment. Set to 0 to disable.
* **NetworkMode** : Not currently used. Eventually will let you set WiFi only, ethernet only, or both.
* **Frequency** : Time in seconds between regular status reports to the server. Recommended 10 to 40.
* **NoBuzzer** : Set to 1 to turn off buzzer tones and (if applicable) other audio outputs from the Core.
* **DebugMode** : If 1, sends verbose running data to the Serial output. 

!!! warning
    Enabling DebugMode can expose sensitive information, such as WiFi credentials, API keys, ID card numbers, and more. Do not use unless you need it, turn off when not used!

### JSON Special Parameters

The following parameters are not settings per se, but are sent like settings for other purposes.

* **Wipe** : Send with a "1" value to wipe all internal parameter stores, such as when decommissioning a device.
* **DumpAll** : Exports most* settings as a JSON document, for easily saving backups or configuring new hardware.
    * This also reports the MAC address.
    * If the firmware is compiled with the parameter *DumpKey* set to 0, the API key will be removed from the exported information. The only way to change this is to recompile and upload new firmware.

### JSON Configuration Examples

=== "Turn on Debug Mode"

    > {"OldPassword":"Shlug","DebugMode":1"}

=== "Set new WiFi Credentials"

    > {"OldPassword":"Shlug","SSID":"WifiName","Password":"null"}

=== "Export Current Configuration"

    > {"OldPassword":"Shlug","DumpAll":"1"}

=== "Forgot password, wipe all settings and set a new password"

    > {"Wipe":"1"}

    After restart, you can send the following when prompted: 

    > {"NewPassword":"Shlug"}