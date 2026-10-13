# Vehicles — User and Integration Guide

> Generated from code at [`742b0e6d`](https://github.com/alxp1982/opentcs/commit/742b0e6df9ccbabc660e585a85e55d585c0642fc) · fact coverage 85.9% · 76 verified claims

## Overview

The vehicles interface allows external clients to monitor vehicle fleet states, configure operating parameters, manage communication adapter drivers, compute routes, and issue operational commands such as pausing, rerouting, or withdrawing orders.

Fleet vehicles can be queried across their lifecycle, tracked by coordinates and resource allocations, and filtered by processing states.

## How to …

### Retrieve vehicles

To list all vehicles in the system, retrieve the vehicle collection.  The returned vehicles are sorted in ascending order by vehicle name.

Callers can filter the list by vehicle processing state using the `procState` query parameter.  Supported vehicle processing states are `IDLE`, `AWAITING_ORDER`, and `PROCESSING_ORDER`.

To view detailed information for a single vehicle, request the vehicle by its name.  The response includes the vehicle's position, coordinates, orientation, current and processing states with timestamps, pause status, energy levels, claimed and allocated resources, bounding box, acceptable order types, properties, and assigned transport order.

### Manage communication adapters

Each vehicle communicates via a communication adapter.  Callers can manage adapter attachments and execution states:

- **Check adapter attachment**: Retrieve attachment information for a vehicle by name to receive the vehicle name, the currently attached communication adapter, and a list of available adapters.
- **Attach an adapter**: Attach an adapter to a vehicle by providing the adapter driver class name in the `newValue` query parameter.  When attaching a new adapter, any previously attached adapter is disabled and terminated before the new adapter is initialized and attached.  The vehicle's preferred adapter property is automatically updated with the factory class name of the attached adapter.
- **Enable or disable an adapter**: Change the adapter's enabled state using the `newValue` query parameter (`true` to enable, `false` to disable).
- **Send custom adapter messages**: Send a custom message containing a message type and key-value parameters to the communication adapter of a specified vehicle.

### Control vehicle operation

- **Pause or resume a vehicle**: Set the vehicle's paused status by sending a boolean value in the `newValue` query parameter.  Updating the paused status updates the vehicle model, notifies the vehicle controller, and emits a modification event.
- **Update integration level**: Update how the vehicle interacts with traffic management using the `newValue` query parameter.  An entity's integration level indicates whether it is ignored (`TO_BE_IGNORED`), displayed without impacting traffic management (`TO_BE_NOTICED`), or taken into account by traffic management (`TO_BE_RESPECTED`).  Successfully updating the integration level emits an object modified event.
- **Withdraw an assigned transport order**: Withdraw any transport order currently assigned to a vehicle.  The `immediate` query parameter controls withdrawal behavior (defaults to `false`, or evaluates to `true` when set to `true`).  Setting the `disableVehicle` query parameter to `true` updates the vehicle's integration level to `TO_BE_RESPECTED`; otherwise it defaults to false.
- **Reroute a vehicle**: Trigger a route recalculation for a vehicle.  When the `forced` query parameter is `true`, forced rerouting is used; otherwise, regular rerouting is requested by default.  Regular rerouting respects current resource allocations, whereas forced rerouting reroutes from the current position disregarding allocations.

### Configure vehicle order acceptance and thresholds

- **Set allowed order types**: Update the transport order types a vehicle is permitted to process by providing a list of order type names in the request body.  Each allowed order type is assigned a priority of 0.
- **Set acceptable order types with priorities**: Update the full list of acceptable order types by specifying each transport order type name along with its dispatching priority in the request body.
- **Configure energy level thresholds**: Update vehicle energy level thresholds by supplying critical, good, sufficiently recharged, and fully recharged values in the request body.
- **Update envelope key**: Update a vehicle's envelope key using the `newValue` query parameter.

### Calculate routes

Compute potential routes for a vehicle to one or more destination points using the vehicle's routing capabilities.  Callers specify destination points and optional resources to avoid in the request body.

- If the request body omits `sourcePoint`, the vehicle's current position is used as the starting point.
- The maximum number of routes calculated per destination point can be controlled via the `maxRoutesPerDestinationPoint` query parameter, which defaults to `1` when omitted.
- The response returns routes detailing total costs, destination points, and route steps specifying source point, destination point, path, and vehicle orientation (`FORWARD`, `BACKWARD`, or `UNDEFINED`).

## Field reference

### Vehicle representation

The vehicle state and status responses describe attributes of a vehicle:

| Field | Meaning |
|---|---|
| `name` | Unique identifier for the vehicle.  |
| `state` | Current operational state of the vehicle, reported with a timestamp.  |
| `procState` | Vehicle processing state (`IDLE`, `AWAITING_ORDER`, or `PROCESSING_ORDER`), reported with a timestamp.  |
| `paused` | Boolean flag indicating whether the vehicle is paused.  |
| `currentPosition` | Current topological point of the vehicle.  |
| `precisePosition` | Precise 3D coordinates (`x`, `y`, `z`) of the vehicle.  |
| `orientationAngle` | Vehicle orientation angle.  |
| `energyLevel` | Current energy level of the vehicle.  |
| `energyLevelCritical` | Configured critical energy level threshold.  |
| `energyLevelGood` | Configured good energy level threshold.  |
| `energyLevelSufficientlyRecharged` | Configured sufficiently recharged energy level threshold.  |
| `energyLevelFullyRecharged` | Configured fully recharged energy level threshold.  |
| `claimedResources` | Sequence of resources claimed by the vehicle.  |
| `allocatedResources` | Sequence of resources currently allocated to the vehicle.  |
| `envelopeKey` | Key identifying the vehicle's envelope profile.  |
| `boundingBox` | Dimensions of length, width, and height with a 2D reference offset (`x`, `y`).  |
| `acceptableOrderTypes` | List of acceptable transport order types and their dispatching priorities.  |
| `properties` | Custom key-value string pairs associated with the vehicle.  |
| `transportOrder` | Identifier of the transport order currently assigned to the vehicle.  |

### Energy level threshold request

Vehicle energy level threshold sets configure critical, good, sufficiently recharged, and fully recharged energy levels:

| Field | Meaning |
|---|---|
| `energyLevelCritical` | Critical energy level percentage threshold (0–100).  |
| `energyLevelGood` | Good energy level percentage threshold (0–100).  |
| `energyLevelSufficientlyRecharged` | Sufficiently recharged energy level percentage threshold (0–100).  |
| `energyLevelFullyRecharged` | Fully recharged energy level percentage threshold (0–100).  |

### Adapter message request

| Field | Meaning |
|---|---|
| `type` | Custom message type identifier sent to the communication adapter.  |
| `parameters` | Key-value pairs representing parameters forwarded with the message.  |

## Errors and what to do

| Symptom | Meaning | What to do |
|---|---|---|
| HTTP 400 Bad Request on vehicle list | The provided `procState` query parameter is not a valid vehicle processing state name.  | Use a valid processing state value: `IDLE`, `AWAITING_ORDER`, or `PROCESSING_ORDER`.  |
| HTTP 400 Bad Request on route calculation | Invalid parameter, such as non-integer `maxRoutesPerDestinationPoint`, malformed JSON body, or an omitted `sourcePoint` when the vehicle has no known position.  | Provide an integer value for route count, verify the request body JSON syntax, or ensure `sourcePoint` is specified if the vehicle position is unknown.  |
| HTTP 400 Bad Request on adapter attachment | The `newValue` parameter does not match the class name of any available communication adapter description for the vehicle.  | Query the vehicle attachment information first and select an adapter class name from the available adapters list.  |
| HTTP 400 Bad Request on enabling/disabling adapter | No vehicle entry is found for the vehicle reference.  | Verify the vehicle controller and adapter configuration before enabling or disabling the adapter.  |
| HTTP 400 Bad Request on setting integration level | Attempted to set integration level to `TO_BE_IGNORED` or `TO_BE_NOTICED` while the vehicle is currently processing an order.  | Wait until the vehicle finishes processing its current order, or withdraw the order before changing the integration level.  |
| HTTP 400 Bad Request on rerouting | Forced rerouting was requested, but the vehicle position is unknown, resources cannot be allocated immediately, or an error occurred during resource allocation.  | Verify the vehicle has a reported position and that its current position resources are available for allocation before applying forced rerouting.  |
| HTTP 400 Bad Request on energy thresholds | One or more threshold values (`energyLevelCritical`, `energyLevelGood`, `energyLevelSufficientlyRecharged`, or `energyLevelFullyRecharged`) are outside the range of 0 to 100 inclusive.  | Supply integer percentage values between 0 and 100 for all threshold fields.  |
| HTTP 400 Bad Request on envelope key update | The vehicle is currently processing an order, claiming resources, or allocating resources.  | Ensure the vehicle is not processing an order, claiming resources, or allocating resources before updating its envelope key.  |
| HTTP 404 Not Found on vehicle operations | No vehicle exists with the specified name.  | Verify the vehicle name against the registered vehicle list.  |
| HTTP 404 Not Found on route calculation points | The specified `sourcePoint`, one of the `destinationPoints`, or a resource in `resourcesToAvoid` cannot be found or is not a recognized point, path, or location.  | Check that all source, destination, and avoided resource identifiers correspond to existing model topology entities.  |
| HTTP 500 Internal Server Error on route calculation | An unexpected error occurred while computing the shortest path or serializing the response to JSON.  | Check system connectivity and retry the routing calculation.  |
| HTTP 500 Internal Server Error on general requests | An internal state error or runtime exception occurred during request processing.  | Check server logs for internal kernel details and retry the request.  |
