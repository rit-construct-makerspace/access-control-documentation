# ACS Core

![ACS Core](assets/acs%20core.jpg)

The Core is the heart of an ACS deployment. Every machine has a Core, that controls all connected ACS switches and tertiary equipment. The Core also serves as the connection between the website and the hardware. 

## ACS State

The ACS can be in one of multiple states that impact how it operates. The state is reported on the [user interface](#user-interface), as well as regularly by the [API](./API%20Information.md), making it visible on the [access devices page](../ACS%20Website/Equipment%20Management.md#access-devices).

* **Idle** is the standard state the system should be in. This means the device is ready to be used, but not currently being used. It has no apparent issues and everything is working as expected. This is generally represented by a *yellow* light.
* **Unlocked** is the other standard state of the system. This means that a user with proper authorization is currently using the machine. This is generally represented by a *green* light.
* **AlwaysOn** is a special state where the equipment is always active, as if the ACS was not installed. It is generally represented by a *green* light. It is achieved with a [staff bypass](#state-bypass).
* **Lockout** is a special state where the machine cannot be activated, no matter what. It is generally represented by a *red* light. It is achieved with a [staff bypass](#state-bypass).
* **Fault** is an emergency state the system enters when an issue is detected, such as excessively high temperature or detecting attempts to tamper with the hardware. It is generally represented by a *flashing red* light. It can be cleared by rectifying the cause of the fault, and then [restarting the device](../Reference%20Guides/Staff%20Quick%20Reference.md#restarting-device).
* **Startup** is the default state of a machine when it powers on unexpectedly, such as after a power outage or a software crash. It is represented by a *blue* light. 

### State Bypass

[Maker Mentors](../ACS%20Website/User%20Management.md#mentor-role) and [Staff](../ACS%20Website/User%20Management.md#staff-role) have the ability to change the ACS into and out of many of the states listed above. For instance;

* Changing from Startup to Idle when a machine is first powered on
* Disabling broken equipment by changing it from Idle to Lockout
* Setting a machine AlwaysOn so untrained users can use it for a workshop/event.

The device will inform the website (assuming it has a network connection) of whom has changed the state for record keeping purposes.

See the [Staff Quick Reference](../Reference%20Guides/Staff%20Quick%20Reference.md#changing-device-state) for information on how to change state.

## Card Reader

The entire premise of ACS works on the idea that users have a unique NFC card, used to unlock equipment. The card is inserted physically into the equipment, and removing the card ends the session. This ensures that a user only starts 1 session, and they don't forget to end their session. 

Card presence is detected by 2 limit switches. Once the switches detect a card's presence, it is read by the NFC reader. The ID card is re-read periodically to ensure the right one is still present. When the switches detect the card is removed, the system returns to idle state after a 50 millisecond safety timer. A card being re-inserted in this time is treated as a continuation of the session.

The [NFC UID is used for user identification](../ACS%20Website/User%20Management.md#id-cards). Hardware version 2.4.X and beyond optionally checks for the proper NFC SAK and REQA response as a security measure. Once SAK and REQA are matched, the system makes an [Authorization API Call](./API%20Information.md#Authorization).

### Offline Users List

In the event of a Website failure, we still want ACS equipment to be accessible. When a user first activates a piece of equipment, their UID is added to an internal list in the Core's flash. If the hardware cannot reach the server (see [Network Watchdog](#network-watchdog)), users can still be authorized by referencing this internal list. It is also used to verify staff IDs for [staff bypass](#state-bypass).

If a user is approved to use the 

## Network Communication

An ACS deployment communicates with the server via the Core's network connection. The Core is built around an ESP32-S2, providing a 2.4GHz WiFi connection for most deployments. Deployments in areas with poor WiFi performance can instead opt for the 10/100 ethernet connection achieved using a Wiznet W5500.

The Core can be configured to use WiFi, ethernet, or both interchangeably for communication. See [ACS Configuration](ACS Configuration.md) for more information.

The Core's network connection is used to communicate with the [website](../ACS%20Website/index.md) via the hardware [API](./API%20Information.md).

### Network Watchdog

Since a network connection is crucial to ACS operating, a network watchdog is constantly monitoring the Core's connection. If the Core fails to perform a regular network action twice in a row, it triggers the watchdog. The watchdog then goes through 3 steps to see if the network connection is still valid;

1. Check network state. If the network connection is not reporting as nominally connected, re-attempt the connection. If it fails, skip to restart.
2. If the network is nominally connected and/or was able to reconnect, check access to the internet. Do this by pinging www.rit.edu. If we get a bad HTTP response, skip to restart.
3. If the internet is working, check the server specifically. Make an API [Check](./API%20Information.md#Check) call to verify the server is working.
    * If we get an HTTP response other than 200, the server is probably down.
    * If we get a negative HTTP response, our connection is probably bad/damaged somehow that wasn't previously caught. 

If the watchdog runs into any issues, it restarts the Core to try and fix it. This will fix a wide number of issues, such as improper network enumeration, memory limit failures, etc.The watchdog waits until the device is not being used before restarting. It will signify to the user that network connection is lost by slowly alternating between *green* (the default color when a machine is in use) and *white*. 

!!! note
    The watchdog cannot catch and deal with all issues. A common one that you may run into is the network's password being changed. If the Core does not have the updated password, it will attempt to connect to the network, fail, restart, etc. an infinite number of times.

If the watchdog detects an issue with just the server and not the internet, there is no need to restart. The indicator light on the front is set to *white*. The system can still be activated by [staff bypass](#state-bypass) or by referencing the [offline users list](#offline-users-list).

## User Interface

While basic, the Core does provide some user signaling to show state.

Hardware **2.3.X and previous** have an illuminated button on the front to indicate system state. There is also a piezo buzzer for playing tones to indicate state changes.

Hardware **2.4.X and beyond** have multiple RGB lights on the front to indicate system state. Since there are multiple independent-controlled lights, the interface provides more fine-control than the 2.3.X solution. The piezo buzzer is replaced with a DAC and speaker on this hardware revision, allowing spoken-language reports of system state and more verbose error reporting to the user (e.g. "Error, please sign in at the front desk." instead of 3 beeps).