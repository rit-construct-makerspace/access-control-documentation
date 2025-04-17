# User Management

## Roles

The website supports 3 tiers of user, to restrict access to certain resources and menus.

### Maker Role

The **Maker** is the lowest-permission role, and the default role of new users. It is intended for end-users (e.g. students) of the makerspace. 

Makers can:

* Complete [trainings](./Trainings.md).
* See the requirements to access a piece of equipment in the [equipment view](./Equipment Management.md), and if that equipment is available now.
* View the shop hours, announcements, and other [homepage information](./Homepage.md).
* Access any additional links in the "Maker" sidebar.

### Mentor Role

The Maker Mentor, shortened in most places to **Mentor**, is the role intended for student staff, shop monitors, etc.

In addition to what Makers can do, Mentors can;

* View the shop [history and statistics pages](./History%20&%20Statistics.md) to see records of what's happened in the space.
* See current users, machine states, etc. for all [access control devices](./Equipment Management.md).
* Modify the [inventory](./Inventory%20&%20Sales.md) and process transactions on the [storefront](./Inventory%20&%20Sales.md).
* Edit [equipment parameters](./Equipment%20Management.md).
* Create, edit, and resolve [maintenance logs](./Equipment%20Management.md#mainenance-logs).
* View and edit [trainings](./Trainings.md).
* Approve [equipment sign-offs](./Trainings.md#staff-approval) for users.
* Update [ID card numbers](#id-cards) for users.
* Place [holds](#holds) on user accounts.
* See all of a user's completed [trainings and authorizations](./Trainings.md).
* Remove [training holds](./Trainings.md).
* [Checkout and check in tools](./Tool%20Checkout.md) for users.
* Post, edit, and delete [announcements](./Homepage.md#announcements).
* Create or edit [rooms](./Equipment%20Management.md#rooms-and-zones).
* Create or edit [zone hours](./Homepage.md#open-hours)

When a student is a mentor but off-shift, they use the same account for ease of integration with SSO. This also means any trainings they do on shift can be used off shift and vice-versa. Mentors are still restricted to only using equipment they have passed trainings and been approved for. While they are able to self-approve trainings, this generates a flag for review to ensure it is not being abused. Mentors are also required to sign in before using equipment. 

In future ACS firmware updates, only Mentor-role users will be able to change the [ACS state](../ACS%20Hardware/ACS%20Core.md#acs-state). See [ACS Roadmap](../ACS%20Hardware/index.md#roadmap) for more information.

### Staff Role

The **Staff** role is the highest permission level, able to do just about everything.

In addition to what a Mentor can do, Staff can:

* Remove [holds](#holds).
* Change a user's role, including their own.
* Manually create approvals for equipment that has no associated training, and/or equipment the user has not completed trainings for.
* Create or edit [tools for checkout](./Tool%20Checkout.md).
* Write and view [user notes](#user-notes)