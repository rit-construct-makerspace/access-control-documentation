# Equipment Management

Managing equipment is one of the main functions of the website. Multiple tools and resources are developed to assist in that;

## Equipment Page - Maker

[Makers](./User%20Management.md#maker-role) can see all shop equipment on the "Equipment" page. This shows the same information as the [Homepage Zone Tabs](./Homepage.md#zone-views).

Each piece of equipment has a green icon that can be pressed to go to the SOP for that equipment. Otherwise, clicking the equipment card brings up more information about the equipment.
 
In the bottom right of the equipment card are icons indicating if the [training quizzes](./Trainings.md#training-quizzes) are complete (and if not how many are left outstanding), and if the user has their [staff approval](./Trainings.md#staff-approval). 

![Image](./assets/make%20equipment%20page.png)

Clicking on the equipment brings up a page with more details on the equipment, as seen below. This shows the room it is in, trainings required for the equipment (click on an incomplete one to go to it), and if you have your staff approval.

The Standard Operating Procedure (SOP) link is placed prominently at the top of this page for easy access.

At the bottom you can see how many pieces of equipment are in the shop and how many are in use.

![Image](./assets/make%20equipment%20details.png)

## Equipment Page - Staff

In addition to the maker's equipment page, [Maker Mentor](./User%20Management.md#mentor-role) and [Staff](./User%20Management.md#staff-role) can see a second equipment page, titled "Manage Equipment". This page allows users to edit equipment information or make new equipment.

[!Image](./assets/make%20manage%20equipment.png)

The bottom left corner of each equipment card has the equipment's type ID, used in both the [ACS Configuration](../ACS%20Hardware/ACS%20Configuration.md) and the [API](../ACS%20Hardware/API%20Information.md).

Each equipment card has 3 critical buttons at the bottom;

* The **Green Paper** is a link to the SOP.
* The **Blue Speech Bubble** opens the [Issue Log](#maintenance-logs).
* The **Red Downward Arrow** hides the equipment. 

Hiding equipment puts it into the "Hidden Equipment" section at the bottom of the page. Hidden equipment works like normal equipment, but is not visible to makers. Mentors will not be able to create approvals for hidden equipment, but Staff will be able to. Users with approval on hidden equipment can still use it as-is.

Clicking on a piece of equipment brings up more information;

![Image](./assets/make%20manage%20equipment%20details.png)

On this page you can;

* Rename device.
* Provide new image URL.
* Provide new SOP URL.
* Change location (room).
* Add notes - visible to all other staff.
* Toggle "Available by reservation only?" to hide ACS availability and direct users to email in to use.
* Attach or remove [Training Modules](./Trainings.md#training-quizzes).

At the top of the page, you have buttons to;

* View Logs: See the [history](./History%20&%20Statistics.md#history) filtered to just this machine type.
* Hide: Hide the equipment
* View Issue Logs: Open the [Issue Log](#maintenance-logs).
* Manage Instances: Create new instances of the equipment for use in the [Issue Log](#maintenance-logs).

### Maintenance Logs

To aid in managing equipment issues, a maintenance log / issue log system is build into the website. From the [Staff Equipment Page](#equipment-page-staff), select the "Issue Log" button to access the logs. 

![Image](./assets/make%20issue%20log.png)

This page will show current reported issues, along with when they were reported, what specific machine instance the issue is with, and who reported it. Issues can be tagged for easier sorting, management, or statistics.

At the bottom of the page, you can select an instance and write a brief report to post an issue log. 

If an issue report is no longer needed, the red trash can icon can be used to delete it. If an issue has been resolved, the blue arrow can be pressed to send the issue to the **Resolution Log**.

At the top of the page, you can manage instances, as well as go to the **Resolution Log**.

!!! info
    An "Instance" refers to a unique machine. This is separate from an ACS unique deployment so that non-ACS equipment like 3D printers can have maintenance logs. This also allows ACS to be independent of a machine. 
    
    For instance, at the SHED we have 4 sewing stations, but 5 sewing machines. We swap out the 5th if one of the other ones goes down. This means the machines are constantly moved, but the ACS stays in place.


In the **Resolution Log**, you can see past equipment issues, as well as a brief resolution. On this page, you can also manage (edit, delete, create, etc.) the tags used to track issues and resolutions.

![Image](./assets/make%20resolution%20log.png)

## Rooms and Zones

The makerspace is divided into areas called **Rooms**. Equipment must be assigned to a room to function. This room information is used for a few key functions;

* The reported room a piece of equipment is is in used to check if a user has signed in before granting access.
* Grouping machines to display on the [Homepage](./Homepage.md).
* Sorting [History](./History%20&%20Statistics.md#history) to see only things relevant to a single space.

Clicking on a room shows more information about it, such as recent sign-ins, equipment, etc.

![Image](./assets/make%20room%20list.png)

**Zones** refer to an area comprised of one or many rooms. Zones are used as an administrative tool, and do not directly impact ACS. Zones are used for;

* Statistics.
* Open Hours.
* Grouping rooms to display on the [Homepage](./Homepage.md).

![Image](./assets/make%20zone%20list.png)

Rooms and Zones can be edited on the "Rooms" page on the website, accessible to Maker Mentors and Staff.

The ID number next to each room is used in [ACS Configuration](../ACS%20Hardware/ACS%20Configuration.md) as well as when using the [API](../ACS%20Hardware/API%20Information.md) to sign in to a space. 

The ID number next to each zone is used in the [API](../ACS%20Hardware/API%20Information.md) to determine zone hours.

## Access Devices

The **Access Devices** page is available to staff roles, and shows all [ACS Cores](../ACS%20Hardware/ACS%20Core.md) that have connected to the website. Each Core has a card, and can be searched for by MachineID at the top of the screen.

![Image](./assets/make%20access%20devices.png)

Each card shows the following information;

* MachineID
* Machine Type
* Device ID (used internally for database)
* Zone(s) the machine is in
* Machine name from [Equipment](#equipment-page-staff)
* Maximum reported temperature in the ACS deployment
* Current [ACS State](../ACS%20Hardware/ACS%20Core.md#acs-state)
* Time since last [status report](../ACS%20Hardware/API%20Information.md#status), how frequently reports should come in, and the source of the report.
* Last user, or current user if equipment currently in use.
* BE (Back End) firmware version, FE (Front end) firmware version, and HW (hardware) revision.

In the event of a device overtemperature issue, the card is tinted red and moved to the top of the list.