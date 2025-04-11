---
nav_order: 4
---

# Staff Quick Reference

This document is a brief reference on the operation and use of the ACS system, with reminders for staff on common use-cases and issues.

!!! note
    The other sectioncs of this reference website will contain useful information, and it is suggested to familarize yourself with all of it. Especially the in-depth troubleshooting pages in the Hardware and Website sections.Below is just the most common issues and reminders.

## Determining ACS State
Light on front of the device will indicate device state;

  * **YELLOW SOLID**: Idle mode, ready to accept card.
  * **YELLOW FLASHING**: Verifying card, should be approved/denied in a few seconds.
    * We also enter this lighting cue to indicate you've swapped to Idle mode when using a key, with a keycard present. Otherwise it'd be indistinguishable from Always On.
    * If when doing a normal verification it stays in this state for an extended period, may indicate a network failure.
  * **RED SOLID**: Lockout, device unavailable for use.
  * **RED FLASHING**: Fault, device is unavailable for use due to an internal failure. See website History for more information.
  * **GREEN**: Unlocked or Always On, device may already be in use (if card present) or unlocked for an event or similar (if no card present).
  * **WHITE**: Idle but no network, will accept cards it has previously read and cached.
  * **WHITE & Another Color Alternating**: Lost network connection, but still in the other state (Lockout, AlwaysOn, Unlocked).
  * **BLUE**: Device has been powered on and needs an initial state set. See "Changing Device Device State" below.
  * **PINK**: Reset button is being held down, restarting in 3 seconds.
  * **RED/GREEN/BLUE Cycling**: Initializing, should be in Startup mode in 30 seconds.
    * If OTA update installing, may take up to 3 minutes.
    * Should only be present on power-on or if a reset is triggered. Otherwise may be a sign of a system issue to investigate.

ACS State can also be found in the Staff section of the website.

  * Left menu, "Access Devices".
  * Search device name or find in list.

## Changing Device State

The following instructions are for staff and authorized personnel only.

* Identify the current state via the light or website
  * See "Determining ACS State" above for more information.
* On ACS Core Hardware Version 2.3.2 (Shlug):
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
* On ACS Core Hardware Version 2.4.1:
    * TODO
* Remove key and ID card when complete.

## Restarting Device

Preferred Method: Restart Button

  * On ACS Core Hardware V2.3.2 (Shlug):
    * Press and hold button on front for 5 seconds
        * Light should turn pink while pressed
    * When light changes from pink to another color, release
    * Startup will be complete in 30 seconds or less, and the system will return to its original state.
  * On ACS Core Hardware V2.4.1:
    * Press and hold button on back for 5 seconds
        * All lights will turn pink when pressed
    * When light colors change from pink, release
    * Startup will complete in 30 seconds or less, and the system will return to its original state.

Backup Method: Power Cycle (All Versions):

  * Remove power from the ACS Core to do a hard restart
  * Either unplug the DE-9 from the ACS Core, or if accessible remove power from the ACS Switch element.
  * Wait 5 seconds before plugging back in.
  * In a power-on reset, the machine will go to the "Startup" state, and need a key to return to normal operation.

## Why can't (user) access this device? 

* Go to "History" in the Staff section on the left side of the screen.
* Search the user's name to find their history.
* A log in the format "(user) failed to swipe into (machine) with error (error type)" will be present
    * If there is no log, indication of ACS Core network issues.
    * "User required Welcome" : User did not sign in to the space yet today.
    * "User does not exist" : User not signed up for make.rit.edu OR keycard is not registered to their account
    * "Incomplete trainings" : User is missing one or more of the online trainings for that equipment
        * The missing training(s) will be listed in the log too.
        * If it lists no training missing, it means there is a hold on the account.
    * "Missing Staff Approval" : User is missing in-person sign off on that equipment, but has all trainings done.

