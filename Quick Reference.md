---
nav_order: 3
---

# Quick Reference

This document is a brief reference on the operation and use of the ACS system, with reminders for users and staff on common use-cases.

## Hardware References

### Determining ACS State
* Light on front of the device will indicate device state;
  * **YELLOW**: Idle mode, ready to accept card.
  * **RED**: Lockout or Fault, device unavailable for use.
  * **GREEN**: Unlocked or Always On, device may already be in use (if card present) or unlocked for an event or similar (if no card present).
  * **WHITE**: Idle but no network, will accept cards it has previously read and cached.
  * **WHITE & Another Color Alternating**: Lost network connection, but still in the other state (Lockout, AlwaysOn, Unlocked).
  * **BLUE**: Device has been powered on and needs an initial state set. See "Changing Device Device State" below.
  * **PINK**: Reset button is being held down, restarting in 3 seconds.
  * **RED/GREEN/BLUE Cycling**: Initializing, should be in Startup mode in 30 seconds.
    * If OTA update installing, may take up to 3 minutes.
    * Should only be present on power-on or if a reset is triggered. Otherwise may be a sign of a system issue to investigate.
* ACS State can also be found in the Staff section of the website.
  * Left menu, "Access Devices".
  * Search device name or find in list.

### Accessing a Device

* Check the state of the ACS Core, indicated by the light  
  * See "Determining ACS State" above for determining the state.
  * Only can/should continue if in "Idle" state.
* Insert your card until it hits the stop inside the ACS Code.
  * Card should be inserted >75% of the way.
  * Light should immediately change.
* If authroization granted: light will turn green and a "happy beep" will play.
  * You are now set to use the equipment!
  * Remove your card to end your session when complete.
* If authorization denied: light will turn red and a "sad beep" will play.
  * Speak to a staff member to figure out why you were denied
  * If you have used the equipment before, odds are you forgot to sign in today.

### Changing Device State

The following instructions are for staff and authorized personnel only.
* Identify the current state via the light or website
  * See "Determining ACS State" above for more information.
* If changing to a less restrictive state (Lockout to Idle, Idle to AlwaysOn, etc.), insert your ID card.
  * Wait for a beep to indicate your ID has been authenticated before continuing.
* Identify the current keyswitch position.
  * Small silver arrow in center of switch points to current position.
  * Right (towards button): Idle Set.
  * Up (away from card): AlwaysOn Set.
  * Up and Left (furthest counter-clockwise): Lockout Set.
* Insert the key with the bump lined up with the arrow.
* Turn the key to the position desired.
  * If key was already in right spot, move to another spot, wait for lighting cue to change, return to position.
* Light will update to indicate state.
* Remove key and ID card when complete.

### Didn't find what you're looking for?
See "Hardware Troubleshooting" page for more in-depth diagnostics and uses.

## Website Reference

TODO
