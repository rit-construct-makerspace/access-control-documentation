---
layout: default
title: Troubleshooting
nav_order: 3
---

# Troubleshooting Guide

## ACS denying all cards, slow to respond, or white light(s) visible on front

**ISSUE: Network Failure**

The ACS device cannot reach the server, so access checks, sign-ins, and status reports will not go through. 

**Triage Fix:**
* For ACS devices, users who have previously used the equiopment will be stored locally, and can still use the equipment.
* The above list can also be used to verify users to switch machine states to Always On, bypassing the ACS devices.

**Cause 1: Network Hardware Failure**
* Reboot the ACS Device
  * Hardware V2.4.1 and Above: Press and hold the button on the back for 5 seconds, until the light(s) start cycling red-green-blue.
  * All other versions: Unplug the DE9 cable, wait 5 seconds, plug cable back in.
    * If accessible, removing power input from the Switch will also work (such as unplugging the AC Power Switch).
If this does not resolve it, move to Cause 2.

**Cause 2: Local Network Failure**
* If this is the case, all other nearby ACS devices will also not work.
* If ACS device is on WiFi, use your phone to check if the WiFi network is present
* If ACS device is on ethernet, check for blinking activity light on the network jack.
* If local network is not working, reboot access points and/or switches that the ACS device connects to
  * You may need to contact your administrator for assistance with this
If local network is fine, move to Cause 3.

**Cause 3: Server Failure**
* If this is the case, ALL ACS devices and the website will be non-functional.
* Check if the website is still present, try clicking through a few pages to confirm.
* If the website is offline, contact your administrator for assistance.

**If this is a new device**
* For new devices, the most common issue is restricted networks. Contact your institution's IT department for help registering a device to the network.
* Ensure that all configuration settings have been applied properly.

## ACS stuck in lockout state, will not change state, constantly beeping/alarming

**ISSUE: ACS device is in an emergency state**

There are some situations that disable an ACS device from operating, including the override keys.

**Cause 1: Overtemperature Alarm**
* Check the Access Devices tabs on the website
  * If overtemperature, there will be a red border on the device
* Shut down the device that is overtemperature and remove power if possible
* If tempearture is within normal operating range, configure a higher temperature limit on the ACS system
  * Keep in mind that the temperature limit is when any part of the system hits that number. Devices with Switches embedded in an enclosure or similar may require a higher temperature limit.
  * Setting a temperature limit of 999C will disable the temperature safety (not reccomended).

**Cause 2: Hardware Fault Detected**
* A hardware fault that the system cannot recover from has been detected. Usually caused by damage to the electronics.
* Restart the ACS device
  * Hardware V2.4.1 and Above: Press and hold the button on the back for 5 seconds, until the light(s) start cycling red-green-blue.
  * All other versions: Unplug the DE9 cable, wait 5 seconds, plug cable back in.
    * If accessible, removing power input from the Switch will also work (such as unplugging the AC Power Switch).
* Open the ACS device and check for damage to the electronics
  * Wires unplugged or damaged, bad solder connections, etc. are common
  * Hardware V2.3.1 and earlier, check for damage to to removable NFC reader.
