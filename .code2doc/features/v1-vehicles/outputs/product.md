# Vehicles — Product Requirements

## Summary
The vehicle management capability allows operators and integration clients to monitor, direct, and configure automated guided vehicles (AGVs) across a facility [C-0034, C-0100]. Users can query live vehicle telemetry, assign communication adapters, adjust operational pause and integration states, manage allowed order types, update envelope shapes and energy thresholds, simulate candidate travel routes, and trigger dispatching or rerouting workflows [C-0028, C-0030, C-0032, C-0033, C-0100, C-0700, C-0800, C-1000]. These controls ensure vehicles are safely coordinated and correctly utilized according to plant conditions [C-0012, C-0100].

## What users can do

### Monitor vehicle state and fleet inventory
Users can inspect fleet status across all automated guided vehicles or inspect a specific vehicle's state, physical coordinates, orientation, bounding box, energy thresholds, resource reservations, envelope key, acceptable order types, and assigned transport orders [C-0034, C-1500, C-1600]. Fleet listings are sorted in ascending alphabetical order by vehicle name and can optionally be filtered by vehicle processing state; omitting the filter returns all vehicles [C-1600, C-1601, C-1602]. If an unrecognized processing state filter is supplied or an unknown vehicle is queried, the system rejects the request [C-1501, C-1603].

### Compute candidate routes
Users can compute potential navigation routes for a vehicle from a source point to one or more destination points, specifying resources to avoid [C-0033, C-0200, C-0201]. If no source point is provided, the vehicle's current position is used as the default starting location [C-0206]. If the vehicle's position is unknown and no source point is supplied, or if the route calculation limit is invalid, the system rejects the request [C-0204, C-0207]. If the vehicle, the source point, any destination point, or any resource to avoid does not exist in the plant model, the system rejects the request [C-0205, C-0208, C-0209, C-0210].

### Manage communication adapters
Users can view adapter attachment details, switch the vehicle's communication adapter driver, enable or disable the adapter, and forward custom messages containing key-value parameters to the driver [C-0028, C-0029, C-0300, C-0400, C-0500, C-0600]. Attaching an adapter updates the preferred adapter property on the vehicle [C-0303]. The system rejects adapter commands if the vehicle does not exist, if the specified driver is not among the available adapters for the vehicle, or if adapter state changes are attempted without an existing vehicle entry [C-0301, C-0302, C-0402, C-0501, C-0502, C-0503, C-0601].

### Control vehicle operational and dispatching lifecycle
Users can trigger an immediate full dispatching run across the kernel, pause or resume a vehicle, adjust its integration level, and request withdrawal of any currently assigned transport order [C-0100, C-0700, C-0800, C-0900]. When withdrawing an assigned order, users can elect to disable the vehicle, which sets the vehicle's integration level to be respected [C-0902]. Setting an integration level that ignores or merely notices the vehicle is rejected while the vehicle is actively processing a transport order [C-0802]. Requests are rejected if the targeted vehicle does not exist [C-0701, C-0801, C-0903].

### Reroute a vehicle
Users can trigger route recalculation for a vehicle's active transport order, choosing either regular or forced rerouting [C-0019, C-1000, C-1001]. If forced rerouting is initiated but the vehicle's current position is unknown or resources for its current position cannot be allocated, the system rejects the request [C-1003, C-1004, C-1005]. Requests are rejected if the targeted vehicle does not exist [C-1002].

### Configure vehicle order profiles, envelopes, and energy thresholds
Users can update the order types a vehicle is permitted to execute—either providing order type names with default priority zero or specifying explicit execution priorities—and modify the vehicle's envelope key or energy level thresholds [C-0030, C-0031, C-0032, C-1100, C-1200, C-1300, C-1400]. Energy threshold updates require critical, good, sufficiently recharged, and fully recharged percentages, each between 0 and 100 inclusive [C-0032, C-1301]. Updating the envelope key is rejected if the vehicle is currently processing an order, allocating resources, or claiming resources [C-1402]. All configuration requests are rejected if the vehicle does not exist [C-1101, C-1201, C-1302, C-1401].

## States and lifecycle
A vehicle maintains operational, processing, and integration states [C-0012, C-0013, C-0014]:
- **Operational State**: Describes hardware and connection readiness as `UNKNOWN` (communication lost), `UNAVAILABLE` (not in error but unavailable for orders), `ERROR` (failure condition), `IDLE` (ready for movement orders), `EXECUTING` (processing movement orders), or `CHARGING` (recharging or refueling) [C-0013].
- **Processing State**: Describes order execution activity as `IDLE` (no order assigned), `AWAITING_ORDER` (assigned to a transport order but awaiting the next drive order), or `PROCESSING_ORDER` (actively executing a drive order) [C-0014].
- **Integration Level**: Governs dispatcher utilization as `TO_BE_IGNORED` (completely ignored by traffic management), `TO_BE_NOTICED` (visible but no resources reserved), `TO_BE_RESPECTED` (resources reserved for reported positions but not assigned orders), or `TO_BE_UTILIZED` (fully available for order assignments by the dispatcher) [C-0012].

## Business rules
- When updating acceptable order types using only a list of order type names, each specified order type is assigned an execution priority of zero [C-1100].
- Bounding box definitions require length, width, height dimensions, and two-dimensional reference offsets [C-0026].
- Route step orientations are evaluated as forward, backward, or undefined [C-0015].
- Route candidate calculations limit results per destination point to one by default if not specified [C-0202].
- Rerouting requests default to regular rerouting unless forced rerouting is explicitly requested [C-1001].
- When a drive order is rerouted without prior executed steps, route continuity is preserved; intermediate steps receive no-operation commands while the destination step retains the target operation [C-1006, C-1007].
- Automatically generated recharge orders created during dispatch runs are assigned the order type `Charge` [C-0103].
- Drive orders are not assigned if redundant order assignment is disabled, the destination point matches the vehicle's position, and the operation is a movement or no-operation [C-0102].
- Block resources restrict simultaneous traffic either to a single vehicle at a time or to multiple vehicles traveling in the same direction [C-0006].
- When rerouting is impossible, dispatchers are configured to ignore locked paths, pause immediately at the source point, or pause upon reaching a locked path [C-0007].

## Glossary
- **Acceptable Order Type**: A transport order classification that an automated guided vehicle is allowed to execute, accompanied by an execution priority [C-0025].
- **Bounding Box**: Physical dimensions (length, width, height) and reference offsets defining the vehicle's physical footprint [C-0026].
- **Communication Adapter**: Driver software component interfacing between openTCS and the physical vehicle hardware [C-0028, C-0300].
- **Envelope Key**: An identifier referencing the geometric boundary envelope associated with a vehicle [C-0034, C-1400].
- **Integration Level**: Operational setting determining whether a vehicle is ignored, noticed, respected, or utilized by the dispatcher [C-0012].
- **Processing State**: Indicator of whether a vehicle is idle, waiting for a subsequent drive order, or actively executing a drive order [C-0014].

## Requirements
- VEH-A1: The system shall return all vehicles and their details sorted ascending by name [C-1600].
- VEH-A2: The system shall filter vehicle listings by vehicle processing state when requested [C-1601].
- VEH-A3: The system shall return all vehicles regardless of processing state when no filter is requested [C-1602].
- VEH-A4: The system shall reject vehicle listing requests that supply an invalid processing state [C-1603].
- VEH-A5: The system shall return state and telemetry details for a specified vehicle [C-0034, C-1500].
- VEH-A6: The system shall reject requests for vehicle details if the vehicle does not exist [C-1501].
- VEH-B1: The system shall compute navigation routes avoiding specified resources from a source point to destinations for a vehicle [C-0033, C-0200, C-0201].
- VEH-B2: The system shall default the source point to the vehicle's current position when omitted [C-0206].
- VEH-B3: The system shall default the maximum routes per destination point to one when omitted [C-0202].
- VEH-B4: The system shall reject route calculation requests if the vehicle does not exist [C-0205].
- VEH-B5: The system shall reject route calculation requests if the source point is omitted and the vehicle has no known position [C-0207].
- VEH-B6: The system shall reject route calculation requests if the source point, any destination point, or any resource to avoid does not exist [C-0208, C-0209, C-0210].
- VEH-B7: The system shall reject route calculation requests if the route limit parameter is not an integer [C-0204].
- VEH-C1: The system shall return communication adapter attachment details for a vehicle [C-0028, C-0400, C-0401].
- VEH-C2: The system shall reject attachment detail queries if the vehicle does not exist [C-0402].
- VEH-C3: The system shall attach a specified communication adapter driver to a vehicle and record it as the preferred adapter [C-0300, C-0303].
- VEH-C4: The system shall reject adapter attachment requests if the vehicle does not exist or the adapter is not among the available adapters [C-0301, C-0302].
- VEH-C5: The system shall enable or disable a vehicle's communication adapter [C-0500].
- VEH-C6: The system shall reject adapter enable or disable requests if the vehicle does not exist or has no entry [C-0501, C-0502, C-0503].
- VEH-C7: The system shall deliver custom messages with key-value parameters to a vehicle's communication adapter [C-0029, C-0600].
- VEH-C8: The system shall reject adapter messages if the vehicle does not exist [C-0601].
- VEH-D1: The system shall trigger a full fleet dispatching run [C-0100].
- VEH-D2: The system shall update the paused state of a vehicle [C-0700].
- VEH-D3: The system shall reject pause updates if the vehicle does not exist [C-0701].
- VEH-D4: The system shall update a vehicle's integration level [C-0800].
- VEH-D5: The system shall reject integration level updates to ignored or noticed while the vehicle is processing an order [C-0802].
- VEH-D6: The system shall reject integration level updates if the vehicle does not exist [C-0801].
- VEH-D7: The system shall withdraw transport orders assigned to a vehicle [C-0900].
- VEH-D8: The system shall set a vehicle's integration level to respected upon withdrawal when requested [C-0902].
- VEH-D9: The system shall reject order withdrawal requests if the vehicle does not exist [C-0903].
- VEH-E1: The system shall request regular or forced rerouting for a vehicle [C-1000, C-1001].
- VEH-E2: The system shall reject rerouting requests if the vehicle does not exist [C-1002].
- VEH-E3: The system shall reject forced rerouting requests if the vehicle position is unknown or resources for its position cannot be allocated [C-1003, C-1004, C-1005].
- VEH-F1: The system shall update acceptable order types with default priority zero [C-1100].
- VEH-F2: The system shall reject acceptable order type updates if the vehicle does not exist [C-1101].
- VEH-F3: The system shall update acceptable order types with explicit priorities [C-0030, C-1200].
- VEH-F4: The system shall reject acceptable order type priority updates if the vehicle does not exist [C-1201].
- VEH-F5: The system shall update vehicle energy level threshold sets [C-0032, C-1300].
- VEH-F6: The system shall reject energy threshold updates if any value is outside 0 to 100 or the vehicle does not exist [C-1301, C-1302].
- VEH-F7: The system shall update a vehicle's envelope key [C-1400].
- VEH-F8: The system shall reject envelope key updates if the vehicle is processing an order, allocating resources, or claiming resources [C-1402].
- VEH-F9: The system shall reject envelope key updates if the vehicle does not exist [C-1401].

## Open items
- Published docs state that vehicle processing state includes UNAVAILABLE alongside IDLE, AWAITING_ORDER, and PROCESSING_ORDER, whereas the processing state model defines IDLE, AWAITING_ORDER, and PROCESSING_ORDER [C-0014].
- Published docs state that the vehicle property for preferred adapter is `tcs:preferredAdapterClass`, whereas the system sets property `tcs:preferredAdapter` [C-0303].
- Published docs state that peripheral operation execution triggers are limited to AFTER_ALLOCATION and AFTER_MOVEMENT, whereas IMMEDIATE is also supported [C-0018].
- Published docs specify adapter message parameters as a JSON array, whereas the implementation accepts key-value parameters [C-0029].

## Appendix: interface reference

| Operation | Request | Inputs | Success Result | Failure Cases | Claims |
|---|---|---|---|---|---|
| Trigger dispatcher | POST /v1/vehicles/dispatcher/trigger | None | HTTP 200 (empty text response) | HTTP 500 on kernel runtime error or interrupted execution | [C-0100, C-0106, C-0107] |
| List vehicles | GET /v1/vehicles | Query: `procState` (optional vehicle processing state string) | HTTP 200 with JSON list of vehicle objects sorted by name | HTTP 400 if `procState` is invalid; HTTP 500 on kernel error | [C-0005, C-1600, C-1601, C-1602, C-1603] |
| Get vehicle | GET /v1/vehicles/{NAME} | Path: `NAME` | HTTP 200 with vehicle details JSON | HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0034, C-1500, C-1501] |
| Compute vehicle routes | POST /v1/vehicles/{NAME}/routes | Path: `NAME`; Query: `maxRoutesPerDestinationPoint` (optional integer); Body: JSON with `destinationPoints` (required list), `sourcePoint` (optional string), `resourcesToAvoid` (optional list) | HTTP 200 with computed route steps JSON | HTTP 400 on malformed JSON, invalid query int, or missing source point when vehicle position unknown; HTTP 404 if vehicle, source point, destination point, or avoid resource not found; HTTP 500 on routing or serialization failure | [C-0200, C-0201, C-0202, C-0203, C-0204, C-0205, C-0206, C-0207, C-0208, C-0209, C-0210, C-0211, C-0212] |
| Get comm adapter attachment info | GET /v1/vehicles/{NAME}/commAdapter/attachmentInformation | Path: `NAME` | HTTP 200 with attachment info JSON | HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0028, C-0400, C-0401, C-0402] |
| Attach comm adapter | PUT /v1/vehicles/{NAME}/commAdapter/attachmentInformation | Path: `NAME`; Query: `newValue` (adapter class name) | HTTP 200 (empty text response) | HTTP 400 if adapter not available; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0300, C-0301, C-0302, C-0303] |
| Enable/disable comm adapter | PUT /v1/vehicles/{NAME}/commAdapter/enabled | Path: `NAME`; Query: `newValue` (boolean) | HTTP 200 (empty text response) | HTTP 400 if vehicle entry not found; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0500, C-0501, C-0502, C-0503] |
| Send comm adapter message | POST /v1/vehicles/{NAME}/commAdapter/message | Path: `NAME`; Body: JSON message type and key-value parameters | HTTP 200 (empty text response) | HTTP 400 on malformed body; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0001, C-0005, C-0029, C-0600, C-0601] |
| Set paused state | PUT /v1/vehicles/{NAME}/paused | Path: `NAME`; Query: `newValue` (boolean) | HTTP 200 (empty text response) | HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0700, C-0701] |
| Set integration level | PUT /v1/vehicles/{NAME}/integrationLevel | Path: `NAME`; Query: `newValue` (integration level string) | HTTP 200 (empty text response) | HTTP 400 if set to TO_BE_IGNORED/TO_BE_NOTICED while processing order; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0800, C-0801, C-0802] |
| Withdraw transport order | POST /v1/vehicles/{NAME}/withdrawal | Path: `NAME`; Query: `disableVehicle` (optional boolean) | HTTP 200 (empty text response) | HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0900, C-0902, C-0903] |
| Request reroute | POST /v1/vehicles/{NAME}/rerouteRequest | Path: `NAME`; Query: `forced` (optional boolean) | HTTP 200 (empty text response) | HTTP 400 if forced and position unknown or resources unallocatable; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-1000, C-1001, C-1002, C-1003, C-1004, C-1005] |
| Set acceptable order types (default priority) | PUT /v1/vehicles/{NAME}/acceptableOrderTypes | Path: `NAME`; Body: JSON array of order type strings | HTTP 200 (empty text response) | HTTP 400 on malformed body; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0001, C-0005, C-0031, C-1100, C-1101] |
| Set acceptable order types with priorities | PUT /v1/vehicles/{NAME}/acceptableOrderTypes | Path: `NAME`; Body: JSON list of order types with name and priority | HTTP 200 (empty text response) | HTTP 400 on malformed body; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0001, C-0005, C-0025, C-0030, C-1200, C-1201] |
| Set energy level thresholds | PUT /v1/vehicles/{NAME}/energyLevelThresholdSet | Path: `NAME`; Body: JSON threshold set (critical, good, sufficientlyRecharged, fullyRecharged) | HTTP 200 (empty text response) | HTTP 400 if thresholds not 0-100; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-0032, C-1300, C-1301, C-1302] |
| Set envelope key | PUT /v1/vehicles/{NAME}/envelopeKey | Path: `NAME`; Query: `newValue` (envelope key string) | HTTP 200 (empty text response) | HTTP 400 if processing order, claiming, or allocating resources; HTTP 404 if vehicle unknown; HTTP 500 on kernel error | [C-0005, C-1400, C-1401, C-1402] |
