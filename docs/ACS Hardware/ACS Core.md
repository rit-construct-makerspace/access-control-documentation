# ACS Core

![ACS Core](assets/acs%20core.jpg)

The Core is the heart of an ACS deployment. Every machine has a Core, that controls all connected ACS switches and tertiary equipment. The Core also serves as the connection between the website and the hardware. 

## Features

### ACS State

### Card Reader

The entire premise of ACS works on the idea that users have a unique NFC card, used to unlock equipment. The card is inserted physically into the equipment, and removing the card ends the session. This ensures that a user only starts 1 session, and they don't forget to end their session. 

Card presence is detected by 2 limit switches. Once the switches detect a card's presence, it is read by the NFC reader. The ID card is re-read periodically to ensure the right one is still present.

The [NFC UID is used for user identification](../ACS%20Website/User%20Management.md#id-cards). Hardware version 2.4.X and beyond optionally checks for the proper NFC SAK and REQA response as a security measure.

### Network Communication

An ACS deployment communicates with the server via the Core's network connection. The Core is built around an ESP32-S2, providing a 2.4GHz network connection for most deployments. Deployments in areas with poor WiFi performance can instead opt for the W5500-based 10/100 ethernet connection.

The Core can be configured to use WiFi, ethernet, or both interchangeably for communication. See [ACS Configuration](ACS Configuration.md) for more information.

### User Interface

While basic, the Core does provide some user signaling to show state.

Hardware **2.3.X and previous** have an illuminated button on the front to indicate system state. There is also a piezo buzzer for playing tones to indicate state changes.

Hardware **2.4.X and beyond** have multiple RGB lights on the front to indicate system state. Since there are multiple independent-controlled lights, the interface provides more fine-control than the 2.3.X solution. The piezo buzzer is replaced with a DAC and speaker on this hardware revision, allowing spoken-language reports of system state and more verbose error reporting to the user (e.g. "Error, please sign in at the front desk." instead of 3 beeps).