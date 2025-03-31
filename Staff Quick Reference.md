---
nav_order: 4
---

# Staff Quick Reference

This document is a brief reference on the operation and use of the ACS system, with reminders for staff on common use-cases and issues.

See "Hardware Troubleshooting" page for more in-depth diagnostics and uses of the ACS Hardware

See "Website Troubleshooting" page for diagnosing issues with the Website.

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

### Restarting Device
* Preferred Method: Restart Button
 * On ACS Core Hardware V2.3.2 (Shlug), press and hold button for 5 seconds
 * Light should turn purple while pressed
 * When light changes colors, release
 * Startup will be complete in 30 seconds or less, and the system will return to its original state.
* Backup Method: Power Cycle
 * Remove power from the ACS Core to do a hard restart
 * Either unplug the DE-9 from the ACS Core, or if accessible remove power from the ACS Switch element.
 * Wait 5 seconds before plugging back in.
 * In a power-on reset, the machine will go to the "Startup" state, and need a key to return to normal operation.

### Didn't find what you're looking for?
See "Hardware Troubleshooting" page for more in-depth diagnostics and uses.

## Website Reference

### Why can't I/(user) access this device? 

Users:
 * Did you remember to sign in today?
 * Was the equipment in the Idle (Yellow light) state? If not it may be offline.
 * Click "Equipment" on the left side of the screen.
 * Find the piece of equipment you want to use and click on it.
 * Check that all trainings are marked as taken, and that the Mentor Training Approval is complete.
 
![image](https://github.com/user-attachments/assets/f834bb08-7966-49d7-ae9d-f1f776be02ff)

Staff:
 * Go to "History" in the Staff section on the left side of the screen.
 * Search the user's name to find their history.
 * A log in the format "(user) failed to swipe into (machine) with error "(error type)" will be present
  * If there is no message, indication of ACS Core network issues.
  * "User required Welcome" : User did not sign in to the space yet today.
  * "User does not exist" : User not signed up for make.rit.edu OR keycard is not registered to their account
  * "Incomplete trainings" : User is missing one or more of the online trainings for that equipment
    * The missing training(s) will be listed in the log too.
    * If it lists no training missing, it means there is a hold on the account.
  * "Missing Staff Approval" : User is missing in-person sign off on that equipment, but has all trainings done.
 
### Internal Inventory System (Staff Only)

Checking Out Materials for Internal Use:
* Go to "Storefront" on left side of screen.
* Click "Show Internal Items" to set to internal use mode.
* Search for the item you need in the search bar.
  * If the material has a barcode on it, scanning the barcode will populate the search bar automatically.
* Click on a material and add the quantity to your cart.
* Once your cart is set, hit the orange "Use" button.
  * If you see a blue "Checkout" button instead, you forgot the "Show Internal Items" toggle.
* Enter the reason for your usage.

Bulk-Modifying Material Quantities:
* This should be used when doing an inventory audit.
* Select "Material Inventory" on the left side of the screen
* Find the material you want to edit
  * Be careful, many materials have similar names!
* Hit the edit icon (pencil) on the far right of the screen.
* Adjust the "Count" field to change how many are in inventory.
  * See "Adding New Inventory Item" below for details on the other fields.
* Hit "Save" to save your changed.

Adding New Item to Inventory
* Go to "Material Inventory" on the left side of the screen.
* Hit "New Material" under the "All Materials" header
* Enter the Name of the Material
  * Try to look for other similar materials in inventory and follow the naming scheme.
  * Ex: 3D Printer Filament is named (Special Qualifier) (Material) (Color) (Added Qualifier)
    * A 1kg 1.75mm spool of orange PLA is just "PLA Orange", since it is the standard
    * A 3.5kg spool of orange PLA is "3.5kg PLA Orange" to differentiate it
    * A 10kg spool of 2.85mm orange PLA for the BigRep only is "2.85mm 10kg PLA Orange BigRep"
   * Ex: Laser Cutter materials are named (Thickness) (Dimension) (Material) (Color)
     * 1/8" 12x20 MDF
     * 1/4" 12x12 Acrylic Clear
   * Generic Components are named for their most relevant characteristic first
     * "LM2596 Buck Converter" instead of "Buck Converter - LM2596"
     * Puts the most important details (the central chip and the topology) first.
* Enter the unit names
  * Single unit, what to call one of this object
    * "Unit", "Spool", "Shirt", etc.
  * Plural unit, what to call many of this object
    * "Units", "Spools", "Shirts", etc.
* Price per unit: How much each is worth for internal items, or how much things are sold for for storefront items.
* Count: How many are currently in inventory
* Threshold: How many are in inventory when we need to re-order.
* Notes: Public-facing information on the item.
* Hit Save to create the item.
* Find the material in the "All Materials" list
* Change the toggle for "Available on Storefront" if it is something we sell.
* Add a tag with the "+" icon under the "Tags" header.

Adding Incoming Material to Inventory
* See Jim for how to use the label generator software
* TODO

### Selling Materials (Staff Only)

* Go to "Storefront" on left side of screen.
* Select the proper materials and quantities from the list based on the customer's wants.
* When ready, press "Checkout" at the top.
* Open the Tiger Bucks Portal with the link provided.
  * Complete the transaction for the listed total amount using the payment portal.
* Once the transaction is complete, select the customer from the dropdown list back on the Storefront page.
  * You can optionally also add notes here to be included in the ledger entry.
* Hit "Update Inventory" to complete transaction.
