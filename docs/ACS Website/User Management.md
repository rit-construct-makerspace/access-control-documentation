# User Management

Each makerspace user has an account on the website, accessible by the shibboleth SSO students use to log in to other university resources. This is used to uniquely identify them and track their progress on trainings and the like.

## Roles

The website supports 3 tiers of user, to restrict access to certain resources and menus.

### Maker Role

The **Maker** is the lowest-permission role, and the default role of new users. It is intended for end-users (e.g. students) of the makerspace. 

Makers can:

* Complete [trainings](./Trainings.md).
* See the requirements to access a piece of equipment in the [equipment view](./Equipment Management.md), and if that equipment is available now.
* View the shop hours, announcements, and other [homepage information](./Homepage.md).
* Access any additional links in the "Maker" sidebar.

###Mentor Role

The Maker Mentor, shortened in most places to **Mentor**, is the role intended for student staff, shop monitors, etc.

In addition to what Makers can do, Mentors can;

* View the shop [history and statistics pages](./History%20&%20Statistics.md) to see records of what's happened in the space.
* See current users, machine states, etc. for all [access control devices](./Equipment Management.md).
* Modify the [inventory](./Inventory%20&%20Sales.md) and process transactions on the [storefront](./Inventory%20&%20Sales.md).
* Edit [equipment parameters](./Equipment%20Management.md).
* Create, edit, and resolve [maintenance logs](./Equipment%20Management.md#maintenance-logs).
* View and edit [trainings](./Trainings.md).
* Approve [equipment sign-offs](./Trainings.md#staff-approval) for users.
* Update [ID card numbers](#id-cards) for users.
* Place [holds](#holds) on user accounts.
* See all of a user's completed [trainings and authorizations](./Trainings.md).
* Remove [training holds](./Trainings.md#training-holds).
* [Checkout and check in tools](./Tool%20Checkout.md) for users.
* Post, edit, and delete [announcements](./Homepage.md#announcements).
* Create or edit [rooms](./Equipment%20Management.md#rooms-and-zones).
* Create or edit [zone hours](./Homepage.md#open-hours).

When a student is a mentor but off-shift, they use the same account for ease of integration with SSO. This also means any trainings they do on shift can be used off shift and vice-versa. Mentors are still restricted to only using equipment they have passed trainings and been approved for. While they are able to self-approve trainings, this generates a flag for review to ensure it is not being abused. Mentors are also required to sign in before using equipment. 

In future ACS firmware updates, only Mentor-role users will be able to change the [ACS state](../ACS%20Hardware/ACS%20Core.md#acs-state). See [ACS Roadmap](../ACS%20Hardware/index.md#roadmap) for more information.

### Staff Role

The **Staff** role is the highest permission level, able to do just about everything.

In addition to what a Mentor can do, Staff can:

* Always unlock ACS-controlled equipment, even if missing trainings, approvals, or sign-in.
* Remove [holds](#holds).
* Change a user's role, including their own.
* Manually create approvals for equipment that has no associated trainings, and/or equipment the user has not completed trainings for.
* Create or edit [tools for checkout](./Tool%20Checkout.md).
* Write and view [user notes](#user-notes).

## User Information

![Image](./assets/make%20user%20info.png){: align=left loading=lazy width="375"}

When a user logs in to the website for the first time and an account is created, some information is pulled down to complete their makerspace account. This includes;

* Preferred first and last name
* University email address
* Preferred pronouns

Users are then manually prompted for;

* Graduation year
* College and major
* Overriding preferred pronouns

All of this information is visible in the "People" section of the site, visible to non-maker users.

The website checks the SSO records every night for changes to preferred name and pronouns, and will update the website records accordingly.

The website is developed to also display a user photo, but this not currently implemented at RIT for privacy reasons. All users have the same placeholder user photo for now.


## ID Cards

To use ACS equipment, users will need a physical NFC card associated with their account. While it makes the most sense to use pre-issued university IDs or similar, there's no reason you couldn't also just issue arbitrary "makerspace ID" cards. 

User verification is done by the NFC Unique ID, or UID for short. Since this is accessed before the encrypted and/or stored data, cards can be used for ACS access that are already programmed and primarily used for other purposes, like door access.

ACS devices are programmed to look for ID Cards that match the right UID length (4, 7, or 10 bytes), as well as optionally match the REQA response and SAK to limit spoofing by other NFC devices. 

The UID of a user's card needs to be manually associated by a [Maker Mentor](#mentor-role) or [Staff](#staff-role) before it will work for ACS. The UID can be entered on the user account, accessible through the "People" page. Scanning an unrecognized card at a sign-in reader will report the UID in the history, so it can be easily copied and pasted into the user profile. Card numbers also need to be updated whenever a new card is issued and/or the old one is lost. Only one UID can be associated with a user, and they cannot share that UID with other users.

![Image](./assets/make%20card%20tag%20setting.png)

## User Notes

[Staff](#staff-role) have the ability to add notes to a user's account on the "People" page. This can be useful for remembering information about a user. All other staff can see and edit the notes. 

## Holds

In the event that a user of the shop does something unsafe or otherwise in violation of lab policies, an account hold can be placed by [Staff](#staff-role) or [Maker Mentors](#mentor-role). When placing a hold, the user will be prompted to add a note with some information. 

When a user has an account hold, the system will stop them from performing most functions. They will no longer be able to use ACS-equipped machines, check out tools, etc. 

Users with holds on their account are sorted to the top of the "People" page and given a red border for easier identification by staff.

An account hold can only be removed by someone with Staff role. The record of the hold (the note, who placed it, who removed it, when, etc.) will be permanently on the user's account.

![Image](./assets/active%20hold.png)

![Image](./assets/removed%20hold.png)