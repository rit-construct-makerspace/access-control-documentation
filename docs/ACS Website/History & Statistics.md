## History

The History shows everything that has happened in the shop related to ACS in chronological order. It is the most important page for staff to monitor when working. The following are reported in the history:

### Sign-In Attempts

Users signing into a space are logged in the history, so you can match a face to a name. 

Users with ID cards that are not recognized are reported with the following message:

```
UID Click to Reveal failed to swipe into Atrium - Wood Shop, Atrium - Manual Shop, Atrium - South Shop with error 'User does not exist'
```
Pressing "Click to Reveal" will show the card UID, for the staff to copy in and update the [ID Card number](./User%20Management.md#id-cards).

#### Manual Sign-In

If a user wants to sign in to a space but they don't have their ID, or the [Sign-In Reader](../ACS%20Hardware/ACS%20Tertiary%20Components.md#sign-in-reader) is not working, staff can manually sign them in by pressing "Manual Room Sign-In" in the top right corner of the screen. The staff will then need to enter the user's email, and select the room to sign them into.

![Image](./assets/make%20manual%20sign%20in.png)

### Equipment Activation Attempts

When a user attempts to activate a piece of equipment, both the user and equipment are reported in the history. 

If the activation was not successful, the reasoning is reported;

* "User requires Welcome" - User did not sign in to the space today.
* "User does not exist" - User's UID could not be found in the system.
* "Incomplete Trainings" - User is missing one or more of the [Training Modules](./Trainings.md#training-quizzes). 
    * The missing training(s) will be listed in the history log as well.
    * If the user has a [hold](./User%20Management.md#holds) on their account, this is the error message used but with no trainings listed.
* "Missing Staff Approval" - User does not have the [staff approval](./Trainings.md#staff-approval) for that equipment.

If the user is a [Staff](./User%20Management.md#staff-role) role, the history record will indicate they were authorized because of their Staff access.

## Statistics