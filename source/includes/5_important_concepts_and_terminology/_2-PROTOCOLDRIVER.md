## Protocol Driver

Two similar devices may have the same functionality but utilize a very different command set.  A protocol driver provides the device-specific information needed to communicate with the Control4 system.  In the case of DriverWorks, the DriverWorks driver is the protocol driver.  When combined with the device-specific.c4a file it provides the custom code necessary to implement the 2-way device driver.