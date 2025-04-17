# Inventory & Sales

One of the secondary features of the ACS Website is managing inventory of materials and consumables, both for internal use and for sale.

Most aspects of the inventory system are only accessible to "Mentor" or "Staff" roles. "Maker" roles can see what items are listed in the store available on the storefront for purchase.

## Inventory System Features

* Track both internal and for-sale materials, consumables, etc. for the shop.
* Tag materials for easier sorting by location, use-case, etc.
* Add dollar values to materials to understand real cost of inventory.
* Set low stock thresholds to see when items need to be re-ordered.
    * Set up notifications via email or Slack so you don't miss low-stock alerts.


## Step-By-Step Guides

### Checking Out Materials for Internal Use

* Go to "Storefront" on left side of screen.
* Click "Show Internal Items" to set to internal use mode.
* Search for the item you need in the search bar.
* Click on a material and add the quantity to your cart.
* Once your cart is set, hit the orange "Use" button.
    * If you see a blue "Checkout" button instead, you forgot the "Show Internal Items" toggle.
* Enter the reason for your usage.

### Bulk-Modifying Material Quantities

* This should be used when doing an inventory audit, receiving incoming shipments, etc.
* Select "Material Inventory" on the left side of the screen.
* Find the material you want to edit.
  * Be careful, many materials have similar names!
* Hit the edit icon (pencil) on the far right of the screen.
* Adjust the "Count" field to change how many are in inventory.
  * See [Adding New Item to Inventory](#adding-new-item-to-inventory) below for details on the other fields.
* Hit "Save" to save your changed.

### Adding New Item to Inventory

* Go to "Material Inventory" on the left side of the screen.
* Hit "New Material" under the "All Materials" header.
* Enter the Name of the Material.
  * Try to look for other similar materials in inventory and follow the naming scheme.
  * Ex: 3D Printer Filament is named (Special Qualifier) (Material) (Color) (Added Qualifier).
    * A 1kg 1.75mm spool of orange PLA is just "PLA Orange", since it is the standard.
    * A 3.5kg spool of orange PLA is "3.5kg PLA Orange" to differentiate it.
    * A 10kg spool of 2.85mm orange PLA for the BigRep only is "2.85mm 10kg PLA Orange BigRep".
   * Ex: Laser Cutter materials are named (Thickness) (Dimension) (Material) (Color).
     * 1/8" 12x20 MDF
     * 1/4" 12x12 Acrylic Clear
   * Generic Components are named for their most relevant characteristic first.
     * "LM2596 Buck Converter" instead of "Buck Converter - LM2596"
     * Puts the most important details (the central chip and the topology) first.
* Enter the unit names:
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

### Selling Materials

* Go to "Storefront" on left side of screen.
* Select the proper materials and quantities from the list based on the customer's wants.
* When ready, press "Checkout" at the top.
* Open the Tiger Bucks Portal with the link provided.
  * Complete the transaction for the listed total amount using the payment portal.
* Once the transaction is complete, select the customer from the dropdown list back on the Storefront page.
  * You can optionally also add notes here to be included in the ledger entry.
* Hit "Update Inventory" to complete transaction.
