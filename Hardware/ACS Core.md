# ACS Core

[Click here for information on configuring the ACS Core's settings](https://rit-construct-makerspace.github.io/access-control-documentation/Hardware/Deploying%20ACS%20Hardware.html#acs-configuration).

The Core is the heart of an ACS deployment. It has 3 major components;

## Card Reader

The entire premise of ACS works on the idea that users have a unique NFC card, used to unlock equipment. The card is inserted physically into the equipment, and removing the card ends the session. This ensures that a user only starts 1 session, and they don't forget to end their session. 

Card presence is detected by 2 limit switches. Once the switches detect a card's presence, it is read by the NFC reader. 

Right now, only the unencrypted UID is used for user identification and verification.

## Network Communication

An ACS deployment communicates with the server via the Core's network connection. The Core is built around an ESP32-S2, providing a 2.4GHz network connection for most deployment. Deplyoments in areas with poor WiFi performance can instead option for the W5500-based 10/100 ethernet connection.

## User Interface

While basic, the Core does provide some user signaling to show state.

