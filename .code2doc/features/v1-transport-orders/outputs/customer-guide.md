# Transport Orders — User and Integration Guide

## Overview

The transport orders interface allows host systems to create, manage, query, and withdraw transport orders assigned to automated vehicles [C-0020, C-0400]. Transport orders define tasks and movement sequences within the plant model, coordinating destinations, execution routes, and vehicle assignments [C-0008, C-0011, C-0020].

## How to …

### Create a transport order

To create a new transport order, submit a request specifying the order name in the path and provide a JSON request body containing the order parameters [C-0400]. 

- A transport order creation request requires a list of destinations [C-0008].
- For each destination, specify the target destination location name, the operation to perform, and optionally a list of properties [C-0007].
- Destinations targeting a point must specify either the "MOVE" or "PARK" operation [C-0405].
- Destinations targeting a location must specify an operation that is either "NOP" or allowed by both the location type and at least one attached link [C-0406].
- All destination points and locations must exist in the plant model [C-0404].
- Optional creation parameters include an incomplete name flag, dispensability, deadline, intended vehicle, peripheral reservation token, wrapping sequence, type, properties, and order dependencies [C-0008].
- If the order type is omitted, it defaults to "-" (no specific type) [C-0403].
- When adding an order to a wrapping sequence, the order type must match the allowed sequence order types unless the sequence allows any type ("*") [C-0407].

On success, the API responds with JSON content type and the created order details [C-0400].

### Retrieve transport orders

- Retrieve a single transport order by specifying its name in the path parameter [C-0600].
- Retrieve all transport orders, which are returned sorted by name in ascending order [C-0700].
- Filter the list of transport orders by specifying the `intendedVehicle` query parameter, returning only orders intended for that named vehicle [C-0701].

### Update the intended vehicle

Callers can update the intended vehicle for a transport order using the order's name parameter [C-0500].

- Setting the `vehicle` query parameter assigns the transport order to the named vehicle [C-0501].
- Omitting the `vehicle` query parameter clears the intended vehicle from the order [C-0501].
- The transport order must be in RAW, ACTIVE, or DISPATCHABLE state to update its intended vehicle [C-0504].

### Request immediate assignment

Callers can request immediate assignment of a transport order identified by its name path parameter to its intended vehicle [C-0200].

### Withdraw a transport order

Callers can withdraw a transport order identified by its name path parameter via the dispatcher [C-0300].

- Setting the `immediate` query parameter to true requests an immediate withdrawal, while setting it to false or omitting it requests a non-immediate withdrawal [C-0302].
- Setting the `disableVehicle` query parameter to true updates the assigned processing vehicle's integration level to TO_BE_RESPECTED if the transport order has an assigned processing vehicle [C-0303].

### Trigger order dispatching

Callers can trigger a dispatcher run to assign transport orders [C-0100]. On success, this request returns an empty text/plain; charset=utf-8 response [C-0100].

## Field reference

### Transport order fields

| Field | Description |
|---|---|
| Name | The unique name of the transport order [C-0020]. |
| Dispensable | Flag indicating whether the order is dispensable [C-0020]. |
| Type | The transport order type, defaulting to "-" if omitted during creation [C-0020, C-0403]. |
| State | Lifecycle state of the order: RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, or UNROUTABLE [C-0020, C-0021]. |
| Destinations | List of drive order destinations [C-0020]. |
| Drive orders | List of drive orders associated with the transport order [C-0020]. |
| Current drive order index | Index of the current drive order being executed [C-0020]. |
| Current route step index | Index of the current route step being executed [C-0020]. |
| Creation timestamp | Timestamp when the transport order was created [C-0020]. |
| Deadline timestamp | Due timestamp for completing the transport order [C-0020]. |
| Finished timestamp | Timestamp when the transport order completed [C-0020]. |
| History | List of historical events recorded for the order [C-0019, C-0020]. |
| Dependencies | Transport orders that this order depends upon [C-0020]. |
| Intended vehicle | The vehicle designated to execute the order [C-0020]. |
| Processing vehicle | The vehicle currently executing the order [C-0020]. |
| Peripheral reservation token | Reservation token for peripheral devices [C-0020]. |
| Wrapping sequence | The order sequence wrapping this transport order [C-0020]. |
| Properties | Custom key-value properties attached to the order [C-0020]. |

### Drive orders and destinations

- A drive order representation reports the drive order name, destination details, parent transport order name, planned route, and execution state [C-0016].
- A drive order execution state can be PRISTINE, TRAVELLING, OPERATING, FINISHED, or FAILED [C-0017].
- A destination state describes a drive order's destination by its location name, operation, list of properties, and execution state [C-0009].
- A destination state can be PRISTINE, TRAVELLING, OPERATING, FINISHED, or FAILED [C-0010].
- A drive order destination specifies the destination point or location name, the operation to perform, and a list of properties [C-0015].

### Routes and route steps

- A route contains the total routing costs and an ordered list of route steps [C-0011].
- Each route step specifies the source point, destination point, connecting path, vehicle orientation, route index, costs, whether execution is allowed, and optional rerouting type [C-0012].
- The vehicle orientation indicates whether the vehicle is traveling FORWARD, BACKWARD, or UNDEFINED [C-0013].
- The rerouting type indicates whether rerouting was REGULAR or FORCED [C-0014].

### Properties and history

- A property consists of a required key string and an optional string value [C-0006].
- An object history entry records the event timestamp, event code, and supplemental details [C-0018].

## Errors and what to do

| Symptom | Meaning | What to do |
|---|---|---|
| HTTP 400 Bad Request | Malformed JSON in request body [C-0401]. | Verify that the JSON request payload is properly formatted [C-0401]. |
| HTTP 400 Bad Request | A destination targeting a point specifies an operation other than "MOVE" or "PARK" [C-0405]. | Specify "MOVE" or "PARK" when targeting a point [C-0405]. |
| HTTP 400 Bad Request | A destination targeting a location specifies an operation that is neither "NOP" nor allowed by both the location type and at least one attached link [C-0406]. | Check the location type and link definitions in the plant model to ensure the operation is allowed, or use "NOP" [C-0406]. |
| HTTP 400 Bad Request | The order type does not match the allowed types of the wrapping sequence and the sequence does not allow any type [C-0407]. | Ensure the transport order type matches the allowed types for the wrapping sequence [C-0407]. |
| HTTP 400 Bad Request | Attempted to update the intended vehicle for an order that is not in RAW, ACTIVE, or DISPATCHABLE state [C-0504]. | Update the intended vehicle only while the order is in RAW, ACTIVE, or DISPATCHABLE state [C-0504]. |
| HTTP 400 Bad Request | The request triggers an illegal argument error [C-0001]. | Check request parameters and payload structure against the API requirements [C-0001]. |
| HTTP 404 Not Found | The referenced transport order name does not exist [C-0201, C-0301, C-0502, C-0601]. | Verify that the transport order name exists before requesting it [C-0201, C-0301, C-0502, C-0601]. |
| HTTP 404 Not Found | A destination references a location or point name that does not exist in the plant model [C-0404]. | Ensure all destination point and location names exist in the plant model [C-0404]. |
| HTTP 404 Not Found | The vehicle specified in the query parameter does not exist [C-0503, C-0702]. | Verify the vehicle name against existing vehicles in the system [C-0503, C-0702]. |
| HTTP 404 Not Found | A referenced object is unknown [C-0003]. | Ensure all referenced entities exist [C-0003]. |
| HTTP 409 Conflict | The object being created already exists [C-0004]. | Use a unique name for the entity being created [C-0004]. |
| HTTP 500 Internal Server Error | Serializing the response to JSON failed [C-0402]. | Check server logs for details regarding payload serialization [C-0402]. |
| HTTP 500 Internal Server Error | Kernel execution was unexpectedly interrupted or encountered an unexpected runtime failure [C-0101, C-0102, C-0002, C-0005]. | Retry the request or inspect system state and logs [C-0101, C-0102, C-0002, C-0005]. |
