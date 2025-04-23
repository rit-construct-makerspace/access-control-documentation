## History

The History shows everything that has happened in the shop related to ACS in chronological order. It is the most important page for staff to monitor when working. 

The top of the history page has a search bar for finding past records, as well as a date range to narrow results. Only 100 results can be viewed at once, sorted chronologically, newest first.

For further sorting, in the history, different categories of message types can be selectively hidden.

!!! info
    The formatting of the code blocks below doesn't show it, but many of these reports have embedded links to go to relevant sections. For example, clicking on a user's name brings up their [Account Page](./User%20Management.md), or clicking on the machine's name brings up the relevant part of the [Access Devices Page](./Equipment%20Management.md#access-devices).

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

### ACS Status Reports

ACS Status reports are used for reporting notable [status reports](../ACS%20Hardware/API%20Information.md#status) from the [ACS Core](../ACS%20Hardware/ACS%20Core.md). This is predominately used for reporting when a user signs out of a machine, ending a session. The session length is included in the message.

Example:

```
Jim Heaney signed out of horizontalsaw - Horizontal Bandsaw - JET 814GH (Session: 98 sec)
```

### ACS State Change

The ACS State Change is used to indicate an ACS Core is in a state different than the website last recorded. 

If a user's ID was present when the state changed, it is included in the history log.

!!! note
    Since the state change is based on the website's records, it is possible for a Core to quickly swap between states and the website not record them. State changes are also not recorded if there are networking issues with the Core.

Example Format:

```
Jim Heaney changed state of horizontalsaw: Unlocked -> Idle
```

### ACS Messages

As part of the [API](../ACS%20Hardware/API%20Information.md), the ACS Core can send plaintext messages to the server to convey verbose data or reports. These are logged in the history as the ACS Message.

Example Messages:

```
sewing3 message: Restart-Reason-Restart-Button
```

```
horizontalsaw message: INTERNAL_TEMPERATURE_OVERHEAT_AT_50.06
```

### Server Messages

Server Messages report actions undertaken by the website. There are over a dozen different server messages. Some are periodic, some are for logging events, and some are for generating debug data.

Example Messages:


[Training Hold](./Trainings.md#training-holds) Applied:
```
Daily attempt limit reached. A hold has been placed on training Textiles Safety for Jim Heaney
```

[Training API](./Trainings.md#api-triggers) Trigger:
```
Jim Heaney has been automatically added to 3DPrinterOS Workgroup 4670.
```

Daily Server Cleanup:
```
It is now 4:00am. Daily Temp Records have been wiped.
```

### Admin Actions

Admin Actions refer to tasks carried out by staff. 

Examples:

```
Jim Heaney loaned tool item 'Dremel Kit #1' to (Student Name Redacted)
```

```
Jim Heaney updated (Student Name Redacted)'s Card Tag ID.
```

```
Jim Heaney approved the Sewing Machine - Bernina 475 access check for (Student Name Redacted)
```

### Training Submissions

This section shows users completing [Training Quizzes](./Trainings.md#training=quizzes). The user, the quiz, and the score are reported.

Example:

```
Jim Heaney submitted attempt of 3D Printing Self-Serve Training with a grade of 75.
```

### Uncategorized

Any message type that does not fit one of the above categories goes into Uncategorized.

## Statistics

