# API Information

The API is what links the Access Control Hardware to the [Website](../ACS%20Website/index.md). API calls are primarily made by the [Core](./ACS%20Core.md) and the [Sign-In Reader](./ACS%20Tertiary%20Components.md#sign-in-reader), but any device (even non-ACS ones) can make API calls.

The API is under the /api subdomain. Ex: "https://make.rit.edu/api/welcome".

!!! note
    More specific documentation on the API's implementation can be found in the documentation for the [ACS Hardware](https://github.com/rit-construct-makerspace/access-control-hardware/wiki/API-Information) and the [ACS Website](https://github.com/rit-construct-makerspace/makerspace/wiki/Machine-API).

!!! warning
    For calls that PUT a JSON, the header must include **"Content-Type", "application/json"** to be properly parsed by the server.

## API Parameters

The following parameters are used in the API calls. Click on an item to expand for information.

??? "Zone"
    **Zone** is the numerical representation of the area that a machine is located in. This is used for statistics and mandating sign-in to specific areas if the ACS is configured for "NeedsWelcome".

    Multiple Zones can be included by separating them by commas. This is useful for the [Welcome](#welcome) API call, to sign a user into multiple zones at once.

    **Rooms** are different from Zones!
    
    Further Reading:
    
    * [ACS Configuration](./ACS%20Configuration.md) for information on NeedsWelcome.
    * [Statistics](../ACS%20Website/History%20&%20Statistics.md)
    * [Rooms and Zones](../ACS%20Website/Equipment%20Management.md#rooms-and-zones) for more information on Rooms and Zones and how to identify their number.

??? "NFC UID"
    **NFC UID** is the unique identifier of a user, from their NFC card. It is usually shortened to "ID" in API calls.

    See [ID Cards](../ACS%20Website/User%20Management.md#id-cards) for more information.

??? "API Key"
    The **API Key** usually shortened to "Key" in API calls, is a unique secret key used to verify the source of API calls. Not all API calls need the API key.

    See [this page](https://github.com/rit-construct-makerspace/makerspace/wiki/Environment-Variables#api_key) on the ACS Website Documentation for more information.

??? "Machine Type"
    **Machine Type** is the numerical representation of the *type* of equipment (not the unique piece of equipment). 
    
    Example: All drill presses in the shop are machine type 8.

    See [Equipment Management](../ACS%20Website/Equipment%20Management.md) for more information.

??? "Machine Name"
    ** Machine Name** is the plaintext representation of that *unique piece* of equipment (not the machine type).

    Example: All drill presses in the shop are machine type 8, but this one is "DrillPress1" and that one is "DrillPress2".

    Names are used for referring to the device in the [History](../ACS%20Website/History%20&%20Statistics.md) as well as the [Access Devices Page](../ACS%20Website/Equipment%20Management.md#access-devices).

    Names are limited to only character permitted in a URL, except for "?" (question mark), "=" (equal sign), and "&" (ampersand). Names are case-sensitive.

## Welcome

**Type: PUT to /api/welcome**

Welcome is the only API call used by the [Sign=In Reader](./ACS%20Tertiary%20Components.md#sign-in-reader). It is used to indicate that a user has "checked-in" to that area of the lab. This is required before using ACS equipment that day configured with the [NeedsWelcome](./ACS%20Configuration.md) parameter.

JSON Example Payload:
``` json
    {
        "Type":"Welcome",
        "Zone":"4,7,8",
        "ID":"123456789",
        "key":"Super-Secret"
    }
```

!!! warning
    The header of the PUT must include **"Content-Type", "application/json"** to be properly parsed by the server.

Response:

* HTTP 403 on key mismatch
* HTTP 406 if user not in system (UID not recognized)
* HTTP 202 if user in system and successfully signed in.

!!! note
    Signing in multiple times in the same day has no impact on ACS, and the Website will not count more than the first sign-in in that zone towards user statistics. If multiple "Zone" numbers are included though, the user signing into each of these counts as a unique visit. See [Statistics](../ACS%20Website/History%20&%20Statistics.md) for more information.

## Authorization

**Type: GET from /api/auth**

Authorization is the most critical call from the [Core](./ACS%20Core.md), used to verify if a user is authorized to use the equipment. All criteria to use the machine that day must be satisfied. See [Trainings](../ACS%20Website/Trainings.md) for more information.

URL Parameters:

* **id**: NFC UID
* **needswelcome**: See [ACS Configuration](./ACS%20Configuration.md)
* **zone**: Zone
* **type**: Machine Type

URL Formatted:
``` 
    https://make.rit.edu/api/auth?id=123456789&needswelcome=1&zone=4&type=6
```

Response:

* HTTP 400 if missing parameters
* HTTP 406 if user or equipment not found in system
* HTTP 401 if there is an issue, including;
    * user requires welcome but did not sign in to this zone yet.
    * user has not completed all trainings associated with this equipment.
    * user is missing their [staff approval](../ACS%20Website/Trainings.md#staff-approval) for this equipment.
* HTTP 202 if all criteria are met, or user has [Staff Role](../ACS%20Website/User%20Management.md#staff-role).

and JSON Payload:

``` json
    {
        "Type":"Authorization",
        "Machine":"6",
        "UID":"123456789",
        "Allowed":"1",
        "Role":"Staff",
        "Reason":"unknown-uid"
    }   
```

* **Allowed** is 1 if all criteria are met (or Staff Role), 0 otherwise.
* **Reason** is why authorization was denied (not included if authorization granted).
    * unknown-uid: UID not recognized.
    * unknown-machine: Machine Type not recognized.
    * missing-training: [Online trainings](../ACS%20Website/Trainings.md) incomplete, or user has an [account hold](../ACS%20Website/User%20Management.md#holds).
    * no-approval: Have not completed [staff approval](../ACS%20Website/Trainings.md#staff-approval).
* **Role** is the [user's role](../ACS%20Website/User%20Management.md#roles) on the website.

## Status

**Type: PUT to /api/status**

ACS devices regularly report their status (see "Frequency" in [ACS Configuration](./ACS%20Configuration.md)) to the website, allowing for real-time visualization of shop utilization and machine states. It also serves as a way to keep the API connection open for re-use.

The first status report is what creates a record of the device in the database.

JSON Example Payload:

``` json
    {
        "Type":"Status",
        "Machine":"186bx1",
        "MachineType":"12",
        "Zone":"7",
        "Temp":"28.123",
        "State":"Unlocked",
        "UID":"123456789",
        "Time":"60",
        "Source":"Scheduled",
        "Frequency":"10",
        "Key":"Super-Secret"
    }
```

* **Machine**: Machine Name
* **MachineType**: Machine Type
* **Zone**: Zone
* **Temp**: Maximum temperature measured on the ACS deployment, in degrees Celsius (3 decimal float).
* **State**: Current system state. See [ACS State](./ACS%20Core.md#acs-state).
* **UID**: NFC UID, blank if no UID to report, reports last UID if after session is ended.
* **Time**: Session time, in seconds. 0 if no session active.
* **Source**: The reason for the status report;
    * Startup: System initialization complete.
    * Temperature: Overtemperature fault.
    * Session Start: User session started.
    * Card Removed: Session ended by removing card.
    * State Change: System state has changed
    * Scheduled: Regular scheduled time for a report.
    !!! note
        Multiple reports with different states can rapidly be sent in succession. For instance, when a card is removed a "Card Removed" status is sent, as well as a "State Change" because the state would have changed.

Response:

* HTTP 403 if key mismatch
* HTTP 400 if card reader not in system and could not be created
* HTTP 200 on success.

## Message

**Type: PUT to /api/message/(MachineName)**

Message allows ACS devices to send plaintext messages to the server, displayed in the [History](../ACS%20Website/History%20&%20Statistics.md). This can be used for more verbose error logging, providing more details on a fault, etc. 

Messages must be sent by an ACS device that has a Machine Name.

Messages are case-sensitive, and can only contain characters allowed in a URL except "?" (question mark), "=" (equal sign), and "&" (ampersand). 

!!! note
    Even though this is a "PUT" call, no payload is actually PUT to the server.

URL Parameters:

* MachineName: Machine Name
* message: The message to log.

URL Formatted:

```
    https://make.rit.edu/api/message/MachineName?message=This-Is-A-Message-About-Documentation
```

Response:

* HTTP 400 on missing parameter.
* HTTP 200 on success.

## Check

**Type: GET from /api/check/(MachineName)**

Check is a simple call that can be used to check if the server is still responding. It is often used to keep a server connection alive for faster response. 

Machine Name is included so that the server knows this machine is still alive and working. An invalid machine name can be included and the response would still be the same.

URL Parameters: 

* MachineName: Machine Name

URL Formatted:

```
    https://make.rit.edu/api/check/MachineName
```

Response:

* HTTP 200 if success

## State

**Type: GET from /api/state/(MachineName)**

Returns the current state of a piece of equipment. Implemented for status display pages.

URL Parameters:

* MachineName: Machine Name

URL Formatted: 

```
    https://make.rit.edu/api/state/MachineName
```

Response:

* HTTP 400 if machine not found in system
* HTTP 200 on success

and JSON Payload: 

``` json
    {
        "Type":"State",
        "MachineID":"kmxmill1",
        "State":"Lockout"
    }
```

* MachineID: Machine Name
* State: [ACS System State](./ACS%20Core.md#acs-state).

## Hours

**Type: GET from /api/hours/(Room)**

This API call allows you to get the hours for the specified area. See [Rooms and Zones](../ACS%20Website/Equipment%20Management.md#rooms-and-zones) for more information.

Formatted URL:

```
    https://make.rit.edu/api/hours/36
```

Response: 

* HTTP 200 (always)

and JSON Payload:

!!! warning
    A blank JSON payload will be returned with an invalid room number, like this;
    ``` json
    {"text":"","obj":[]}
    ```

!!! info
    "dayOfTheWeek" is 1 for Sunday, 2 for Monday, etc.

``` json
    {
  "text": "Sunday OPEN: 09:00:00\nSunday CLOSE: 21:00:00\nMonday OPEN: 09:00:00\nMonday CLOSE: 22:00:00\nUndefined OPEN: 09:00:00\nUndefined CLOSE: 22:00:00\nUndefined OPEN: 09:00:00\nUndefined CLOSE: 21:00:00\nUndefined OPEN: 09:00:00\nUndefined CLOSE: 22:00:00\nUndefined OPEN: 09:00:00\nUndefined CLOSE: 20:00:00\nUndefined OPEN: 09:00:00\nUndefined CLOSE: 17:00:00\n",
  "obj": [
    {
      "id": 370,
      "type": "OPEN",
      "dayOfTheWeek": 1,
      "time": "09:00:00",
      "zoneID": 36
    },
    {
      "id": 371,
      "type": "CLOSE",
      "dayOfTheWeek": 1,
      "time": "21:00:00",
      "zoneID": 36
    },
    {
      "id": 333,
      "type": "OPEN",
      "dayOfTheWeek": 2,
      "time": "09:00:00",
      "zoneID": 36
    },
    {
      "id": 334,
      "type": "CLOSE",
      "dayOfTheWeek": 2,
      "time": "22:00:00",
      "zoneID": 36
    },
    {
      "id": 335,
      "type": "OPEN",
      "dayOfTheWeek": 3,
      "time": "09:00:00",
      "zoneID": 36
    },
    {
      "id": 336,
      "type": "CLOSE",
      "dayOfTheWeek": 3,
      "time": "22:00:00",
      "zoneID": 36
    },
    {
      "id": 338,
      "type": "OPEN",
      "dayOfTheWeek": 4,
      "time": "09:00:00",
      "zoneID": 36
    },
    {
      "id": 339,
      "type": "CLOSE",
      "dayOfTheWeek": 4,
      "time": "21:00:00",
      "zoneID": 36
    },
    {
      "id": 340,
      "type": "OPEN",
      "dayOfTheWeek": 5,
      "time": "09:00:00",
      "zoneID": 36
    },
    {
      "id": 341,
      "type": "CLOSE",
      "dayOfTheWeek": 5,
      "time": "22:00:00",
      "zoneID": 36
    },
    {
      "id": 366,
      "type": "OPEN",
      "dayOfTheWeek": 6,
      "time": "09:00:00",
      "zoneID": 36
    },
    {
      "id": 367,
      "type": "CLOSE",
      "dayOfTheWeek": 6,
      "time": "20:00:00",
      "zoneID": 36
    },
    {
      "id": 368,
      "type": "OPEN",
      "dayOfTheWeek": 7,
      "time": "09:00:00",
      "zoneID": 36
    },
    {
      "id": 369,
      "type": "CLOSE",
      "dayOfTheWeek": 7,
      "time": "17:00:00",
      "zoneID": 36
    }
  ]
}
```