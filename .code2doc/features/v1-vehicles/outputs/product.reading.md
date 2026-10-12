# Vehicles — Product Requirements

> Generated from code at `742b0e6d` · fact coverage 98.1% · 81 verified claims

## Summary
The vehicle management capability allows operators and integration clients to monitor, direct, and configure automated guided vehicles (AGVs) across a facility . Users can query live vehicle telemetry, assign communication adapters, adjust operational pause and integration states, manage allowed order types, update envelope shapes and energy thresholds, simulate candidate travel routes, and trigger dispatching or rerouting workflows . These controls ensure vehicles are safely coordinated and correctly utilized according to plant conditions .

## What users can do

### Monitor vehicle state and fleet inventory
Users can inspect fleet status across all automated guided vehicles or inspect a specific vehicle's state, physical coordinates, orientation, bounding box, energy thresholds, resource reservations, envelope key, acceptable order types, and assigned transport orders . Fleet listings are sorted in ascending alphabetical order by vehicle name and can optionally be filtered by vehicle processing state; omitting the filter returns all vehicles . If an unrecognized processing state filter is supplied or an unknown vehicle is queried, the system rejects the request .

### Compute candidate routes
Users can compute potential navigation routes for a vehicle from a source point to one or more destination points, specifying resources to avoid . If no source point is provided, the vehicle's current position is used as the default starting location . If the vehicle's position is unknown and no source point is supplied, or if the route calculation limit is invalid, the system rejects the request . If the vehicle, the source point, any destination point, or any resource to avoid does not exist in the plant model, the system rejects the request .

### Manage communication adapters
Users can view adapter attachment details, switch the vehicle's communication adapter driver, enable or disable the adapter, and forward custom messages containing key-value parameters to the driver . Attaching an adapter updates the preferred adapter property on the vehicle . The system rejects adapter commands if the vehicle does not exist, if the specified driver is not among the available adapters for the vehicle, or if adapter state changes are attempted without an existing vehicle entry .

### Control vehicle operational and dispatching lifecycle
Users can trigger an immediate full dispatching run across the kernel, pause or resume a vehicle, adjust its integration level, and request withdrawal of any currently assigned transport order . When withdrawing an assigned order, users can elect to disable the vehicle, which sets the vehicle's integration level to be respected . Setting an integration level that ignores or merely notices the vehicle is rejected while the vehicle is actively processing a transport order . Requests are rejected if the targeted vehicle does not exist .

### Reroute a vehicle
Users can trigger route recalculation for a vehicle's active transport order, choosing either regular or forced rerouting . If forced rerouting is initiated but the vehicle's current position is unknown or resources for its current position cannot be allocated, the system rejects the request . Requests are rejected if the targeted vehicle does not exist .

### Configure vehicle order profiles, envelopes, and energy thresholds
Users can update the order types a vehicle is permitted to execute—either providing order type names with default priority zero or specifying explicit execution priorities—and modify the vehicle's envelope key or energy level thresholds . Energy threshold updates require critical, good, sufficiently recharged, and fully recharged percentages, each between 0 and 100 inclusive . Updating the envelope key is rejected if the vehicle is currently processing an order, allocating resources, or claiming resources . All configuration requests are rejected if the vehicle does not exist .

## States and lifecycle
A vehicle maintains operational, processing, and integration states :
- **Operational State**: Describes hardware and connection readiness as `UNKNOWN` (communication lost), `UNAVAILABLE` (not in error but unavailable for orders), `ERROR` (failure condition), `IDLE` (ready for movement orders), `EXECUTING` (processing movement orders), or `CHARGING` (recharging or refueling) .
- **Processing State**: Describes order execution activity as `IDLE` (no order assigned), `AWAITING_ORDER` (assigned to a transport order but awaiting the next drive order), or `PROCESSING_ORDER` (actively executing a drive order) .
- **Integration Level**: Governs dispatcher utilization as `TO_BE_IGNORED` (completely ignored by traffic management), `TO_BE_NOTICED` (visible but no resources reserved), `TO_BE_RESPECTED` (resources reserved for reported positions but not assigned orders), or `TO_BE_UTILIZED` (fully available for order assignments by the dispatcher) .

## Business rules
- When updating acceptable order types using only a list of order type names, each specified order type is assigned an execution priority of zero .
- Bounding box definitions require length, width, height dimensions, and two-dimensional reference offsets .
- Route step orientations are evaluated as forward, backward, or undefined .
- Route candidate calculations limit results per destination point to one by default if not specified .
- Rerouting requests default to regular rerouting unless forced rerouting is explicitly requested .
- When a drive order is rerouted without prior executed steps, route continuity is preserved; intermediate steps receive no-operation commands while the destination step retains the target operation .
- Automatically generated recharge orders created during dispatch runs are assigned the order type `Charge` .
- Drive orders are not assigned if redundant order assignment is disabled, the destination point matches the vehicle's position, and the operation is a movement or no-operation .
- Block resources restrict simultaneous traffic either to a single vehicle at a time or to multiple vehicles traveling in the same direction .
- When rerouting is impossible, dispatchers are configured to ignore locked paths, pause immediately at the source point, or pause upon reaching a locked path .

## Glossary
- **Acceptable Order Type**: A transport order classification that an automated guided vehicle is allowed to execute, accompanied by an execution priority .
- **Bounding Box**: Physical dimensions (length, width, height) and reference offsets defining the vehicle's physical footprint .
- **Communication Adapter**: Driver software component interfacing between openTCS and the physical vehicle hardware .
- **Envelope Key**: An identifier referencing the geometric boundary envelope associated with a vehicle .
- **Integration Level**: Operational setting determining whether a vehicle is ignored, noticed, respected, or utilized by the dispatcher .
- **Processing State**: Indicator of whether a vehicle is idle, waiting for a subsequent drive order, or actively executing a drive order .

## Requirements
- VEH-A1: The system shall return all vehicles and their details sorted ascending by name .
- VEH-A2: The system shall filter vehicle listings by vehicle processing state when requested .
- VEH-A3: The system shall return all vehicles regardless of processing state when no filter is requested .
- VEH-A4: The system shall reject vehicle listing requests that supply an invalid processing state .
- VEH-A5: The system shall return state and telemetry details for a specified vehicle .
- VEH-A6: The system shall reject requests for vehicle details if the vehicle does not exist .
- VEH-B1: The system shall compute navigation routes avoiding specified resources from a source point to destinations for a vehicle .
- VEH-B2: The system shall default the source point to the vehicle's current position when omitted .
- VEH-B3: The system shall default the maximum routes per destination point to one when omitted .
- VEH-B4: The system shall reject route calculation requests if the vehicle does not exist .
- VEH-B5: The system shall reject route calculation requests if the source point is omitted and the vehicle has no known position .
- VEH-B6: The system shall reject route calculation requests if the source point, any destination point, or any resource to avoid does not exist .
- VEH-B7: The system shall reject route calculation requests if the route limit parameter is not an integer .
- VEH-C1: The system shall return communication adapter attachment details for a vehicle .
- VEH-C2: The system shall reject attachment detail queries if the vehicle does not exist .
- VEH-C3: The system shall attach a specified communication adapter driver to a vehicle and record it as the preferred adapter .
- VEH-C4: The system shall reject adapter attachment requests if the vehicle does not exist or the adapter is not among the available adapters .
- VEH-C5: The system shall enable or disable a vehicle's communication adapter .
- VEH-C6: The system shall reject adapter enable or disable requests if the vehicle does not exist or has no entry .
- VEH-C7: The system shall deliver custom messages with key-value parameters to a vehicle's communication adapter .
- VEH-C8: The system shall reject adapter messages if the vehicle does not exist .
- VEH-D1: The system shall trigger a full fleet dispatching run .
- VEH-D2: The system shall update the paused state of a vehicle .
- VEH-D3: The system shall reject pause updates if the vehicle does not exist .
- VEH-D4: The system shall update a vehicle's integration level .
- VEH-D5: The system shall reject integration level updates to ignored or noticed while the vehicle is processing an order .
- VEH-D6: The system shall reject integration level updates if the vehicle does not exist .
- VEH-D7: The system shall withdraw transport orders assigned to a vehicle .
- VEH-D8: The system shall set a vehicle's integration level to respected upon withdrawal when requested .
- VEH-D9: The system shall reject order withdrawal requests if the vehicle does not exist .
- VEH-E1: The system shall request regular or forced rerouting for a vehicle .
- VEH-E2: The system shall reject rerouting requests if the vehicle does not exist .
- VEH-E3: The system shall reject forced rerouting requests if the vehicle position is unknown or resources for its position cannot be allocated .
- VEH-F1: The system shall update acceptable order types with default priority zero .
- VEH-F2: The system shall reject acceptable order type updates if the vehicle does not exist .
- VEH-F3: The system shall update acceptable order types with explicit priorities .
- VEH-F4: The system shall reject acceptable order type priority updates if the vehicle does not exist .
- VEH-F5: The system shall update vehicle energy level threshold sets .
- VEH-F6: The system shall reject energy threshold updates if any value is outside 0 to 100 or the vehicle does not exist .
- VEH-F7: The system shall update a vehicle's envelope key .
- VEH-F8: The system shall reject envelope key updates if the vehicle is processing an order, allocating resources, or claiming resources .
- VEH-F9: The system shall reject envelope key updates if the vehicle does not exist .

## Open items
- Published docs state that vehicle processing state includes UNAVAILABLE alongside IDLE, AWAITING_ORDER, and PROCESSING_ORDER, whereas the processing state model defines IDLE, AWAITING_ORDER, and PROCESSING_ORDER .
- Published docs state that the vehicle property for preferred adapter is `tcs:preferredAdapterClass`, whereas the system sets property `tcs:preferredAdapter` .
- Published docs state that peripheral operation execution triggers are limited to AFTER_ALLOCATION and AFTER_MOVEMENT, whereas IMMEDIATE is also supported .
- Published docs specify adapter message parameters as a JSON array, whereas the implementation accepts key-value parameters .

## Appendix: interface reference

| Operation | Request | Inputs | Success Result | Failure Cases | Claims |
|---|---|---|---|---|---|
| Trigger dispatcher | POST /v1/vehicles/dispatcher/trigger | None | HTTP 200 (empty text response) | HTTP 500 on kernel runtime error or interrupted execution |  |
| List vehicles | GET /v1/vehicles | Query: `procState` (optional vehicle processing state string) | HTTP 200 with JSON list of vehicle objects sorted by name | HTTP 400 if `procState` is invalid; HTTP 500 on kernel error |  |
| Get vehicle | GET /v1/vehicles/{NAME} | Path: `NAME` | HTTP 200 with vehicle details JSON | HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Compute vehicle routes | POST /v1/vehicles/{NAME}/routes | Path: `NAME`; Query: `maxRoutesPerDestinationPoint` (optional integer); Body: JSON with `destinationPoints` (required list), `sourcePoint` (optional string), `resourcesToAvoid` (optional list) | HTTP 200 with computed route steps JSON | HTTP 400 on malformed JSON, invalid query int, or missing source point when vehicle position unknown; HTTP 404 if vehicle, source point, destination point, or avoid resource not found; HTTP 500 on routing or serialization failure |  |
| Get comm adapter attachment info | GET /v1/vehicles/{NAME}/commAdapter/attachmentInformation | Path: `NAME` | HTTP 200 with attachment info JSON | HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Attach comm adapter | PUT /v1/vehicles/{NAME}/commAdapter/attachmentInformation | Path: `NAME`; Query: `newValue` (adapter class name) | HTTP 200 (empty text response) | HTTP 400 if adapter not available; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Enable/disable comm adapter | PUT /v1/vehicles/{NAME}/commAdapter/enabled | Path: `NAME`; Query: `newValue` (boolean) | HTTP 200 (empty text response) | HTTP 400 if vehicle entry not found; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Send comm adapter message | POST /v1/vehicles/{NAME}/commAdapter/message | Path: `NAME`; Body: JSON message type and key-value parameters | HTTP 200 (empty text response) | HTTP 400 on malformed body; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Set paused state | PUT /v1/vehicles/{NAME}/paused | Path: `NAME`; Query: `newValue` (boolean) | HTTP 200 (empty text response) | HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Set integration level | PUT /v1/vehicles/{NAME}/integrationLevel | Path: `NAME`; Query: `newValue` (integration level string) | HTTP 200 (empty text response) | HTTP 400 if set to TO_BE_IGNORED/TO_BE_NOTICED while processing order; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Withdraw transport order | POST /v1/vehicles/{NAME}/withdrawal | Path: `NAME`; Query: `disableVehicle` (optional boolean) | HTTP 200 (empty text response) | HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Request reroute | POST /v1/vehicles/{NAME}/rerouteRequest | Path: `NAME`; Query: `forced` (optional boolean) | HTTP 200 (empty text response) | HTTP 400 if forced and position unknown or resources unallocatable; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Set acceptable order types (default priority) | PUT /v1/vehicles/{NAME}/acceptableOrderTypes | Path: `NAME`; Body: JSON array of order type strings | HTTP 200 (empty text response) | HTTP 400 on malformed body; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Set acceptable order types with priorities | PUT /v1/vehicles/{NAME}/acceptableOrderTypes | Path: `NAME`; Body: JSON list of order types with name and priority | HTTP 200 (empty text response) | HTTP 400 on malformed body; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Set energy level thresholds | PUT /v1/vehicles/{NAME}/energyLevelThresholdSet | Path: `NAME`; Body: JSON threshold set (critical, good, sufficientlyRecharged, fullyRecharged) | HTTP 200 (empty text response) | HTTP 400 if thresholds not 0-100; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |
| Set envelope key | PUT /v1/vehicles/{NAME}/envelopeKey | Path: `NAME`; Query: `newValue` (envelope key string) | HTTP 200 (empty text response) | HTTP 400 if processing order, claiming, or allocating resources; HTTP 404 if vehicle unknown; HTTP 500 on kernel error |  |


## Traceability

| Requirement / section | Claims | Source |
|---|---|---|
| Summary | C-0012, C-0028, C-0030, C-0032, C-0033, C-0034, C-0100, C-0700, C-0800, C-1000 | GetVehicleAttachmentInfoResponseTO.java:10-23; GetVehicleResponseTO.java:23-145; PostVehicleRoutesRequestTO.java:13-26; PutVehicleAcceptableOrderTypesTO.java:12-25; PutVehicleEnergyLevelThresholdSetTO.java:9-23; V1RequestHandler.java:169-169; V1RequestHandler.java:181-181; V1RequestHandler.java:182-182; V1RequestHandler.java:184-184; Vehicle.java:1396-1415 |
| Monitor vehicle state and fleet inventory | C-0034, C-1500, C-1501, C-1600, C-1601, C-1602, C-1603 | GetVehicleResponseTO.java:23-145; V1RequestHandler.java:195-195; V1RequestHandler.java:196-196; V1RequestHandler.java:609-609; VehicleHandler.java:112-112; VehicleHandler.java:86-88 |
| Compute candidate routes | C-0033, C-0200, C-0201, C-0204, C-0205, C-0206, C-0207, C-0208, C-0209, C-0210 | PostVehicleRoutesRequestTO.java:13-26; V1RequestHandler.java:170-170; V1RequestHandler.java:326-345; V1RequestHandler.java:784-789; VehicleHandler.java:295-296; VehicleHandler.java:299-302; VehicleHandler.java:299-304; VehicleHandler.java:306-311; VehicleHandler.java:315-322; VehicleHandler.java:328-349 |
| Manage communication adapters | C-0028, C-0029, C-0300, C-0301, C-0302, C-0303, C-0400, C-0402, C-0500, C-0501, C-0502, C-0503, C-0600, C-0601 | AttachmentManager.java:206-210; GetVehicleAttachmentInfoResponseTO.java:10-23; PostVehicleCommAdapterMessageRequestTO.java:13-26; StandardVehicleService.java:273-275; StandardVehicleService.java:288-290; V1RequestHandler.java:171-174; V1RequestHandler.java:175-178; V1RequestHandler.java:179-179; V1RequestHandler.java:180-180; VehicleHandler.java:167-168; VehicleHandler.java:184-185; VehicleHandler.java:197-198; VehicleHandler.java:204-208; VehicleHandler.java:222-223 |
| Control vehicle operational and dispatching lifecycle | C-0100, C-0700, C-0701, C-0800, C-0801, C-0802, C-0900, C-0902, C-0903 | StandardVehicleService.java:350-360; TransportOrderDispatcherHandler.java:83-84; V1RequestHandler.java:169-169; V1RequestHandler.java:181-181; V1RequestHandler.java:182-182; V1RequestHandler.java:183-183; V1RequestHandler.java:441-441; VehicleHandler.java:123-124; VehicleHandler.java:140-141 |
| Reroute a vehicle | C-0019, C-1000, C-1001, C-1002, C-1003, C-1004, C-1005 | ReroutingType.java:10-21; ServiceWebApi.java:172-193; ServiceWebApi.java:175-178; TransportOrderDispatcherHandler.java:102-103; V1RequestHandler.java:184-184; V1RequestHandler.java:835-837 |
| Configure vehicle order profiles, envelopes, and energy thresholds | C-0030, C-0031, C-0032, C-1100, C-1101, C-1200, C-1201, C-1300, C-1301, C-1302, C-1400, C-1401, C-1402 | PutVehicleAcceptableOrderTypesTO.java:12-25; PutVehicleAllowedOrderTypesTO.java:11-24; PutVehicleEnergyLevelThresholdSetTO.java:9-23; StandardVehicleService.java:417-424; V1RequestHandler.java:185-185; V1RequestHandler.java:186-189; V1RequestHandler.java:190-193; V1RequestHandler.java:194-194; Vehicle.java:1537-1560; VehicleHandler.java:153-154; VehicleHandler.java:244-245; VehicleHandler.java:270-271 |
| States and lifecycle | C-0012, C-0013, C-0014 | Vehicle.java:1363-1391; Vehicle.java:1396-1415; Vehicle.java:1420-1435 |
| Business rules | C-0006, C-0007, C-0015, C-0026, C-0102, C-0103, C-0202, C-1001, C-1006, C-1007, C-1100 | Block.java:204-215; BoundingBoxTO.java:10-25; DefaultDispatcherConfiguration.java:196-210; DefaultVehicleController.java:502-505; MovementCommandMapper.java:68-70; RechargeIdleVehiclesPhase.java:150-156; TransportOrderUtil.java:258-262; V1RequestHandler.java:185-185; V1RequestHandler.java:782-783; V1RequestHandler.java:835-837; Vehicle.java:1441-1457 |
| Glossary | C-0012, C-0014, C-0025, C-0026, C-0028, C-0034, C-0300, C-1400 | AcceptableOrderTypeTO.java:10-23; BoundingBoxTO.java:10-25; GetVehicleAttachmentInfoResponseTO.java:10-23; GetVehicleResponseTO.java:23-145; V1RequestHandler.java:171-174; V1RequestHandler.java:194-194; Vehicle.java:1396-1415; Vehicle.java:1420-1435 |
| VEH-A1 | C-1600 | V1RequestHandler.java:196-196 |
| VEH-A2 | C-1601 | V1RequestHandler.java:609-609 |
| VEH-A3 | C-1602 | VehicleHandler.java:86-88 |
| VEH-A4 | C-1603 | VehicleHandler.java:86-88 |
| VEH-A5 | C-0034, C-1500 | GetVehicleResponseTO.java:23-145; V1RequestHandler.java:195-195 |
| VEH-A6 | C-1501 | VehicleHandler.java:112-112 |
| VEH-B1 | C-0033, C-0200, C-0201 | PostVehicleRoutesRequestTO.java:13-26; V1RequestHandler.java:170-170; V1RequestHandler.java:326-345 |
| VEH-B2 | C-0206 | VehicleHandler.java:299-304 |
| VEH-B3 | C-0202 | V1RequestHandler.java:782-783 |
| VEH-B4 | C-0205 | VehicleHandler.java:295-296 |
| VEH-B5 | C-0207 | VehicleHandler.java:299-302 |
| VEH-B6 | C-0208, C-0209, C-0210 | VehicleHandler.java:306-311; VehicleHandler.java:315-322; VehicleHandler.java:328-349 |
| VEH-B7 | C-0204 | V1RequestHandler.java:784-789 |
| VEH-C1 | C-0028, C-0400, C-0401 | GetVehicleAttachmentInfoResponseTO.java:10-23; V1RequestHandler.java:175-178; V1RequestHandler.java:310-310 |
| VEH-C2 | C-0402 | VehicleHandler.java:184-185 |
| VEH-C3 | C-0300, C-0303 | AttachmentManager.java:206-210; V1RequestHandler.java:171-174 |
| VEH-C4 | C-0301, C-0302 | VehicleHandler.java:197-198; VehicleHandler.java:204-208 |
| VEH-C5 | C-0500 | V1RequestHandler.java:179-179 |
| VEH-C6 | C-0501, C-0502, C-0503 | StandardVehicleService.java:273-275; StandardVehicleService.java:288-290; VehicleHandler.java:167-168 |
| VEH-C7 | C-0029, C-0600 | PostVehicleCommAdapterMessageRequestTO.java:13-26; V1RequestHandler.java:180-180 |
| VEH-C8 | C-0601 | VehicleHandler.java:222-223 |
| VEH-D1 | C-0100 | V1RequestHandler.java:169-169 |
| VEH-D2 | C-0700 | V1RequestHandler.java:181-181 |
| VEH-D3 | C-0701 | VehicleHandler.java:140-141 |
| VEH-D4 | C-0800 | V1RequestHandler.java:182-182 |
| VEH-D5 | C-0802 | StandardVehicleService.java:350-360 |
| VEH-D6 | C-0801 | VehicleHandler.java:123-124 |
| VEH-D7 | C-0900 | V1RequestHandler.java:183-183 |
| VEH-D8 | C-0902 | V1RequestHandler.java:441-441 |
| VEH-D9 | C-0903 | TransportOrderDispatcherHandler.java:83-84 |
| VEH-E1 | C-1000, C-1001 | V1RequestHandler.java:184-184; V1RequestHandler.java:835-837 |
| VEH-E2 | C-1002 | TransportOrderDispatcherHandler.java:102-103 |
| VEH-E3 | C-1003, C-1004, C-1005 | ServiceWebApi.java:172-193; ServiceWebApi.java:175-178 |
| VEH-F1 | C-1100 | V1RequestHandler.java:185-185 |
| VEH-F2 | C-1101 | VehicleHandler.java:244-245 |
| VEH-F3 | C-0030, C-1200 | PutVehicleAcceptableOrderTypesTO.java:12-25; V1RequestHandler.java:186-189 |
| VEH-F4 | C-1201 | VehicleHandler.java:244-245 |
| VEH-F5 | C-0032, C-1300 | PutVehicleEnergyLevelThresholdSetTO.java:9-23; V1RequestHandler.java:190-193 |
| VEH-F6 | C-1301, C-1302 | Vehicle.java:1537-1560; VehicleHandler.java:270-271 |
| VEH-F7 | C-1400 | V1RequestHandler.java:194-194 |
| VEH-F8 | C-1402 | StandardVehicleService.java:417-424 |
| VEH-F9 | C-1401 | VehicleHandler.java:153-154 |
| Appendix: interface reference | C-0001, C-0005, C-0025, C-0028, C-0029, C-0030, C-0031, C-0032, C-0034, C-0100, C-0106, C-0107, C-0200, C-0201, C-0202, C-0203, C-0204, C-0205, C-0206, C-0207, C-0208, C-0209, C-0210, C-0211, C-0212, C-0300, C-0301, C-0302, C-0303, C-0400, C-0401, C-0402, C-0500, C-0501, C-0502, C-0503, C-0600, C-0601, C-0700, C-0701, C-0800, C-0801, C-0802, C-0900, C-0902, C-0903, C-1000, C-1001, C-1002, C-1003, C-1004, C-1005, C-1100, C-1101, C-1200, C-1201, C-1300, C-1301, C-1302, C-1400, C-1401, C-1402, C-1500, C-1501, C-1600, C-1601, C-1602, C-1603 | AcceptableOrderTypeTO.java:10-23; AttachmentManager.java:206-210; GetVehicleAttachmentInfoResponseTO.java:10-23; GetVehicleResponseTO.java:23-145; JsonBinder.java:46-48; JsonBinder.java:66-68; KernelExecutorWrapper.java:55-57; KernelExecutorWrapper.java:58-63; PostVehicleCommAdapterMessageRequestTO.java:13-26; PutVehicleAcceptableOrderTypesTO.java:12-25; PutVehicleAllowedOrderTypesTO.java:11-24; PutVehicleEnergyLevelThresholdSetTO.java:9-23; ServiceWebApi.java:172-193; ServiceWebApi.java:175-178; ServiceWebApi.java:197-200; ShortestPathPointRouter.java:119-124; StandardVehicleService.java:273-275; StandardVehicleService.java:288-290; StandardVehicleService.java:350-360; StandardVehicleService.java:417-424; TransportOrderDispatcherHandler.java:102-103; TransportOrderDispatcherHandler.java:83-84; V1RequestHandler.java:169-169; V1RequestHandler.java:170-170; V1RequestHandler.java:171-174; V1RequestHandler.java:175-178; V1RequestHandler.java:179-179; V1RequestHandler.java:180-180; V1RequestHandler.java:181-181; V1RequestHandler.java:182-182; V1RequestHandler.java:183-183; V1RequestHandler.java:184-184; V1RequestHandler.java:185-185; V1RequestHandler.java:186-189; V1RequestHandler.java:190-193; V1RequestHandler.java:194-194; V1RequestHandler.java:195-195; V1RequestHandler.java:196-196; V1RequestHandler.java:310-310; V1RequestHandler.java:326-345; V1RequestHandler.java:441-441; V1RequestHandler.java:609-609; V1RequestHandler.java:782-783; V1RequestHandler.java:784-789; V1RequestHandler.java:835-837; Vehicle.java:1537-1560; VehicleHandler.java:112-112; VehicleHandler.java:123-124; VehicleHandler.java:140-141; VehicleHandler.java:153-154; VehicleHandler.java:167-168; VehicleHandler.java:184-185; VehicleHandler.java:197-198; VehicleHandler.java:204-208; VehicleHandler.java:222-223; VehicleHandler.java:244-245; VehicleHandler.java:270-271; VehicleHandler.java:295-296; VehicleHandler.java:299-302; VehicleHandler.java:299-304; VehicleHandler.java:306-311; VehicleHandler.java:315-322; VehicleHandler.java:328-349; VehicleHandler.java:86-88 |
