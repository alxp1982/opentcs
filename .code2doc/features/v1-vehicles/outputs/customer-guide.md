# Vehicles — User and Integration Guide

## Overview

The vehicles interface allows external clients to monitor vehicle fleet states, configure operating parameters, manage communication adapter drivers, compute routes, and issue operational commands such as pausing, rerouting, or withdrawing orders. [C-0038, C-0200, C-0300, C-0500, C-0700, C-0900, C-1000, C-1600]

Fleet vehicles can be queried across their lifecycle, tracked by coordinates and resource allocations, and filtered by processing states. [C-0038, C-1600, C-1601]

## How to …

### Retrieve vehicles

To list all vehicles in the system, retrieve the vehicle collection. [C-1600] The returned vehicles are sorted in ascending order by vehicle name. [C-1600]

Callers can filter the list by vehicle processing state using the `procState` query parameter. [C-1601] Supported vehicle processing states are `IDLE`, `AWAITING_ORDER`, and `PROCESSING_ORDER`. [C-0029]

To view detailed information for a single vehicle, request the vehicle by its name. [C-1500] The response includes the vehicle's position, coordinates, orientation, current and processing states with timestamps, pause status, energy levels, claimed and allocated resources, bounding box, acceptable order types, properties, and assigned transport order. [C-0038, C-0052, C-1500]

### Manage communication adapters

Each vehicle communicates via a communication adapter. [C-0037, C-0300] Callers can manage adapter attachments and execution states:

- **Check adapter attachment**: Retrieve attachment information for a vehicle by name to receive the vehicle name, the currently attached communication adapter, and a list of available adapters. [C-0037, C-0400]
- **Attach an adapter**: Attach an adapter to a vehicle by providing the adapter driver class name in the `newValue` query parameter. [C-0300] When attaching a new adapter, any previously attached adapter is disabled and terminated before the new adapter is initialized and attached. [C-0303] The vehicle's preferred adapter property is automatically updated with the factory class name of the attached adapter. [C-0304]
- **Enable or disable an adapter**: Change the adapter's enabled state using the `newValue` query parameter (`true` to enable, `false` to disable). [C-0500, C-0501]
- **Send custom adapter messages**: Send a custom message containing a message type and key-value parameters to the communication adapter of a specified vehicle. [C-0044, C-0600, C-0601, C-0602]

### Control vehicle operation

- **Pause or resume a vehicle**: Set the vehicle's paused status by sending a boolean value in the `newValue` query parameter. [C-0700] Updating the paused status updates the vehicle model, notifies the vehicle controller, and emits a modification event. [C-0702]
- **Update integration level**: Update how the vehicle interacts with traffic management using the `newValue` query parameter. [C-0800] An entity's integration level indicates whether it is ignored (`TO_BE_IGNORED`), displayed without impacting traffic management (`TO_BE_NOTICED`), or taken into account by traffic management (`TO_BE_RESPECTED`). [C-0009] Successfully updating the integration level emits an object modified event. [C-0803]
- **Withdraw an assigned transport order**: Withdraw any transport order currently assigned to a vehicle. [C-0900] The `immediate` query parameter controls withdrawal behavior (defaults to `false`, or evaluates to `true` when set to `true`). [C-0901] Setting the `disableVehicle` query parameter to `true` updates the vehicle's integration level to `TO_BE_RESPECTED`; otherwise it defaults to false. [C-0902]
- **Reroute a vehicle**: Trigger a route recalculation for a vehicle. [C-1000] When the `forced` query parameter is `true`, forced rerouting is used; otherwise, regular rerouting is requested by default. [C-1001] Regular rerouting respects current resource allocations, whereas forced rerouting reroutes from the current position disregarding allocations. [C-0021]

### Configure vehicle order acceptance and thresholds

- **Set allowed order types**: Update the transport order types a vehicle is permitted to process by providing a list of order type names in the request body. [C-0048, C-1100] Each allowed order type is assigned a priority of 0. [C-1100]
- **Set acceptable order types with priorities**: Update the full list of acceptable order types by specifying each transport order type name along with its dispatching priority in the request body. [C-0033, C-1200]
- **Configure energy level thresholds**: Update vehicle energy level thresholds by supplying critical, good, sufficiently recharged, and fully recharged values in the request body. [C-0049, C-1300]
- **Update envelope key**: Update a vehicle's envelope key using the `newValue` query parameter. [C-1400]

### Calculate routes

Compute potential routes for a vehicle to one or more destination points using the vehicle's routing capabilities. [C-0200] Callers specify destination points and optional resources to avoid in the request body. [C-0045, C-0200]

- If the request body omits `sourcePoint`, the vehicle's current position is used as the starting point. [C-0206]
- The maximum number of routes calculated per destination point can be controlled via the `maxRoutesPerDestinationPoint` query parameter, which defaults to `1` when omitted. [C-0201]
- The response returns routes detailing total costs, destination points, and route steps specifying source point, destination point, path, and vehicle orientation (`FORWARD`, `BACKWARD`, or `UNDEFINED`). [C-0025, C-0045]

## Field reference

### Vehicle representation

The vehicle state and status responses describe attributes of a vehicle: [C-0038, C-0052]

| Field | Meaning |
|---|---|
| `name` | Unique identifier for the vehicle. [C-0038] |
| `state` | Current operational state of the vehicle, reported with a timestamp. [C-0038, C-0052] |
| `procState` | Vehicle processing state (`IDLE`, `AWAITING_ORDER`, or `PROCESSING_ORDER`), reported with a timestamp. [C-0029, C-0038, C-0052] |
| `paused` | Boolean flag indicating whether the vehicle is paused. [C-0038, C-0052] |
| `currentPosition` | Current topological point of the vehicle. [C-0038, C-0052] |
| `precisePosition` | Precise 3D coordinates (`x`, `y`, `z`) of the vehicle. [C-0038, C-0052] |
| `orientationAngle` | Vehicle orientation angle. [C-0038, C-0052] |
| `energyLevel` | Current energy level of the vehicle. [C-0038, C-0052] |
| `energyLevelCritical` | Configured critical energy level threshold. [C-0038, C-0049] |
| `energyLevelGood` | Configured good energy level threshold. [C-0038, C-0049] |
| `energyLevelSufficientlyRecharged` | Configured sufficiently recharged energy level threshold. [C-0038, C-0049] |
| `energyLevelFullyRecharged` | Configured fully recharged energy level threshold. [C-0038, C-0049] |
| `claimedResources` | Sequence of resources claimed by the vehicle. [C-0038, C-0052] |
| `allocatedResources` | Sequence of resources currently allocated to the vehicle. [C-0038, C-0052] |
| `envelopeKey` | Key identifying the vehicle's envelope profile. [C-0038] |
| `boundingBox` | Dimensions of length, width, and height with a 2D reference offset (`x`, `y`). [C-0034, C-0038, C-0052] |
| `acceptableOrderTypes` | List of acceptable transport order types and their dispatching priorities. [C-0033, C-0038, C-0052] |
| `properties` | Custom key-value string pairs associated with the vehicle. [C-0038, C-0046, C-0052] |
| `transportOrder` | Identifier of the transport order currently assigned to the vehicle. [C-0038, C-0052] |

### Energy level threshold request

Vehicle energy level threshold sets configure critical, good, sufficiently recharged, and fully recharged energy levels: [C-0049, C-1300]

| Field | Meaning |
|---|---|
| `energyLevelCritical` | Critical energy level percentage threshold (0–100). [C-0049, C-1302] |
| `energyLevelGood` | Good energy level percentage threshold (0–100). [C-0049, C-1302] |
| `energyLevelSufficientlyRecharged` | Sufficiently recharged energy level percentage threshold (0–100). [C-0049, C-1302] |
| `energyLevelFullyRecharged` | Fully recharged energy level percentage threshold (0–100). [C-0049, C-1302] |

### Adapter message request

| Field | Meaning |
|---|---|
| `type` | Custom message type identifier sent to the communication adapter. [C-0044, C-0602] |
| `parameters` | Key-value pairs representing parameters forwarded with the message. [C-0044, C-0046, C-0602] |

## Errors and what to do

| Symptom | Meaning | What to do |
|---|---|---|
| HTTP 400 Bad Request on vehicle list | The provided `procState` query parameter is not a valid vehicle processing state name. [C-1602] | Use a valid processing state value: `IDLE`, `AWAITING_ORDER`, or `PROCESSING_ORDER`. [C-0029, C-1602] |
| HTTP 400 Bad Request on route calculation | Invalid parameter, such as non-integer `maxRoutesPerDestinationPoint`, malformed JSON body, or an omitted `sourcePoint` when the vehicle has no known position. [C-0202, C-0203, C-0207] | Provide an integer value for route count, verify the request body JSON syntax, or ensure `sourcePoint` is specified if the vehicle position is unknown. [C-0202, C-0203, C-0207] |
| HTTP 400 Bad Request on adapter attachment | The `newValue` parameter does not match the class name of any available communication adapter description for the vehicle. [C-0302] | Query the vehicle attachment information first and select an adapter class name from the available adapters list. [C-0037, C-0302, C-0400] |
| HTTP 400 Bad Request on enabling/disabling adapter | No vehicle entry is found for the vehicle reference. [C-0503, C-0504] | Verify the vehicle controller and adapter configuration before enabling or disabling the adapter. [C-0503, C-0504] |
| HTTP 400 Bad Request on setting integration level | Attempted to set integration level to `TO_BE_IGNORED` or `TO_BE_NOTICED` while the vehicle is currently processing an order. [C-0802] | Wait until the vehicle finishes processing its current order, or withdraw the order before changing the integration level. [C-0802, C-0900] |
| HTTP 400 Bad Request on rerouting | Forced rerouting was requested, but the vehicle position is unknown, resources cannot be allocated immediately, or an error occurred during resource allocation. [C-1003, C-1004, C-1005] | Verify the vehicle has a reported position and that its current position resources are available for allocation before applying forced rerouting. [C-1003, C-1004, C-1005] |
| HTTP 400 Bad Request on energy thresholds | One or more threshold values (`energyLevelCritical`, `energyLevelGood`, `energyLevelSufficientlyRecharged`, or `energyLevelFullyRecharged`) are outside the range of 0 to 100 inclusive. [C-1302] | Supply integer percentage values between 0 and 100 for all threshold fields. [C-1302] |
| HTTP 400 Bad Request on envelope key update | The vehicle is currently processing an order, claiming resources, or allocating resources. [C-1402] | Ensure the vehicle is not processing an order, claiming resources, or allocating resources before updating its envelope key. [C-1402] |
| HTTP 404 Not Found on vehicle operations | No vehicle exists with the specified name. [C-0205, C-0301, C-0401, C-0502, C-0603, C-0701, C-0801, C-0903, C-1002, C-1101, C-1201, C-1301, C-1401, C-1501] | Verify the vehicle name against the registered vehicle list. [C-1501, C-1600] |
| HTTP 404 Not Found on route calculation points | The specified `sourcePoint`, one of the `destinationPoints`, or a resource in `resourcesToAvoid` cannot be found or is not a recognized point, path, or location. [C-0208, C-0209, C-0210] | Check that all source, destination, and avoided resource identifiers correspond to existing model topology entities. [C-0208, C-0209, C-0210] |
| HTTP 500 Internal Server Error on route calculation | An unexpected error occurred while computing the shortest path or serializing the response to JSON. [C-0204, C-0211] | Check system connectivity and retry the routing calculation. [C-0204, C-0211] |
| HTTP 500 Internal Server Error on general requests | An internal state error or runtime exception occurred during request processing. [C-0054, C-0057] | Check server logs for internal kernel details and retry the request. [C-0054, C-0057] |
