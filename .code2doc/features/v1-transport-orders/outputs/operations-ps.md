# Transport Orders — Configuration and Operations Guide

## What this feature does

This feature manages the creation, assignment, withdrawal, and querying of transport orders within the plant model [C-0020, C-0022, C-0400]. Host systems define transport destinations and parameters, track order lifecycles and historical events, and assign routes to automated vehicles [C-0008, C-0016, C-0018, C-0022]. External dispatching workflows can trigger assignment runs, request immediate assignments to designated vehicles, or withdraw orders during execution [C-0100, C-0200, C-0300].

## Settings and defaults

| Setting / input | Default | Effect | Notes |
|---|---|---|---|
| `type` | `"-"` | Sets the transport order category [C-0403]. | Represents no specific type when omitted [C-0403]. |
| `immediate` | None / not specified | Sets immediate withdrawal when `true`, or non-immediate withdrawal otherwise [C-0302]. | Query parameter when withdrawing an order [C-0302]. |
| `vehicle` | Omitted | Sets the intended vehicle to the named vehicle, or clears the intended vehicle when omitted [C-0501]. | Query parameter when updating an order's intended vehicle [C-0501]. |

## Operating procedures

### 1. Create a transport order

1. Prepare a JSON payload containing at least a destination list [C-0008]. Each destination requires a destination location name and operation, and accepts an optional list of properties where each property consists of a required key and optional value [C-0006, C-0007].
2. Optionally supply additional configuration fields including dispensability, deadline, intended vehicle, peripheral reservation token, wrapping sequence, incomplete name flag, order type, order properties, and dependencies [C-0008].
3. Ensure every destination targeting a point references an existing point in the plant model and specifies either a `MOVE` or `PARK` operation [C-0404, C-0405].
4. Ensure every destination targeting a location references an existing location in the plant model and specifies an operation that is either `NOP` or allowed by both the location type and at least one attached link [C-0404, C-0406].
5. If assigning the order to a wrapping sequence, verify that the order type matches the allowed sequence types or that the sequence permits any type (`*`) [C-0407].
6. Submit the creation request with the order name [C-0400]. A successful request returns the created transport order details as JSON [C-0400].

### 2. Update an intended vehicle

1. Identify the target transport order by name [C-0500]. Verify that the order is currently in `RAW`, `ACTIVE`, or `DISPATCHABLE` state [C-0504].
2. To assign an intended vehicle, submit the update request with the target vehicle name [C-0501]. Ensure the specified vehicle exists in the model [C-0503].
3. To clear an existing intended vehicle assignment, submit the update request omitting the vehicle name [C-0501].

### 3. Trigger immediate vehicle assignment

1. Identify the transport order by name [C-0200].
2. Ensure the order exists in the model [C-0201].
3. Submit the immediate assignment request to route the order directly to its designated vehicle [C-0200].

### 4. Trigger a general dispatcher run

1. Submit a dispatcher trigger request [C-0100].
2. The dispatcher evaluates assignments across the plant model and responds with an empty success confirmation [C-0100].

### 5. Withdraw a transport order

1. Identify the transport order by name [C-0300]. Ensure the order exists in the model [C-0301].
2. Choose whether to perform an immediate or non-immediate withdrawal [C-0302].
3. Choose whether to disable the assigned processing vehicle: if requested and a processing vehicle is assigned, the vehicle's integration level updates to `TO_BE_RESPECTED`, reserving resources without allowing new order assignments [C-0024, C-0303].
4. Submit the withdrawal request via the dispatcher [C-0300].

### 6. Query transport orders

1. To retrieve a single transport order, query by the order name [C-0600]. The response details the order name, dispensability flag, type, state, destinations, drive orders, current drive order and route step indices, creation, deadline, and completion timestamps, history, dependencies, vehicle assignments, peripheral reservation tokens, wrapping sequence, and properties [C-0020].
2. To retrieve all orders, query the collection [C-0700]. Results are returned sorted by name in ascending order [C-0700].
3. To filter orders by intended vehicle, provide the vehicle name parameter [C-0701]. Ensure the vehicle exists in the model [C-0702].

## Interactions and gotchas

- **Withdrawal vehicle disabling**: When withdrawing an order with the vehicle disabling flag set to true and a processing vehicle is assigned, the vehicle's integration level updates to `TO_BE_RESPECTED` [C-0303]. At this level, the vehicle reserves resources without allowing order assignment [C-0024].
- **Intended vehicle update timing**: Updating or clearing an intended vehicle is only permitted while an order is in `RAW`, `ACTIVE`, or `DISPATCHABLE` state [C-0504]. Once an order enters `BEING_PROCESSED`, `WITHDRAWN`, `FINISHED`, `FAILED`, or `UNROUTABLE`, changes are rejected [C-0021, C-0504].
- **Wrapping sequence order types**: Orders attached to a wrapping sequence must match the sequence's permitted order types unless the sequence explicitly allows any type (`*`) [C-0407].
- **Point versus location operations**: Operations on destination points are restricted to `MOVE` and `PARK` [C-0405]. Operations on locations must be `NOP` or must be explicitly allowed by both the destination's location type and at least one attached link [C-0406].

## Troubleshooting

| Symptom | Cause | Resolution |
|---|---|---|
| HTTP 400 Bad Request on order creation | Malformed JSON in the request body [C-0401]. | Validate JSON syntax before resubmitting the request [C-0401]. |
| HTTP 400 Bad Request on order creation | A point destination specified an operation other than `MOVE` or `PARK` [C-0405]. | Change the point destination operation to `MOVE` or `PARK` [C-0405]. |
| HTTP 400 Bad Request on order creation | A location destination specified an operation that is neither `NOP` nor allowed by both the location type and at least one attached link [C-0406]. | Configure an operation permitted by both the location type and attached link, or use `NOP` [C-0406]. |
| HTTP 400 Bad Request on order creation | An order type does not match the wrapping sequence's allowed types and the sequence does not allow any type (`*`) [C-0407]. | Align the order type with sequence rules or allow any type on the sequence [C-0407]. |
| HTTP 400 Bad Request on vehicle update | The order is not in `RAW`, `ACTIVE`, or `DISPATCHABLE` state [C-0504]. | Verify the order status before attempting to reassign or clear the intended vehicle [C-0504]. |
| HTTP 400 Bad Request | An invalid argument was passed in the request [C-0001]. | Correct the invalid parameter in the request payload or URL [C-0001]. |
| HTTP 404 Not Found on order creation | A destination references a location or point name that does not exist in the plant model [C-0404]. | Verify that all referenced point and location names exist in the plant model [C-0404]. |
| HTTP 404 Not Found on order retrieval, update, assignment, or withdrawal | No transport order exists with the specified name [C-0201, C-0301, C-0502, C-0601]. | Verify the transport order name against existing orders [C-0201, C-0301, C-0502, C-0601]. |
| HTTP 404 Not Found on vehicle update or order query | The vehicle specified in the query parameter does not exist [C-0503, C-0702]. | Verify that the vehicle name exists in the plant model [C-0503, C-0702]. |
| HTTP 404 Not Found | A referenced kernel model object does not exist [C-0003]. | Ensure all referenced model entities exist prior to issuing the request [C-0003]. |
| HTTP 409 Conflict | The object being created already exists [C-0004]. | Supply a unique name for the new object [C-0004]. |
| HTTP 500 Internal Server Error on order creation | Response serialization to JSON failed [C-0402]. | Check server logs for serialization errors [C-0402]. |
| HTTP 500 Internal Server Error during dispatcher or order requests | Kernel execution was unexpectedly interrupted, encountered an illegal state, or experienced an underlying execution failure wrapped as a kernel runtime exception [C-0002, C-0005, C-0101, C-0102]. | Inspect kernel operational logs to diagnose runtime failures or thread interruptions [C-0002, C-0005, C-0101, C-0102]. |

## Known differences from published docs

- Code defines `Property.value` as optional [C-0006]. Published docs state that `Property.value` is required [C-0006].
- Code allows destination states to be `PRISTINE`, `TRAVELLING`, `OPERATING`, `FINISHED`, or `FAILED` [C-0010]. Published docs state that `DestinationState` includes `ACTIVE` in addition to `PRISTINE`, `TRAVELLING`, `OPERATING`, `FINISHED`, and `FAILED` [C-0010].
- When updating an intended vehicle, the API responds with HTTP 400 if the order is not in `RAW`, `ACTIVE`, or `DISPATCHABLE` state [C-0504]. Published docs do not specify this state restriction or the HTTP 400 response [C-0504].
- When filtering the transport orders collection by `intendedVehicle`, the API responds with HTTP 404 if the vehicle does not exist [C-0702]. Published docs do not state that an unknown vehicle parameter returns HTTP 404 [C-0702].
- The transport orders collection returns orders sorted by name in ascending order [C-0700]. Published docs do not document this ordering behavior [C-0700].
- An omitted transport order type defaults to `"-"` [C-0403]. Published docs do not document this default value [C-0403].
- Requesting immediate assignment returns HTTP 404 if the transport order does not exist [C-0201]. Published docs do not document this HTTP 404 response [C-0201].
- Requesting an order withdrawal returns HTTP 404 if the transport order does not exist [C-0301]. Published docs do not document this HTTP 404 response [C-0301].
- Retrieving an unknown transport order returns HTTP 404 [C-0601]. Published docs do not document this HTTP 404 response [C-0601].
- Submitting malformed JSON on transport order creation returns HTTP 400 [C-0401]. Published docs do not document this HTTP 400 response [C-0401].
- Specifying an unknown destination point or location on order creation returns HTTP 404 [C-0404]. Published docs do not document this HTTP 404 response [C-0404].
- Specifying an invalid point operation (not `MOVE` or `PARK`) or an invalid location operation returns HTTP 400 [C-0405, C-0406]. Published docs do not document these HTTP 400 responses [C-0405, C-0406].
- Adding an order to a wrapping sequence with a mismatched order type returns HTTP 400 [C-0407]. Published docs do not document this HTTP 400 response [C-0407].
- Updating an intended vehicle returns HTTP 404 if either the transport order or the vehicle does not exist [C-0502, C-0503]. Published docs do not document these HTTP 404 responses [C-0502, C-0503].
- Internal runtime exceptions such as `IllegalStateException` and `KernelRuntimeException` are mapped globally to HTTP 500 [C-0002, C-0005]. Published docs do not document these global exception mappings [C-0002, C-0005].
