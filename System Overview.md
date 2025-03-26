# System Overview

## Executive Summary

The SHED Access Control System, shortened to ACS, is a solution developed to manage access to equipment in the RIT SHED Makerspace. The system manages training content for equipment, grants access to equipment based on these trainings, and gathers statistics on makerspace usage. The system also provides many secondary tools for managing the shop, such as inventory control, event scheduling, and machine maintenance logging. The ACS system is comprised of 2 main parts;

The website is a REACT app that runs on Heroku. It provides the main interface for staff and students to access online. For students, it is where they can access training resources and safety quizzes, see the current availability of equipment, see hours and announcements, and more. For the staff, the website allows for management of all users of the makerspace including granting and revoking equipment access, records and stastics on shop utilization, inventory control, and maintenance ticket/resolution tracking. 

The website can be configured to work with a wide range of APIs for added functionality. In RIT's instance, the univeristy's standardized SSO was integrated for user identification, and 3DPrinterOS was tied in for managing access to 3D printers. The website also provices a standard RESTful API for the second half of the system, the Access Control Hardware.

The Access Control Hardware is what limits access to equipment. Each machine is equipped with a card reader, known as the Core, that will consult with the website over 2.4GHz WiFi or 10/100 Ethernet to verify a user's identity and to determine if they are authorized to use this equipment. Upon receiving approval, the Core will activate the machine through a modular switching interface. Different switch interfaces are developed for differing machine requirements; from simple mains power switches to more complex options such as controlled USB hubs or machine-specific interlocks. All system power is passively drawn through the Switch, minimizing deployment complexity and enabling wide compatability. The Core can interface with further secondary components via a standardized interface.
