## What’s New in 3.2.0

### Driver Conditionals

In additionn to updated content examples and explanations, the Conditionals section includes information on two new Conditional Types: Room and Device. The ability to generate Dynamic Conditionals s also included in O.S. 3.2.0.


### Zigbee

Change to Zigbee Server-Side Cluster Management 
Starting with the release of Operating System 3.2.0, Control4 will change the way Zigbee Clusters are handled. Drivers that have previously relied on the ability of clusters to circumvent zserver and its processes will be impacted by this change and will not function as intended in OS 3.2.0 and going forward.

For example, consider the following sample Profile:

`(HA Profile (0x0104)`
   ` Cluster A: Time (0x000A)`
   ` Cluster B: Basic (0x0000)`
`Cluster C: OTA Upgrade (0x0019)`

Pre-OS 3.2.0, these clusters and others may have been partially handled by Control4's zserver. With the release of OS 3.2.0, Time, Basic and OTA clusters will be advertised as supported by Control4, via a MatchDescriptorRequest, on the HA Profile (0x0104) and will be entirely handled within zserver.

Note that there are two scenarios where zserver will pass a cluster un-handled directly to a driver. These are:

- If the cluster that has the manufacturer-specific flag set, the cluster will be passed through to the driver, un-handled by zserver.

- All clusters which are not supported by Control4, on the HA profile, will be passed through to the driver, un-handled by zserver.

To avoid unintended functionality, a review of your Zigbee driver(s) is recommended prior to the release of OS 3.2.0. The ability to send Zigbee Clusters directly to your driver will need to be handled at the device firmware level.

Operating System 3.2.0 will include an OTA Upgrade API which will provide a driver based 