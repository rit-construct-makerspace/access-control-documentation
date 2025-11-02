# Router

The Router is responsible for branching the normally linear bus of the ACS deployment into up to 4 branches. 

The Router must receive and re-drive the CAN signal across all 4 branches. The Access, Shutdown, and Interrupt signals do not need to be re-driven. 

The Router is not addressed or enumerated like a normal device. Any messages to the Router are sent to address 0x00, and the Router only responds to Router-specific queries. 

The Router does not participate in voltage monitoring and reporting, and does not have to request a power level, but is not permitted to draw more than 0.75 watts from the bus. 

An offline Router would mean the inability for proper communication across the deployment. Therefore, the Router must be designed in such a way that it is normally asserting the Interrupt, and when powered and operating properly, it de-asserts it. 