# Hardware Troubleshooting

Click on a symptom below to expand the section and see diagnostic and rectifying steps.

??? note "ACS is in a lockout state despite nobody putting it in lockout, or randomly suddenly enters a lockout state."
    
    **ISSUE: Unexpected Restart**

    When the ACS Core restarts, it defaults to a lockout state for safety purposes. This does not applied to scheduled restarts, or user-triggered restarts.

    **Cause 1: Power Issue**

    * A power outage would result in devices returning to a lockout state.
    * Check where power is supplied from, and see if there are any issues there.

    **Cause 2: Core Panic**

    * In the event of a software issue that catastrophically disables the ACS Core, it will restart itself.
    * Report what was happening when the issue happened, and a bugfix can be made.
    * Set the Core to Debug Mode and log the USB output to see what is causing the issue.

??? note "Denying all cards, slow to respond, or white light(s) visible on front."
    
    **ISSUE: Network Failure**

    The ACS device cannot reach the server, so access checks, sign-ins, and status reports will not go through. 

    **Triage Fix:**

    * For ACS devices, users who have previously used the equipment will be stored locally, and can still use the equipment.
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
    * If local network is fine, move to Cause 3.

    **Cause 3: Server Failure**

    * If this is the case, ALL ACS devices and the website will be non-functional.
    * Check if the website is still present, try clicking through a few pages to confirm.
    * If the website is offline, contact your administrator for assistance.

    **If this is a new device**

    * For new devices, the most common issue is restricted networks. Contact your institution's IT department for help registering a device to the network.
    * Ensure that all configuration settings have been applied properly.

??? note "Stuck in lockout state, will not change state, constantly beeping/alarming."

    **ISSUE: ACS device is in an emergency state**

    There are some situations that disable an ACS device from operating, including the override keys.

    **Cause 1: Overtemperature Alarm**

    * Check the Access Devices tabs on the website
      * If overtemperature, there will be a red border on the device
    * Shut down the device that is overtemperature and remove power if possible
    * If temperature is within normal operating range, configure a higher temperature limit on the ACS system
      * Keep in mind that the temperature limit is when any part of the system hits that number. Devices with Switches embedded in an enclosure or similar may require a higher temperature limit.
      * Setting a temperature limit of 999C will disable the temperature safety (not recommended).

    **Cause 2: Hardware Fault Detected**

    * A hardware fault that the system cannot recover from has been detected. Usually caused by damage to the electronics.
    * Restart the ACS device
      * Hardware V2.4.1 and Above: Press and hold the button on the back for 5 seconds, until the light(s) start cycling red-green-blue.
      * All other versions: Unplug the DE9 cable, wait 5 seconds, plug cable back in.
        * If accessible, removing power input from the Switch will also work (such as unplugging the AC Power Switch).
    * Open the ACS device and check for damage to the electronics
      * Wires unplugged or damaged, bad solder connections, etc. are common causes
      * Hardware V2.3.1 and earlier, check for damage to to removable NFC reader.

??? note "Seemingly no power, but is plugged in."

    There are a number of safety fuses in the ACS system that may be triggered.

    **Cause 1: Switch Power Issue**

    * Check the Switch's lights. A red light indicates power OK.
      * If power is OK, continue to Cause 2
    * De-energize the Switch by unplugging it and leaving it to sit for at least 60 seconds
    * Check all fuses in the Switch, replace as needed
    * Check for any hot spots on the PCB
      * If the device has an electronic fuse, it will overheat and shut off
    * Check for any shorts, damage, etc. to the board

    **Cause 2: Power Input Failure**

    * Check the power source (mains voltage, computer, etc.) for operation.
    * Attempt plugging in other devices that are known working, see if they get power.
    * Contact your facilities department is a power issue is found.
    * Contact your IT department is a power issue related to a computer is found.

    **Cause 3: Cable Damage**

    * Swap the ACS Core for another one, see if the issue persists
    * Attempt replacing the cable to see if it resolves the issue

    **Cause 4: Core Fuse Blown**

    * Core V2.3.1 and later has an electronic fuse that will perpetually shut off in a short condition
      * Check for hot spots on the PCB, obvious shorts, damage to the PCB, etc.
    * Core V2.2.x and earlier has a one-shot mechanical fuse that will visibly break in an overcurrent incident.
      * Check for the source of the overcurrent, such as a short-circuit.

??? note "ACS is powered and working, but doesn't activate device."

    **ISSUE: Switch Failure**

    **Cause 1: Blown Fuse**

    * Most mains-voltage Switches have a single-blow fuse on the output.
    * Check the fuse for damage after removing power and giving time for the system to de-energize.

    **Cause 2: Damaged Cable or Core**

    * Check the green light on the Switch. It should be on when the machine is online.
    * If it is not turning on, either the cable (likely) or the Core (unlikely) is damaged.
      * Attempt to replace the part(s) and see if it works

    **Cause 3: Damaged Switching Mechanism**

    * Repeated use, physical impact, or extended exposure to higher temperatures can cause a Switch to fail
    * De-energize the Switch and check the circuit board for damage, shorts, etc.
      * A Switch fault will be very apparent, usually manifesting as burn marks or melted parts.
