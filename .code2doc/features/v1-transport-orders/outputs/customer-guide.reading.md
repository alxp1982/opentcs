# Transport Orders — User and Integration Guide

> Generated from code at [`742b0e6d`](https://github.com/alxp1982/opentcs/commit/742b0e6df9ccbabc660e585a85e55d585c0642fc) · fact coverage 100.0% · 48 verified claims

## Overview

The transport orders interface allows host systems to create, manage, query, and withdraw transport orders assigned to automated vehicles . Transport orders define tasks and movement sequences within the plant model, coordinating destinations, execution routes, and vehicle assignments .

## How to …

### Create a transport order

To create a new transport order, submit a request specifying the order name in the path and provide a JSON request body containing the order parameters .

- A transport order creation request requires a list of destinations .
- For each destination, specify the target destination location name, the operation to perform, and optionally a list of properties .
- Destinations targeting a point must specify either the "MOVE" or "PARK" operation .
- Destinations targeting a location must specify an operation that is either "NOP" or allowed by both the location type and at least one attached link .
- All destination points and locations must exist in the plant model .
- Optional creation parameters include an incomplete name flag, dispensability, deadline, intended vehicle, peripheral reservation token, wrapping sequence, type, properties, and order dependencies .
- If the order type is omitted, it defaults to "-" (no specific type) .
- When adding an order to a wrapping sequence, the order type must match the allowed sequence order types unless the sequence allows any type ("*") .

On success, the API responds with JSON content type and the created order details .

### Retrieve transport orders

- Retrieve a single transport order by specifying its name in the path parameter .
- Retrieve all transport orders, which are returned sorted by name in ascending order .
- Filter the list of transport orders by specifying the `intendedVehicle` query parameter, returning only orders intended for that named vehicle .

### Update the intended vehicle

Callers can update the intended vehicle for a transport order using the order's name parameter .

- Setting the `vehicle` query parameter assigns the transport order to the named vehicle .
- Omitting the `vehicle` query parameter clears the intended vehicle from the order .
- The transport order must be in RAW, ACTIVE, or DISPATCHABLE state to update its intended vehicle .

### Request immediate assignment

Callers can request immediate assignment of a transport order identified by its name path parameter to its intended vehicle .

### Withdraw a transport order

Callers can withdraw a transport order identified by its name path parameter via the dispatcher .

- Setting the `immediate` query parameter to true requests an immediate withdrawal, while setting it to false or omitting it requests a non-immediate withdrawal .
- Setting the `disableVehicle` query parameter to true updates the assigned processing vehicle's integration level to TO_BE_RESPECTED if the transport order has an assigned processing vehicle .

### Trigger order dispatching

Callers can trigger a dispatcher run to assign transport orders . On success, this request returns an empty text/plain; charset=utf-8 response .

## Field reference

### Transport order fields

| Field | Description |
|---|---|
| Name | The unique name of the transport order . |
| Dispensable | Flag indicating whether the order is dispensable . |
| Type | The transport order type, defaulting to "-" if omitted during creation . |
| State | Lifecycle state of the order: RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, or UNROUTABLE . |
| Destinations | List of drive order destinations . |
| Drive orders | List of drive orders associated with the transport order . |
| Current drive order index | Index of the current drive order being executed . |
| Current route step index | Index of the current route step being executed . |
| Creation timestamp | Timestamp when the transport order was created . |
| Deadline timestamp | Due timestamp for completing the transport order . |
| Finished timestamp | Timestamp when the transport order completed . |
| History | List of historical events recorded for the order . |
| Dependencies | Transport orders that this order depends upon . |
| Intended vehicle | The vehicle designated to execute the order . |
| Processing vehicle | The vehicle currently executing the order . |
| Peripheral reservation token | Reservation token for peripheral devices . |
| Wrapping sequence | The order sequence wrapping this transport order . |
| Properties | Custom key-value properties attached to the order . |

### Drive orders and destinations

- A drive order representation reports the drive order name, destination details, parent transport order name, planned route, and execution state .
- A drive order execution state can be PRISTINE, TRAVELLING, OPERATING, FINISHED, or FAILED .
- A destination state describes a drive order's destination by its location name, operation, list of properties, and execution state .
- A destination state can be PRISTINE, TRAVELLING, OPERATING, FINISHED, or FAILED .
- A drive order destination specifies the destination point or location name, the operation to perform, and a list of properties .

### Routes and route steps

- A route contains the total routing costs and an ordered list of route steps .
- Each route step specifies the source point, destination point, connecting path, vehicle orientation, route index, costs, whether execution is allowed, and optional rerouting type .
- The vehicle orientation indicates whether the vehicle is traveling FORWARD, BACKWARD, or UNDEFINED .
- The rerouting type indicates whether rerouting was REGULAR or FORCED .

### Properties and history

- A property consists of a required key string and an optional string value .
- An object history entry records the event timestamp, event code, and supplemental details .

## Errors and what to do

| Symptom | Meaning | What to do |
|---|---|---|
| HTTP 400 Bad Request | Malformed JSON in request body . | Verify that the JSON request payload is properly formatted . |
| HTTP 400 Bad Request | A destination targeting a point specifies an operation other than "MOVE" or "PARK" . | Specify "MOVE" or "PARK" when targeting a point . |
| HTTP 400 Bad Request | A destination targeting a location specifies an operation that is neither "NOP" nor allowed by both the location type and at least one attached link . | Check the location type and link definitions in the plant model to ensure the operation is allowed, or use "NOP" . |
| HTTP 400 Bad Request | The order type does not match the allowed types of the wrapping sequence and the sequence does not allow any type . | Ensure the transport order type matches the allowed types for the wrapping sequence . |
| HTTP 400 Bad Request | Attempted to update the intended vehicle for an order that is not in RAW, ACTIVE, or DISPATCHABLE state . | Update the intended vehicle only while the order is in RAW, ACTIVE, or DISPATCHABLE state . |
| HTTP 400 Bad Request | The request triggers an illegal argument error . | Check request parameters and payload structure against the API requirements . |
| HTTP 404 Not Found | The referenced transport order name does not exist . | Verify that the transport order name exists before requesting it . |
| HTTP 404 Not Found | A destination references a location or point name that does not exist in the plant model . | Ensure all destination point and location names exist in the plant model . |
| HTTP 404 Not Found | The vehicle specified in the query parameter does not exist . | Verify the vehicle name against existing vehicles in the system . |
| HTTP 404 Not Found | A referenced object is unknown . | Ensure all referenced entities exist . |
| HTTP 409 Conflict | The object being created already exists . | Use a unique name for the entity being created . |
| HTTP 500 Internal Server Error | Serializing the response to JSON failed . | Check server logs for details regarding payload serialization . |
| HTTP 500 Internal Server Error | Kernel execution was unexpectedly interrupted or encountered an unexpected runtime failure . | Retry the request or inspect system state and logs . |
