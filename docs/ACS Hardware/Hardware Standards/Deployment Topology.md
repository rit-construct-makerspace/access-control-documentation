# Topology Standards

## Overview

All devices in an ACS deployment must connect to each other in order to share information, power, etc. 

The means by which components are organized and connected is defined below.

## Topology Roles

All devices in an ACS deployment must serve one of the following topology roles;

* **Line Terminator** : (LT), These are devices that have 1 connection to other devices, and act as the end of a *branch* in the deployment.

* **Passive Line Terminator** : (PLT), These are devices without electronics that can be plugged into a connector to terminate it.

* **Pass-Through** : (PT), These are devices that have 2 connections to other devices, allowing them to sit between other devices on the *branch*. Pass-through devices may only connect to one device on either side, and cannot specify a connection orientation. PT devices must clearly label they cannot operate with an open connector.

* **Optional Terminator** : An Optional Terminator (OT) is a device that can act as both a Line Terminator or a Pass-Through device, depending on its self-detected position in the *branch*. For the purposes of this documentation, unless otherwise specified, any reference to a Line Terminator or Pass-Through also applies to an Optional Terminator.

* **Router** : A Router is an optional device that allows the interconnecting of multiple branches. It must act as the end of a *branch*, and can have up to 4 branch connectors.

## Topology Limits

There are relatively few limits on a deployment;

* No more than 62 devices across the deployment, including the Core and any Routers.
* Cumulative cable lengths exceeding 20 feet on a branch may lead to degraded performance. 

Deployments are comprised of one or more *branches*. A branch is defined as a straight-line connection of devices, with an LT or OT on each end. A branch can also end at a Router, permitting multiple branches to join on a deployment. Routers can continuously be waterfalled to create infinite branches, so long as device counts are respected.

There is no minimum size for an ACS deployment, so long as all communication lines are properly terminated or unused.
