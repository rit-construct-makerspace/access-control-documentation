# Topology Standards

## Overview

All devices in an ACS deployment must connect to each other in order to share information, power, etc. 

The means by which components are organized and connected is defined below.

## Topology Roles

All devices in an ACS deployment must serve one of the following topology roles;

* **Line Terminator** : (LT), These are devices that have 1 connection to other devices, and act as the end of a *branch* in the deployment.

* **Pass-Through** : (PT), These are devices that have 2 connections to other devices, allowing them to sit between other devices on the *branch*. Pass-through devices may only connect to one device on either side, and cannot specify a connection orientation. 

* **Optional Terminator** : An Optional Terminator (OT) is a device that can act as both a Line Terminator or a Pass-Through device, depending on its self-detected position in the *branch*. For the purposes of this documentation, unless otherwise specified, any reference to a Line Terminator or Pass-Through also applies to an Optional Terminator.

* **Router** : A Router is an optional device that allows the interconnecting of multiple branches. It must act as the end of a *branch*.

## Topology Limits

There are two styles of topologies permitted in an ACS deployment;

* **Single-Branch** deployments must be comprised of 2 LTs on either end of the branch, with up to 6 other PTs. 
* **Multi-Branch** deployments can be comprised of up to 4 branches, each with an LT on one end and no more than 7 PTs, all converging at a single Router. At least one of these devices must be a Core.

There is no minimum size for an ACS deployment, so long as all communication lines are properly terminated or unused.

## Core

The Core is a special device in the ACS deployment's topology. This is defined as the only device in the ACS deployment that can speak to outside sources of truth, such as an Access Control Server. 

In a *Single-Branch* deployment, there can only be one Core. In a *Multi-Branch* deployment, there can be no more than 1 Core per branch, with the master Core being determined as the Core connected to the lowest-number branch of the Router. 

The Core can be designed to act as an LT, PT, or OT device. The Core can act as a Router as well, in which case the Core inherent to the Router always become the master Core. 

Any other Cores become standby Cores. Standby Cores do nothing except maintain an active network connection, and listen to the state of the master Core. If the master Core loses network connection or becomes incapacitated, standby Cores can assert their role as master.