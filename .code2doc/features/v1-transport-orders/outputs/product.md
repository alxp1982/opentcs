# Transport Orders — Product Requirements

## Summary
Transport orders define tasks and movement sequences dispatched to automated guided vehicles and other carriers [C-0008, C-0020]. Users can submit new transport orders with dedicated destinations, manage assignments to intended vehicles, trigger dispatching runs, withdraw orders, and query order states and execution history [C-0100, C-0200, C-0300, C-0400, C-0500, C-0600, C-0700]. This capability enables host systems to direct automated workflows, monitor vehicle progress through individual drive orders, and intervene dynamically when operational plans change [C-0016, C-0020, C-0300, C-0500].

## What users can do

### Create a transport order
Users create a transport order by providing an order name and a list of destination specifications [C-0008, C-0400]. Each destination requires a destination location name and an operation, along with optional custom properties [C-0007]. Creation requests can optionally supply flags for incomplete names or dispensability, a deadline, an intended vehicle, peripheral reservation tokens, a wrapping order sequence, an order type, custom properties, and order dependencies [C-0008]. If no order type is provided, the system assigns a default type of "-" [C-0403].

The system rejects creation if destination locations or points do not exist in the plant model [C-0404]. Destination operations are validated: point destinations must use either "MOVE" or "PARK", while location destinations must specify "NOP" or an operation permitted by both the location type and at least one attached link [C-0405, C-0406]. If the order belongs to a wrapping sequence, its type must match the sequence's allowed types unless the sequence allows any type ("*") [C-0407].

### Retrieve transport orders
Users can inspect an individual transport order by name or retrieve a list of all transport orders [C-0600, C-0700]. Retrieved lists are sorted by order name in ascending order [C-0700]. Users can filter the list to only include orders intended for a specific vehicle [C-0701]. If the requested transport order or the specified intended vehicle cannot be found, the system rejects the query [C-0601, C-0702].

Responses contain full order details, including dispensability, type, current state, destinations, drive orders, current route step and drive order indices, creation, deadline, and finished timestamps, historical event entries, dependencies, intended and processing vehicles, peripheral reservation tokens, wrapping sequences, and properties [C-0019, C-0020].

### Update the intended vehicle
Users can set or clear the intended vehicle for an existing transport order [C-0500, C-0501]. Providing a vehicle name assigns that vehicle as intended, while omitting it clears the assignment [C-0501]. The request is rejected if the transport order does not exist, if the specified vehicle does not exist, or if the transport order is not in an incomplete, submitted, or dispatchable state [C-0022, C-0502, C-0503, C-0504].

### Assign a transport order immediately
Users can request that the dispatcher immediately assign a transport order to its intended vehicle [C-0200]. The system rejects the request if the referenced transport order does not exist [C-0201].

### Withdraw a transport order
Users can withdraw a transport order via the dispatcher, with an option to execute the withdrawal immediately or non-immediately [C-0300, C-0302]. When requested, the system can also update the assigned processing vehicle's integration level to reserve resources without accepting new orders [C-0024, C-0303]. The request is rejected if the named transport order does not exist [C-0301].

### Trigger dispatching
Users can manually trigger a dispatcher run to assign available transport orders to vehicles [C-0100].

## States and lifecycle
A transport order progresses through the following lifecycle states: [C-0021, C-0022]
- RAW: The order is incomplete [C-0021, C-0022].
- ACTIVE: The order has been submitted [C-0021, C-0022].
- DISPATCHABLE: The order's dependencies are satisfied and it is eligible for assignment [C-0021, C-0022].
- BEING_PROCESSED: The order is assigned to a vehicle and currently executing [C-0021, C-0022].
- WITHDRAWN: Processing was aborted [C-0021, C-0022].
- FINISHED: The order completed successfully [C-0021, C-0022].
- FAILED: The order execution failed [C-0021, C-0022].
- UNROUTABLE: No route could be planned for the order [C-0021, C-0022].

Drive orders and drive order destinations reflect execution through five states: PRISTINE, TRAVELLING, OPERATING, FINISHED, and FAILED [C-0009, C-0010, C-0017].

## Business rules
- A property consists of a required key string and an optional string value [C-0006].
- When defining a destination in a creation request, a destination location name and operation are required, with an optional list of properties [C-0007].
- A destination state tracks the destination's location name, operation, property list, and execution state [C-0009].
- A planned route contains total routing costs and an ordered sequence of route steps [C-0011].
- Each route step defines the source point, destination point, connecting path, vehicle orientation (FORWARD, BACKWARD, or UNDEFINED), route index, costs, execution allowance flag, and optional rerouting type (REGULAR or FORCED) [C-0012, C-0013, C-0014].
- A drive order destination specifies the destination point or location name, the operation to perform, and properties [C-0015].
- Object history entries record an event timestamp, an event code, and supplemental details [C-0018].
- Kernel object events classify modifications as created, modified, or removed [C-0023].
- A vehicle's integration level controls kernel participation: ignored (position ignored), noticed (position tracked without resource reservation), respected (resources reserved), or utilized (order assignment allowed) [C-0024].

## Glossary
- **Destination State**: A description of a drive order destination, including its location name, operation, properties, and execution state [C-0009].
- **Drive Order**: A component of a transport order reporting the drive order name, destination details, parent transport order name, planned route, and execution state [C-0016].
- **Integration Level**: The level defining a vehicle's participation in kernel operations, ranging from ignored, noticed, respected, to utilized [C-0024].
- **Object History**: An ordered record containing historical event entries for an object [C-0018, C-0019].
- **Property**: A key-value pair containing a required key string and an optional value string [C-0006].
- **Route**: A path execution plan containing total routing costs and an ordered list of route steps [C-0011].
- **Route Step**: A movement segment within a route defining source and destination points, connecting paths, orientation, costs, execution permissions, and rerouting type [C-0012].
- **Transport Order**: An order composed of destinations, drive orders, dependencies, and execution constraints assigned to vehicles [C-0008, C-0020].

## Requirements
- TO-A1: The system shall create a transport order with a specified name and destination list [C-0008, C-0400].
- TO-A2: The system shall accept optional creation parameters including incomplete name flags, dispensability flags, deadlines, intended vehicles, peripheral reservation tokens, wrapping sequences, types, properties, and order dependencies [C-0008].
- TO-A3: The system shall default the transport order type to "-" when omitted [C-0403].
- TO-A4: The system shall reject creation if a destination references a nonexistent location or point in the plant model [C-0404].
- TO-A5: The system shall reject creation if a point destination specifies an operation other than "MOVE" or "PARK" [C-0405].
- TO-A6: The system shall reject creation if a location destination specifies an operation that is neither "NOP" nor allowed by the location type and at least one attached link [C-0406].
- TO-A7: The system shall reject creation if adding an order to a wrapping sequence whose allowed types do not include the order type and do not permit any type [C-0407].
- TO-B1: The system shall retrieve an individual transport order by its name [C-0600].
- TO-B2: The system shall retrieve all transport orders sorted by name in ascending order [C-0700].
- TO-B3: The system shall filter retrieved transport orders by the specified intended vehicle [C-0701].
- TO-B4: The system shall reject order retrieval requests when the transport order or intended vehicle does not exist [C-0601, C-0702].
- TO-C1: The system shall update the intended vehicle of a transport order to the specified vehicle [C-0500, C-0501].
- TO-C2: The system shall clear the intended vehicle of a transport order when no vehicle is specified [C-0501].
- TO-C3: The system shall reject intended vehicle updates if the order does not exist, the vehicle does not exist, or the order is not in RAW, ACTIVE, or DISPATCHABLE state [C-0502, C-0503, C-0504].
- TO-D1: The system shall request immediate assignment of a transport order to its intended vehicle [C-0200].
- TO-D2: The system shall reject immediate assignment requests when the transport order does not exist [C-0201].
- TO-E1: The system shall withdraw a transport order via the dispatcher [C-0300].
- TO-E2: The system shall support immediate and non-immediate withdrawal requests [C-0302].
- TO-E3: The system shall update the assigned processing vehicle's integration level to TO_BE_RESPECTED when vehicle disabling is requested during withdrawal [C-0024, C-0303].
- TO-E4: The system shall reject withdrawal requests when the transport order does not exist [C-0301].
- TO-F1: The system shall trigger a dispatcher run to assign transport orders [C-0100].

## Open items
- Published docs state that a property value is required, but the code allows the property value to be optional [C-0006].
- Published docs state that destination states include ACTIVE, whereas the code defines destination states as PRISTINE, TRAVELLING, OPERATING, FINISHED, and FAILED [C-0010].

## Appendix: interface reference

| Operation | Request | Inputs | Success Result | Failure Cases | Claims |
|---|---|---|---|---|---|
| Trigger dispatcher | POST /v1/transportOrders | None | 200 OK with empty text/plain; charset=utf-8 body | 500 when kernel execution is interrupted or throws wrapped runtime exceptions | [C-0100, C-0101, C-0102] |
| Request immediate assignment | POST /v1/transportOrders/{NAME}/immediateAssignment | Path: NAME | 200 OK | 404 if order does not exist; 400 for illegal argument; 500 for state or runtime errors | [C-0001, C-0002, C-0003, C-0005, C-0200, C-0201] |
| Withdraw transport order | POST /v1/transportOrders/{NAME}/withdrawal | Path: NAME; Query: immediate (boolean), disableVehicle (boolean) | 200 OK | 404 if order does not exist; 400 for illegal argument; 500 for state or runtime errors | [C-0001, C-0002, C-0003, C-0005, C-0300, C-0301, C-0302, C-0303] |
| Create transport order | POST /v1/transportOrders/{NAME} | Path: NAME; Body: JSON destination list and optional order fields | 200 OK with created order JSON | 400 for malformed JSON, invalid point/location operations, or sequence type mismatch; 404 if destination point/location does not exist; 409 if order exists; 500 on serialization failure or runtime error | [C-0001, C-0002, C-0003, C-0004, C-0005, C-0008, C-0400, C-0401, C-0402, C-0403, C-0404, C-0405, C-0406, C-0407] |
| Update intended vehicle | PUT /v1/transportOrders/{NAME}/intendedVehicle | Path: NAME; Query: vehicle (optional string) | 200 OK | 400 if order not in RAW, ACTIVE, or DISPATCHABLE state; 404 if order or vehicle does not exist; 500 for state or runtime errors | [C-0001, C-0002, C-0003, C-0005, C-0500, C-0501, C-0502, C-0503, C-0504] |
| Get transport order | GET /v1/transportOrders/{NAME} | Path: NAME | 200 OK with order JSON | 404 if order does not exist; 400 for illegal argument; 500 for state or runtime errors | [C-0001, C-0002, C-0003, C-0005, C-0020, C-0600, C-0601] |
| List transport orders | GET /v1/transportOrders | Query: intendedVehicle (optional string) | 200 OK with list of order JSON sorted ascending by name | 404 if intended vehicle does not exist; 400 for illegal argument; 500 for state or runtime errors | [C-0001, C-0002, C-0003, C-0005, C-0020, C-0700, C-0701, C-0702] |
