# API Information

The API is what links the Access Control Hardware to the [Website](../ACS%20Website/index.md). API calls are primarily made by the [Core](./ACS%20Core.md) and the [Sign-In Reader](./ACS%20Tertiary%20Components.md#sign-in-reader), but any device (even non-ACS ones) can make API calls.

The API is under the /api subdomain. Ex: "https://make.rit.edu/api/welcome".

!!! note
    More specific documentation on the API's implementation can be found in the documentation for the [ACS Hardware](https://github.com/rit-construct-makerspace/access-control-hardware/wiki/API-Information) and the [ACS Website](https://github.com/rit-construct-makerspace/makerspace/wiki/Machine-API).

## API Parameters

The following parameters are used in the API calls. Click on an item to expand for information.

??? "Zone"
    **Zone** is the numerical representation of the area that a machine is located in. This is used for statistics and mandating sign-in to specific areas if the ACS is configured for "NeedsWelcome".

    Multiple Zones can be included by separating them by commas. This is useful for the [Welcome](#welcome) API call, to sign a user into multiple zones at once.
    
    Further Reading:
    
    * [ACS Configuration](./ACS%20Configuration.md) for information on NeedsWelcome.
    * [Statistics](../ACS%20Website/History%20&%20Statistics.md)
    * [Zones](../ACS%20Website/Equipment%20Management.md#rooms-and-zones) for more information on Zones and how to identify a Zone's number.

??? "NFC UID"
    **NFC UID** is the unique identifier of a user, from their NFC card. It is usually shortened to "ID" in API calls.

    See [ID Cards](../ACS%20Website/User%20Management.md#id-cards) for more information.

??? "API Key"
    The **API Key** usually shortened to "Key" in API calls, is a unique secret key used to verify the source of API calls. Not all API calls need the API key.

    See [this page](https://github.com/rit-construct-makerspace/makerspace/wiki/Environment-Variables#api_key) on the ACS Website Documentation for more information.

??? "Machine Type"
    **Machine Type** is the numerical representation of the *type* of equipment (not the unique piece of equipment).

    See [Equipment Management](../ACS%20Website/Equipment%20Management.md) for more information.

??? "

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
    * unknown-uid: UID not recognized 
    * unknown-machine: Machine Type not recognized
    * missing-training: [Online trainings](../ACS%20Website/Trainings.md) incomplete, or user has an [account hold](../ACS%20Website/User%20Management.md#holds)
    * no-approval: Have not completed [staff approval](../ACS%20Website/Trainings.md#staff-approval)
* **Role** is the [user's role](../ACS%20Website/User%20Management.md#roles) on the website.

## Status

TODO https://github.com/rit-construct-makerspace/makerspace/wiki/Machine-API