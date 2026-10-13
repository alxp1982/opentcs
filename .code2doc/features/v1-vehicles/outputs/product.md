# Vehicles — Product Requirements

## Summary
The Vehicles capability allows users and external systems to monitor vehicle status, compute paths, control communication adapters, adjust operational parameters, and manage active vehicle assignments. Users can inspect vehicle poses, energy levels, and assigned transport orders, trigger route calculations avoiding designated resources, and pause or resume vehicle operations. In addition, callers can attach and enable vehicle communication adapters, send custom adapter messages, adjust integration levels and envelope keys, update acceptable order types and energy thresholds, reroute in-transit vehicles, or withdraw active assignments. [C-0038, C-0044, C-0045, C-0100, C-0200, C-0300, C-0400, C-0500, C-0600, C-0700, C-0800, C-0900, C-1000, C-1100, C-1200, C-1300, C-1400, C-1500, C-1600]

## What users can do

### Retrieve vehicle information
Users can query all vehicles in the system sorted by name in ascending order, optionally filtering the results by vehicle processing state. [C-1600, C-1601] Users can also retrieve detailed status and attributes for a specific vehicle by name, including positions, orientations, energy thresholds, resource claims, and order assignments. [C-0038, C-1500] If a requested vehicle does not exist, or if an invalid vehicle processing state is provided as a filter, the request is rejected. [C-1501, C-1602]

### Calculate routes
Users can compute vehicle routes from a starting point to one or more destination points, optionally identifying plant model resources to avoid. [C-0045, C-0200] When a starting point is omitted, the system defaults to using the vehicle's current position; the number of computed routes per destination point defaults to one when not specified. [C-0201, C-0206] The system rejects calculation requests if the vehicle does not exist, if the source point is omitted and the vehicle has no known position, or if any referenced source, destination, or avoided resource cannot be found. [C-0205, C-0207, C-0208, C-0209, C-0210]

### Manage communication adapters
Users can inspect adapter attachment details, attach a new communication adapter driver, enable or disable the adapter, and transmit custom messages to the driver. [C-0300, C-0400, C-0500, C-0600] When attaching an adapter, the system terminates and disables any existing adapter before initializing the new one, and records the factory property on the vehicle. [C-0303, C-0304] Requests fail if the vehicle does not exist, if the specified adapter description does not match an available adapter, or if no internal vehicle entry is found. [C-0301, C-0302, C-0401, C-0502, C-0503, C-0504, C-0603]

### Control vehicle movement and integration
Users can pause or resume a vehicle, change its integration level, or withdraw assigned transport orders. [C-0700, C-0800, C-0900] When withdrawing an order, cancellation defaults to non-immediate unless specified, and setting vehicle disabling updates the integration level to be respected by traffic management. [C-0901, C-0902] The system rejects modifications if the vehicle does not exist, or if a user attempts to ignore or merely notice a vehicle while it is actively processing an order. [C-0701, C-0801, C-0802, C-0903]

### Reroute vehicles
Users can request that a vehicle be rerouted along its route. [C-1000] Rerouting defaults to regular rerouting respecting existing allocations, but callers can request forced rerouting from the vehicle's current position regardless of allocations. [C-0021, C-1001] The system rejects reroute requests if the vehicle does not exist, or if a forced rerouting is requested when the vehicle's position is unknown or immediate resource allocation fails. [C-1002, C-1003, C-1004, C-1005]

### Configure vehicle operational profiles
Users can set envelope keys, acceptable order types, and energy level threshold sets. [C-1100, C-1200, C-1300, C-1400] Updating allowed order types configures each specified order type with a priority of zero, whereas updating acceptable order types accepts custom priorities. [C-0033, C-1100, C-1200] Requests are rejected if the vehicle does not exist, if energy percentage thresholds are not between 0 and 100 inclusive, or if the envelope key is modified while the vehicle is processing an order or claiming/allocating resources. [C-1101, C-1201, C-1301, C-1302, C-1401, C-1402]

### Trigger dispatching
Users can explicitly trigger a dispatcher execution run to evaluate and assign transport orders across the fleet. [C-0100]

## States and lifecycle
A vehicle processing state indicates whether a vehicle is IDLE, AWAITING_ORDER, or PROCESSING_ORDER. [C-0029] An integration level determines whether a vehicle is ignored (TO_BE_IGNORED), displayed without impacting traffic (TO_BE_NOTICED), or respected by traffic management (TO_BE_RESPECTED). [C-0009]

## Business rules
- Angle properties support degrees and radians, length properties support millimeters, centimeters, meters, and kilometers, speed properties support kilometers per hour, meters per second, and millimeters per second, and percent properties support percent. [C-0001, C-0002, C-0003, C-0004]
- Driving course blocks restrict resource usage either to a single vehicle at a time or to multiple vehicles traveling in the same direction. [C-0005]
- Vehicle driving orientation along a route is classified as forward, backward, or undefined. [C-0025]
- Route step calculations define source point, destination point, connecting path, and vehicle orientation. [C-0045]
- Energy thresholds configure critical, good, sufficiently recharged, and fully recharged percentage values, each constrained between 0 and 100 inclusive. [C-0049, C-1302]
- A vehicle bounding box defines length, width, height, and a 2D reference offset. [C-0034]
- Updating vehicle pause state or integration level emits model modification events. [C-0702, C-0803]

## Glossary
- **Acceptable Order Type**: A transport order type name coupled with a dispatching priority accepted by a vehicle. [C-0033]
- **Bounding Box**: A three-dimensional vehicle footprint describing length, width, height, and reference offsets. [C-0034]
- **Communication Adapter**: A driver attached to a vehicle that translates communication between the control system and the physical vehicle. [C-0037, C-0300]
- **Envelope Key**: An identifier defining the vehicle envelope profile used for resource reservations. [C-0038, C-1400]
- **Integration Level**: The operational tier of an entity indicating if it is ignored, noticed, or respected by traffic management. [C-0009]
- **Processing State**: The operational dispatching status of a vehicle (IDLE, AWAITING_ORDER, or PROCESSING_ORDER). [C-0029]
- **Rerouting Type**: The strategy used when recomputing vehicle paths, either regular (preserving resource allocations) or forced (recomputing from current position disregarding allocations). [C-0021]

## Requirements
- VEH-A1: The system shall retrieve all vehicles sorted by vehicle name in ascending order. [C-1600]
- VEH-A2: The system shall filter vehicle list results by processing state when specified. [C-1601]
- VEH-A3: The system shall reject vehicle listing queries when an invalid processing state filter is provided. [C-1602]
- VEH-A4: The system shall retrieve full status details for a named vehicle. [C-0038, C-1500]
- VEH-A5: The system shall reject queries for vehicle status when the named vehicle does not exist. [C-1501]
- VEH-B1: The system shall compute routes from a source point to destination points while avoiding specified resources. [C-0045, C-0200]
- VEH-B2: The system shall default the source point to the vehicle's current position when not specified. [C-0206]
- VEH-B3: The system shall default the maximum routes per destination point to one when omitted. [C-0201]
- VEH-B4: The system shall reject route calculation if the vehicle does not exist. [C-0205]
- VEH-B5: The system shall reject route calculation if the source point is omitted and the vehicle has no known position. [C-0207]
- VEH-B6: The system shall reject route calculation if the source point, any destination point, or any avoided resource is not recognized. [C-0208, C-0209, C-0210]
- VEH-C1: The system shall retrieve communication adapter attachment information for a named vehicle. [C-0037, C-0400]
- VEH-C2: The system shall reject communication adapter information requests if the vehicle does not exist. [C-0401]
- VEH-C3: The system shall attach a specified communication adapter to a named vehicle, terminating and disabling any previous adapter. [C-0300, C-0303, C-0304]
- VEH-C4: The system shall reject adapter attachment if the vehicle does not exist or the adapter description does not match available adapters. [C-0301, C-0302]
- VEH-C5: The system shall enable or disable a vehicle's communication adapter. [C-0500, C-0501]
- VEH-C6: The system shall reject adapter enablement changes if the vehicle does not exist or has no entry. [C-0502, C-0503, C-0504]
- VEH-C7: The system shall transmit custom messages with parameters to a vehicle's communication adapter. [C-0044, C-0600, C-0601, C-0602]
- VEH-C8: The system shall reject custom messages if the vehicle does not exist. [C-0603]
- VEH-D1: The system shall set the paused state of a named vehicle and emit modification events. [C-0700, C-0702]
- VEH-D2: The system shall reject pause updates if the vehicle does not exist. [C-0701]
- VEH-D3: The system shall update a vehicle's integration level and emit modification events. [C-0800, C-0803]
- VEH-D4: The system shall reject integration level updates to ignored or noticed states while the vehicle is processing an order. [C-0802]
- VEH-D5: The system shall reject integration level updates if the vehicle does not exist. [C-0801]
- VEH-D6: The system shall withdraw a vehicle's active transport order, defaulting immediate withdrawal and vehicle disabling to false. [C-0900, C-0901, C-0902]
- VEH-D7: The system shall reject order withdrawal if the vehicle does not exist. [C-0903]
- VEH-E1: The system shall trigger rerouting for a named vehicle, defaulting to regular rerouting unless forced rerouting is requested. [C-0021, C-1000, C-1001]
- VEH-E2: The system shall reject rerouting if the vehicle does not exist. [C-1002]
- VEH-E3: The system shall reject forced rerouting if the vehicle's position is unknown or immediate resource allocation fails. [C-1003, C-1004, C-1005]
- VEH-F1: The system shall update vehicle allowed order types with priority zero. [C-0048, C-1100]
- VEH-F2: The system shall reject allowed order type updates if the vehicle does not exist. [C-1101]
- VEH-F3: The system shall update vehicle acceptable order types and their priorities. [C-0033, C-1200]
- VEH-F4: The system shall reject acceptable order type updates if the vehicle does not exist. [C-1201]
- VEH-F5: The system shall update vehicle energy level threshold sets when values are between 0 and 100 inclusive. [C-0049, C-1300, C-1302]
- VEH-F6: The system shall reject energy threshold updates if the vehicle does not exist or threshold values are out of bounds. [C-1301, C-1302]
- VEH-F7: The system shall update a vehicle's envelope key. [C-1400]
- VEH-F8: The system shall reject envelope key updates if the vehicle does not exist or is currently processing orders or claiming/allocating resources. [C-1401, C-1402]
- VEH-G1: The system shall trigger a dispatcher run and return success upon completion. [C-0100]

## Appendix: interface reference

| Operation | Request (method and path) | Inputs | Success result | Failure cases with status codes | Claims |
|---|---|---|---|---|---|
| Trigger dispatcher run | POST /v1/dispatcher | None | HTTP 200 (empty text/plain) | Execution interrupted (HTTP 500); execution exception (HTTP 500) | [C-0100, C-0101, C-0102] |
| Compute vehicle routes | POST /v1/vehicles/{NAME}/routes | Path: NAME; Query: maxRoutesPerDestinationPoint; Body: sourcePoint, destinationPoints, resourcesToAvoid | HTTP 200 (JSON routes) | Vehicle not found (HTTP 404); unparseable query int (HTTP 400); invalid JSON body (HTTP 400); missing source and vehicle position (HTTP 400); missing source/destination/resource (HTTP 404); routing computation error (HTTP 500); serialization failure (HTTP 500) | [C-0200, C-0201, C-0202, C-0203, C-0204, C-0205, C-0206, C-0207, C-0208, C-0209, C-0210, C-0211] |
| Attach communication adapter | PUT /v1/vehicles/{NAME}/commAdapter/attachment | Path: NAME; Query: newValue | HTTP 200 (empty text) | Vehicle not found (HTTP 404); adapter class not found/matched (HTTP 400) | [C-0300, C-0301, C-0302] |
| Get adapter attachment info | GET /v1/vehicles/{NAME}/commAdapter/attachmentInformation | Path: NAME | HTTP 200 (JSON attachment info) | Vehicle not found (HTTP 404) | [C-0400, C-0401] |
| Enable/disable adapter | PUT /v1/vehicles/{NAME}/commAdapter/enabled | Path: NAME; Query: newValue | HTTP 200 (empty text/plain) | Vehicle not found (HTTP 404); vehicle entry missing (HTTP 400) | [C-0500, C-0501, C-0502, C-0503, C-0504] |
| Send adapter message | POST /v1/vehicles/{NAME}/commAdapter/customMessage | Path: NAME; Body: type, parameters | HTTP 200 (empty text) | Vehicle not found (HTTP 404) | [C-0600, C-0601, C-0602, C-0603] |
| Set paused state | PUT /v1/vehicles/{NAME}/paused | Path: NAME; Query: newValue | HTTP 200 (empty text/plain) | Vehicle not found (HTTP 404) | [C-0700, C-0701] |
| Set integration level | PUT /v1/vehicles/{NAME}/integrationLevel | Path: NAME; Query: newValue | HTTP 200 (empty text/plain) | Vehicle not found (HTTP 404); vehicle is processing order (HTTP 400) | [C-0800, C-0801, C-0802] |
| Withdraw transport order | PUT /v1/vehicles/{NAME}/withdrawal | Path: NAME; Query: immediate, disableVehicle | HTTP 200 (empty text) | Vehicle not found (HTTP 404) | [C-0900, C-0901, C-0902, C-0903] |
| Request reroute | PUT /v1/vehicles/{NAME}/reroute | Path: NAME; Query: forced | HTTP 200 (empty text/plain) | Vehicle not found (HTTP 404); position unknown for forced reroute (HTTP 400); resource allocation error for forced reroute (HTTP 400) | [C-1000, C-1001, C-1002, C-1003, C-1004, C-1005] |
| Update allowed order types | PUT /v1/vehicles/{NAME}/allowedOrderTypes | Path: NAME; Body: list of order type names | HTTP 200 (empty text/plain) | Vehicle not found (HTTP 404) | [C-0048, C-1100, C-1101] |
| Update acceptable order types | PUT /v1/vehicles/{NAME}/acceptableOrderTypes | Path: NAME; Body: list of acceptable order types | HTTP 200 (empty text) | Vehicle not found (HTTP 404) | [C-0033, C-1200, C-1201] |
| Update energy thresholds | PUT /v1/vehicles/{NAME}/energyLevelThresholdSet | Path: NAME; Body: energyLevelCritical, energyLevelGood, energyLevelSufficientlyRecharged, energyLevelFullyRecharged | HTTP 200 (empty text) | Vehicle not found (HTTP 404); threshold value not between 0 and 100 (HTTP 400) | [C-0049, C-1300, C-1301, C-1302] |
| Update envelope key | PUT /v1/vehicles/{NAME}/envelopeKey | Path: NAME; Query: newValue | HTTP 200 (empty text) | Vehicle not found (HTTP 404); vehicle processing order or claiming/allocating resources (HTTP 400) | [C-1400, C-1401, C-1402] |
| Get vehicle by name | GET /v1/vehicles/{NAME} | Path: NAME | HTTP 200 (JSON vehicle details) | Vehicle not found (HTTP 404) | [C-0038, C-1500, C-1501] |
| List vehicles | GET /v1/vehicles | Query: procState | HTTP 200 (JSON vehicle list) | Invalid procState value (HTTP 400) | [C-1600, C-1601, C-1602] |
