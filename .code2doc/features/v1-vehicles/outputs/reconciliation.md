# Reconciliation: v1-vehicles — code vs. published documentation

| Verdict | Count |
|---|---|
| CONTRADICTION | 5 |
| UNDOCUMENTED | 44 |
| DOC_ONLY | 4 |
| AMBIGUOUS | 3 |
| CONFIRMED | 346 |

## Contradiction

- **C-0014** — code: A vehicle's processing state is IDLE when no order is assigned, AWAITING_ORDER when assigned to a transport order but awaiting the next drive order, or PROCESSING_ORDER when actively executing a drive order. (`Vehicle.java:1420-1435`)  
  doc (vehicle.yaml:164): vehicle:TimestampedVehicleProcState.state is one of: IDLE, AWAITING_ORDER, PROCESSING_ORDER.  
  _Claim states procState has IDLE, AWAITING_ORDER, PROCESSING_ORDER while v1 schema (D-0928, D-1606) includes UNAVAILABLE as a processing state ('UNAVAILABLE, IDLE, AWAITING_ORDER, PROCESSING_ORDER')._
- **C-0029** — code: Posting a message to a vehicle communication adapter accepts a message type and a list of key-value parameters. (`PostVehicleCommAdapterMessageRequestTO.java:13-26`)  
  doc (vehicles.yaml:959): vehicles:VehicleCommAdapterMessage.parameters; required; type array. The message's parameters.  
  _Web API v1 specifies VehicleCommAdapterMessage.parameters as an array (D-1644: 'type array'), whereas Web API v8 / the claim specifies it as a map / object of key-value parameters (D-2206: 'type object')._
- **C-0303** — code: When a communication adapter is attached to a vehicle, the vehicle property tcs:preferredAdapter is updated to the adapter factory class name. (`AttachmentManager.java:206-210`)  
  doc (users-guide-02_howtos.adoc:38): How-to Guides › Creating curved paths between points › Automatically selecting a specific vehicle driver on startup: In some less common cases, you may have multiple vehicle drivers registered with the kernel that can all attach to the vehicles in your plant model. To automatically select a specific  
  _Doc D-0328 states property key is 'tcs:preferredAdapterClass', whereas code sets vehicle property 'tcs:preferredAdapter'._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (plant-model.yaml:957): plant-model:PlantModelPath.peripheralOperations[].executionTrigger is one of: AFTER_ALLOCATION, AFTER_MOVEMENT.  
  _Doc states executionTrigger is one of AFTER_ALLOCATION, AFTER_MOVEMENT; code C-0018 also includes IMMEDIATE._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (plant-model.yaml:497): plant-model:PlantModelPath.peripheralOperations[].executionTrigger is one of: AFTER_ALLOCATION, AFTER_MOVEMENT.  
  _Spec lists executionTrigger as one of AFTER_ALLOCATION, AFTER_MOVEMENT; code supports IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT (C-0018)._

## Doc Only

- **D-0729**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:340): Runtime behaviour › Topics › Topic `instantActions`: The vehicle is expected to confirm that it has received an instant action by listing it in the `actionStates` array of its state message. In case the vehicle rejects an instant action, it must still reflect that instant action in its `actionStates  
  _Describes VDA 5050 protocol expectation for vehicle behaviour regarding instant actions and actionStates; not covered by any verified kernel API claims._
- **D-0753**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:36): Runtime behaviour › Topics › Topic `connection`: Upon receiving a connection state of `ONLINE`, the driver will report the vehicle's state as `IDLE` to the kernel.  
  _Describes driver behavior reporting vehicle IDLE state to the kernel upon receiving ONLINE connection state; not covered by verified API claims._
- **D-0776**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:100): Runtime behaviour › Topics › Topic `state` › Mapping of state information to vehicle properties: The property with the key `vda5050:information.info` will contain a concatenated list of ``infoType``s of all `info` elements with an `infoLevel` of `INFO`.  
  _Describes VDA 5050 driver state mapping of info elements to property vda5050:information.info; not covered by verified API claims._
- **D-0814**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:337): Runtime behaviour › Topics › Topic `instantActions`: The vehicle is expected to confirm that it has received an instant action by listing it in the `actionStates` array of its state message. In case the vehicle rejects an instant action, it must still reflect that instant action in its `actionStates  
  _Specifies VDA 5050 vehicle behavior for instantActions acknowledgement and FAILED status; not covered by any verified claims._

## Undocumented

- **C-0002** — code: When an operation encounters an IllegalStateException, the Web API responds with HTTP 500 Internal Server Error and serializes the exception. (`ServiceWebApi.java:180-185`)  
  _Web API mapping of IllegalStateException to HTTP 500 Internal Server Error is not explicitly documented in the API specification._
- **C-0005** — code: When a KernelRuntimeException is raised during processing, the Web API responds with HTTP 500 Internal Server Error. (`ServiceWebApi.java:197-200`)  
  _Web API mapping of KernelRuntimeException to HTTP 500 Internal Server Error is not documented._
- **C-0020** — code: The openTCS kernel operates in MODELLING state when topology is defined, OPERATING state during active fleet dispatching, or SHUTDOWN while terminating. (`Kernel.java:44-59`)  
  _The kernel states MODELLING, OPERATING, and SHUTDOWN are not covered by any candidate statement._
- **C-0021** — code: Model object events communicate lifecycle modifications as OBJECT_CREATED, OBJECT_MODIFIED, or OBJECT_REMOVED. (`TCSObjectEvent.java:120-135`)  
  _Model object event types OBJECT_CREATED, OBJECT_MODIFIED, and OBJECT_REMOVED are not documented in the provided statements._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  _The endpoint/feature for calculating a vehicle route (returning detailed route steps with source, path, destination, orientation, and total costs avoiding resources) is not documented in the provided candidate references._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  _No candidate statement covers the full retrieval payload/state attributes returned when retrieving a vehicle via the web API._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  _No candidate statement documents vehicle SSE events delivering previous and current states with pose, bounding box, velocity limits, and layout colors._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  _No candidate documentation specifies peripheral job SSE event streams including completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  _None of the documentation candidates document peripheral job status message specifications regarding timestamps, related vehicle/order names, reservation tokens, and completion requirements._
- **C-0203** — code: Responds with HTTP status 400 when the request body cannot be parsed as JSON. (`JsonBinder.java:46-48`)  
  _Standard HTTP 400 response for malformed JSON request body is not explicitly listed in the routeComputationQuery API documentation._
- **C-0204** — code: Responds with HTTP status 400 if the query parameter maxRoutesPerDestinationPoint is not a valid integer. (`V1RequestHandler.java:784-789`)  
  _HTTP 400 response for invalid integer format on maxRoutesPerDestinationPoint is not documented in the API specification._
- **C-0205** — code: Responds with HTTP status 404 if the vehicle named in the path does not exist. (`VehicleHandler.java:295-296`)  
  _HTTP 404 response when vehicle NAME does not exist is not documented for the routeComputationQuery endpoint._
- **C-0206** — code: Defaults the source point to the vehicle's current position if no sourcePoint is provided in the request body. (`VehicleHandler.java:299-304`)  
  _Documentation does not mention defaulting sourcePoint to vehicle's current position when sourcePoint is omitted in compute-path requests._
- **C-0207** — code: Responds with HTTP status 400 if no sourcePoint is specified and the vehicle has no known current position. (`VehicleHandler.java:299-302`)  
  _Documentation does not specify the HTTP 400 response when no sourcePoint is given and vehicle current position is unknown._
- **C-0208** — code: Responds with HTTP status 404 if the specified sourcePoint does not exist in the plant model. (`VehicleHandler.java:306-311`)  
  _Documentation does not specify HTTP 404 response when sourcePoint does not exist in plant model._
- **C-0209** — code: Responds with HTTP status 404 if any specified destination point does not exist in the plant model. (`VehicleHandler.java:315-322`)  
  _Documentation does not specify HTTP 404 response when a destination point does not exist in the plant model._
- **C-0210** — code: Responds with HTTP status 404 if any resource name in resourcesToAvoid does not correspond to an existing point, path, or location. (`VehicleHandler.java:328-349`)  
  _Documentation does not specify HTTP 404 response when any resource in resourcesToAvoid does not exist._
- **C-0301** — code: Returns HTTP 404 (Not Found) if no vehicle with the specified name exists. (`VehicleHandler.java:197-198`)  
  _Candidates do not document HTTP 404 response when vehicle NAME is not found._
- **C-0302** — code: Returns HTTP 400 (Bad Request) if the driver class specified by newValue is not among the available communication adapters for the vehicle. (`VehicleHandler.java:204-208`)  
  _Candidates do not document HTTP 400 response when specified driver class is not among available adapters._
- **C-0401** — code: Responses for vehicle communication adapter attachment information are formatted as JSON with UTF-8 encoding. (`V1RequestHandler.java:310-310`)  
  _Responses being formatted as JSON with UTF-8 encoding for comm adapter attachment info is not documented in the candidate statements._
- **C-0402** — code: Returns HTTP 404 with error details if the specified vehicle does not exist. (`VehicleHandler.java:184-185`)  
  _Returning HTTP 404 when the vehicle does not exist on this endpoint is not mentioned in the candidate statements._
- **C-0501** — code: Responds with HTTP status 404 if no vehicle with the specified name exists. (`VehicleHandler.java:167-168`)  
  _None of the candidate statements describe returning HTTP 404 if no vehicle with NAME exists for PUT /vehicles/{NAME}/commAdapter/enabled._
- **C-0502** — code: Responds with HTTP status 400 when disabling the communication adapter if no vehicle entry is found for the vehicle. (`StandardVehicleService.java:273-275`)  
  _Returning HTTP 400 when disabling the comm adapter if no vehicle entry is found is not covered by any candidate statement._
- **C-0503** — code: Responds with HTTP status 400 when enabling the communication adapter if no vehicle entry is found for the vehicle. (`StandardVehicleService.java:288-290`)  
  _Returning HTTP 400 when enabling the comm adapter if no vehicle entry is found is not covered by any candidate statement._
- **C-0601** — code: Returns HTTP 404 if no vehicle with the given NAME exists. (`VehicleHandler.java:222-223`)  
  _Candidate statements do not document HTTP 404 when no vehicle with NAME exists on POST commAdapter/message._
- **C-0701** — code: Returns HTTP 404 if no vehicle with the given NAME exists. (`VehicleHandler.java:140-141`)  
  _Returning HTTP 404 if no vehicle with the given NAME exists is not documented in candidate statements for PUT paused._
- **C-0801** — code: Returns HTTP 404 when no vehicle exists with the specified NAME. (`VehicleHandler.java:123-124`)  
  _Returning HTTP 404 when no vehicle exists with the specified NAME is not documented in the candidate statements for PUT integrationLevel._
- **C-0802** — code: Returns HTTP 400 when attempting to set newValue to TO_BE_IGNORED or TO_BE_NOTICED while the vehicle is currently processing a transport order. (`StandardVehicleService.java:350-360`)  
  _Documentation does not mention returning HTTP 400 when attempting to set integration level newValue to TO_BE_IGNORED or TO_BE_NOTICED while the vehicle is currently processing a transport order._
- **C-0902** — code: When the query parameter disableVehicle is set to true, the vehicle's integration level is updated to TO_BE_RESPECTED upon withdrawal. (`V1RequestHandler.java:441-441`)  
  _D-1521 describes disableVehicle as deprecated and suggests explicitly setting integration level instead, but does not document that setting disableVehicle to true updates the vehicle's integration level to TO_BE_RESPECTED upon withdrawal._
- **C-0903** — code: If no vehicle exists with the specified name, the request fails with HTTP status 404 and an error message indicating an unknown vehicle. (`TransportOrderDispatcherHandler.java:83-84`)  
  _None of the candidates cover the 404 response or unknown vehicle error behavior for this endpoint._
- **C-1002** — code: If no vehicle with the given NAME exists, the request fails with HTTP status 404 (ObjectUnknownException: Unknown vehicle: {NAME}). (`TransportOrderDispatcherHandler.java:102-103`)  
  _None of the candidate statements describe the 404 error response (ObjectUnknownException) for POST /vehicles/{NAME}/rerouteRequest._
- **C-1003** — code: If a forced rerouting is executed but the vehicle's current position is unknown, the request fails with HTTP status 400. (`ServiceWebApi.java:172-193`)  
  _None of the candidates document that forced rerouting returns HTTP status 400 if the vehicle's current position is unknown._
- **C-1004** — code: If a forced rerouting is executed but resources for the vehicle's current position cannot be allocated at that moment, the request fails with HTTP status 400. (`ServiceWebApi.java:175-178`)  
  _None of the candidates document that forced rerouting returns HTTP status 400 when resources for the vehicle's current position cannot be allocated._
- **C-1005** — code: If allocating resources for the vehicle's current position during forced rerouting throws a ResourceAllocationException, the request fails with HTTP status 400. (`ServiceWebApi.java:175-178`)  
  _None of the candidates document that ResourceAllocationException during forced rerouting results in HTTP 400._
- **C-1100** — code: Updates the set of acceptable order types for the vehicle identified by NAME with priority 0 for each specified order type, returning an empty plain text response. (`V1RequestHandler.java:185-185`)  
  _While changelog entries D-0033 and D-0026 mention PUT /vehicles/{NAME}/acceptableOrderTypes, no candidate documents updating the acceptable order types with priority 0 for each specified type and returning an empty plain text response._
- **C-1101** — code: Returns HTTP status 404 if no vehicle with the specified NAME exists. (`VehicleHandler.java:244-245`)  
  _None of the candidates document returning HTTP 404 when the vehicle specified by NAME does not exist for this endpoint._
- **C-1201** — code: Returns HTTP 404 if no vehicle with the given NAME exists. (`VehicleHandler.java:244-245`)  
  _The OpenAPI/Web API spec candidates do not mention returning HTTP 404 when no vehicle exists with the given NAME for this endpoint._
- **C-1301** — code: Rejects the request with HTTP status 400 Bad Request if any of energyLevelCritical, energyLevelGood, energyLevelSufficientlyRecharged, or energyLevelFullyRecharged is not between 0 and 100 inclusive. (`Vehicle.java:1537-1560`)  
  _Documentation does not mention HTTP 404/400 validation error on threshold values outside 0-100._
- **C-1302** — code: Returns HTTP status 404 Not Found if no vehicle exists with the specified NAME. (`VehicleHandler.java:270-271`)  
  _Documentation candidates for this endpoint do not document HTTP 404 response on unknown vehicle NAME._
- **C-1401** — code: Responds with HTTP status 404 if no vehicle with the specified NAME exists. (`VehicleHandler.java:153-154`)  
  _No statement documents HTTP status 404 response when vehicle with NAME does not exist for this endpoint._
- **C-1402** — code: Responds with HTTP status 400 if updating the envelope key is attempted while the vehicle is processing an order, claiming resources, or allocating resources. (`StandardVehicleService.java:417-424`)  
  _Returning HTTP 400 when vehicle is processing an order or claiming/allocating resources is not stated in documentation._
- **C-1501** — code: Returns HTTP 404 if no vehicle with the specified name exists. (`VehicleHandler.java:112-112`)  
  _No candidate statement specifies the 404 Not Found response when vehicle is not found for GET /vehicles/{NAME}._
- **C-1600** — code: GET /v1/vehicles retrieves a list of all vehicles and their states, sorted in ascending order by vehicle name. (`V1RequestHandler.java:196-196`)  
  _Sorting vehicles in ascending order by vehicle name in GET /v1/vehicles response is not specified in the documentation candidates._
- **C-1603** — code: If procState is not a valid vehicle processing state name, the request fails with HTTP 400 Bad Request. (`VehicleHandler.java:86-88`)  
  _The OpenAPI spec mentions query parameter procState but does not document that an invalid enum/state name returns HTTP 400 Bad Request._

## Ambiguous

- **C-0028** — code: The vehicle attachment info response reports the vehicle name, the currently attached communication adapter, and the list of available communication adapters. (`GetVehicleAttachmentInfoResponseTO.java:10-23`)  
  doc (vehicles.yaml:826): vehicles:AttachmentInformation.attachedCommAdapter; required; type string. The description class name of the vehicle driver currently attached to this vehicle.  
  _Candidate D-1630 documents attachedCommAdapter, but does not explicitly document the full AttachmentInformation schema containing vehicle name, attachedCommAdapter, and availableCommAdapters in a single statement._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (changelog.adoc:744): openTCS: Change Log › Version 5.4 (2022-02-25): Add claimed and allocated resources to the vehicle state and vehicle status message specification.  
  _D-0125 notes that claimed and allocated resources were added to the vehicle status message specification, but candidates do not detail the full broadcast message content._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (transport-orders.yaml:293): transport-orders:TransportOrderState.currentRouteStepIndex; required; type integer. The current route step index of the transport order.  
  _Documentation candidates describe TransportOrderState fields in the Web API (currentRouteStepIndex, state, currentDriveOrderIndex), but do not explicitly specify the SSE event streaming payload/behavior for transport orders and drive orders._

## Confirmed

- **C-0001** — code: When an operation encounters an IllegalArgumentException, the Web API responds with HTTP 400 Bad Request and serializes the exception. (`ServiceWebApi.java:175-178`)  
  doc (vehicles.yaml:524): POST /vehicles/{NAME}/routeComputationQuery returns 400: The submitted request body is invalid. (example: Could not parse JSON input.)  
  _Documentation confirms that invalid request bodies or parameters triggering IllegalArgumentException result in HTTP 400 Bad Request._
- **C-0003** — code: When a referenced object is not found, throwing an ObjectUnknownException, the Web API responds with HTTP 404 Not Found. (`ServiceWebApi.java:187-190`)  
  doc (order-sequences.yaml:61): GET /orderSequences/{NAME} returns 404: Referencing object that could not be found. (example: Could not find order sequence 'Sequence-002'.)  
  _Both code and documentation agree that ObjectUnknownException maps to HTTP 404 Not Found when a referenced entity cannot be found._
- **C-0004** — code: When attempting to create or modify an entity whose identifier already exists, throwing an ObjectExistsException, the Web API responds with HTTP 409 Conflict. (`ServiceWebApi.java:192-195`)  
  doc (order-sequences.yaml:103): POST /orderSequences/{NAME} returns 409: An object with the same name already exists in the model. (example: Order sequence 'Sequence-002' already exists.)  
  _Both code and documentation agree that ObjectExistsException maps to HTTP 409 Conflict when an object with the same name already exists._
- **C-0006** — code: A Block type can be SINGLE_VEHICLE_ONLY, restricting resource usage in the block to one vehicle at a time, or SAME_DIRECTION_ONLY, permitting multiple vehicles only if they travel through the block in the same direction. (`Block.java:204-215`)  
  doc (users-guide-03_reference.adoc:283): Reference › Supported navigation principles / vehicle types › Plant model elements › Block: _Same direction only_: The resources aggregated in this block can be used by multiple vehicles at the same time, but only if they traverse the block in the same direction.  
  _Both documentation and plant model schema confirm Block type can be SINGLE_VEHICLE_ONLY or SAME_DIRECTION_ONLY with the described resource usage semantics._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (developers-guide-02_howtos.adoc:40): How-to Guides › Acquiring service objects › Working with transport orders › How to create a transport order: Create a list of destinations the vehicle is supposed to travel to. Every destination is described by the name of the destination location in the plant model and an operation the vehicle is s  
  _Documentation confirms transport order destinations specify target location, operation, and related properties._
- **C-0010** — code: A drive order and its mapped destination state can be PRISTINE (unprocessed), TRAVELLING (vehicle moving towards destination), OPERATING (executing an operation), FINISHED (successfully completed), or FAILED. (`DriveOrder.java:347-369`)  
  doc (transport-order.yaml:171): transport-order:DriveOrderState is one of: PRISTINE, TRAVELLING, OPERATING, FINISHED, FAILED.  
  _Documentation schema confirms DriveOrderState / destination states: PRISTINE, TRAVELLING, OPERATING, FINISHED, FAILED._
- **C-0011** — code: An environmental entity integration level indicates whether the entity is ignored (TO_BE_IGNORED), displayed without impacting traffic management (TO_BE_NOTICED), or accounted for during traffic management (TO_BE_RESPECTED). (`EnvironmentalEntity.java:458-471`)  
  doc (environmental-entities.yaml:259): environmental-entities:EnvironmentalEntity.integrationLevel is one of: TO_BE_IGNORED, TO_BE_NOTICED, TO_BE_RESPECTED. The entity's integration level.  
  _Documentation schema confirms environmental entity integrationLevel values: TO_BE_IGNORED, TO_BE_NOTICED, TO_BE_RESPECTED._
- **C-0012** — code: A vehicle integration level defines whether the vehicle is ignored (TO_BE_IGNORED), noticed without reserving resources (TO_BE_NOTICED), respected by reserving resources for reported positions (TO_BE_RESPECTED), or utilized by the dispatcher for order assignments (TO_BE_UTILIZED). (`Vehicle.java:1396-1415`)  
  doc (vehicles.yaml:635): vehicles:VehicleState.integrationLevel is one of: TO_BE_IGNORED, TO_BE_NOTICED, TO_BE_RESPECTED, TO_BE_UTILIZED. The vehicle's integration level.  
  _Documentation schema confirms vehicle integrationLevel values: TO_BE_IGNORED, TO_BE_NOTICED, TO_BE_RESPECTED, TO_BE_UTILIZED._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vehicles.yaml:679): vehicles:VehicleState.state is one of: UNKNOWN, UNAVAILABLE, ERROR, IDLE, EXECUTING, CHARGING. The vehicle's current state.  
  _Documentation schema confirms vehicle states: UNKNOWN, UNAVAILABLE, ERROR, IDLE, EXECUTING, CHARGING._
- **C-0015** — code: A vehicle's orientation on a route step or in the model is reported as FORWARD, BACKWARD, or UNDEFINED. (`Vehicle.java:1441-1457`)  
  doc (vehicles.yaml:941): vehicles:Step.vehicleOrientation is one of: FORWARD, BACKWARD, UNDEFINED.  
  _Vehicles/Step vehicleOrientation is explicitly documented as one of FORWARD, BACKWARD, UNDEFINED._
- **C-0016** — code: A transport order lifecycle transitions through RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, and UNROUTABLE states. (`TransportOrder.java:990-1033`)  
  doc (transport-orders.yaml:297): transport-orders:TransportOrderState.state is one of: RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, UNROUTABLE. The transport order's current state.  
  _TransportOrderState defines the states RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, UNROUTABLE._
- **C-0017** — code: A peripheral job lifecycle is represented by the states TO_BE_PROCESSED, BEING_PROCESSED, FINISHED, and FAILED. (`PeripheralJob.java:420-438`)  
  doc (common.yaml:136): common:PeripheralJobState.state is one of: TO_BE_PROCESSED, BEING_PROCESSED, FINISHED, FAILED. The peripheral job's current state.  
  _PeripheralJobState is documented as having states TO_BE_PROCESSED, BEING_PROCESSED, FINISHED, FAILED._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (common.yaml:182): common:PeripheralOperation.executionTrigger is one of: AFTER_ALLOCATION, AFTER_MOVEMENT, IMMEDIATE. The moment at which this operation is to be performed.  
  _PeripheralOperation executionTrigger is documented as one of AFTER_ALLOCATION, AFTER_MOVEMENT, IMMEDIATE._
- **C-0019** — code: Rerouting of a transport order route step is categorized as either REGULAR or FORCED. (`ReroutingType.java:10-21`)  
  doc (transport-orders.yaml:534): transport-orders:RouteStep.reroutingType is one of: REGULAR, FORCED.  
  _RouteStep reroutingType is documented as one of REGULAR, FORCED._
- **C-0022** — code: User notifications generated by the kernel are assigned a severity level of INFORMATIONAL, NOTEWORTHY, or IMPORTANT. (`UserNotification.java:134-148`)  
  doc (user-notification.yaml:25): user-notification:UserNotificationLevel is one of: INFORMATIONAL, NOTEWORTHY, IMPORTANT.  
  _UserNotificationLevel is documented as one of INFORMATIONAL, NOTEWORTHY, IMPORTANT._
- **C-0025** — code: An acceptable order type transfer object defines the name of the order type and the vehicle's execution priority for that type. (`AcceptableOrderTypeTO.java:10-23`)  
  doc (users-guide-03_reference.adoc:265): Reference › Supported navigation principles / vehicle types › Plant model elements › Vehicle: A set of _acceptable transport order types_, each consisting of a name and a priority, which are used for filtering transport orders (by their type) and sorting them (by their priority) when assigning trans  
  _Acceptable transport order types are documented as consisting of a name and a priority for filtering and sorting._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (users-guide-03_reference.adoc:121): Reference › Supported navigation principles / vehicle types › Plant model elements › Point › Bounding box: A bounding box is characterised by a reference point that is -- by default -- located in the center of the bounding box's base (i.e. at height 0), named _base center_ for the remainder of this   
  _Bounding boxes are defined with length, width, height, and a reference point offset (described in D-0397 and D-0398)._
- **C-0027** — code: A generic Property object defines an arbitrary key-value string pair. (`Property.java:10-26`)  
  doc (users-guide-03_reference.adoc:448): Reference › Supported navigation principles / vehicle types › Common element attributes › Generic properties: In addition to the listed attributes, it is possible to define arbitrary properties as key-value pairs for all driving course elements, which for example can be read and evaluated by vehicle  
  _Generic properties are documented as arbitrary key-value character string pairs._
- **C-0030** — code: Updating a vehicle's acceptable order types accepts a list of AcceptableOrderTypeTO instances containing name and priority. (`PutVehicleAcceptableOrderTypesTO.java:12-25`)  
  doc (users-guide-03_reference.adoc:265): Reference › Supported navigation principles / vehicle types › Plant model elements › Vehicle: A set of _acceptable transport order types_, each consisting of a name and a priority, which are used for filtering transport orders (by their type) and sorting them (by their priority) when assigning trans  
  _Documentation confirms that acceptable transport order types each consist of a name and a priority (D-0441)._
- **C-0031** — code: Updating a vehicle's allowed order types accepts a list of order type names. (`PutVehicleAllowedOrderTypesTO.java:11-24`)  
  doc (changelog.adoc:608): openTCS: Change Log › Version 5.9 (2023-05-31): Add support for updating and retrieving a vehicle's allowed order types via the web API.  
  _D-0102 documents support for updating allowed order types via the web API accepting order type names._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (vehicles.yaml:800): vehicles:EnergyLevelThresholdSet.energyLevelFullyRecharged; required; type integer. The energy level value (in %) at/above which the vehicle is considered fully recharged, i.e. _should_ stop recharging.  
  _EnergyLevelThresholdSet defines critical, good, sufficiently recharged, and fully recharged energy levels (in %)._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (common.yaml:270): common:ObjectHistory.entries; required; type array. The object history entries.  
  _ObjectHistory defines entries (D-1684) with eventCode (D-1686) and supplements (D-1687)._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:251): environmental-entities:EnvironmentalEntity.type is one of: OBJECT, ZONE. This entity's type.  
  _Documentation confirms environmental entities are typed as OBJECT or ZONE and include integration level, envelope, and properties (D-0868, D-0869, D-0880, D-0493)._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:204): order-sequences:OrderSequenceState.finished; required; type boolean. Indicates whether this order sequence has been processed completely.  
  _OrderSequenceState attributes include finished (D-0988), finishedIndex (D-0986), intendedVehicle (D-0990), and processingVehicle (D-0991)._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:125): events:OrderStatusMessage.intendedVehicle; optional; type string. The name of the vehicle that is intended to process the transport order.  
  _OrderStatusMessage documents status updates for transport orders including intended vehicle (D-0907) and associated properties._
- **C-0100** — code: POST /v1/vehicles/dispatcher/trigger triggers a full dispatch run and returns an empty text/plain;charset=utf-8 response with HTTP 200 upon completion. (`V1RequestHandler.java:169-169`)  
  doc (vehicles.yaml:586): POST /vehicles/dispatcher/trigger returns 200: Successful response  
  _Documentation in D-1590, D-1591, and D-1592 confirms POST /vehicles/dispatcher/trigger explicitly triggers dispatching and returns HTTP 200._
- **C-0200** — code: Computes candidate routes from a source point to one or more destination points for the vehicle specified by the path parameter NAME. (`V1RequestHandler.java:170-170`)  
  doc (vehicles.yaml:493): POST /vehicles/{NAME}/routeComputationQuery — Computes routes for the named vehicle to the given destination points.  
  _POST /vehicles/{NAME}/routeComputationQuery computes routes for the named vehicle to the given destination points as documented in D-1578 and D-1584._
- **C-0201** — code: Accepts a JSON request body containing a required list of destination points and optional source point and resources to avoid. (`V1RequestHandler.java:326-345`)  
  doc (changelog.adoc:431): openTCS: Change Log › Version 5.16 (2024-05-09): The endpoint `POST /vehicles/{NAME}/routeComputationQuery` now also accepts an optional list of names of resources to avoid.  
  _Confirmed by OpenAPI specification in D-1584 and changelog D-0062 stating the request body accepts destination points, optional source point, and optional resources to avoid._
- **C-0202** — code: Defaults the query parameter maxRoutesPerDestinationPoint to 1 if it is not provided. (`V1RequestHandler.java:782-783`)  
  doc (vehicles.yaml:501): POST /vehicles/{NAME}/routeComputationQuery: query parameter 'maxRoutesPerDestinationPoint'; optional; default 1; type integer. The maximum number of routes to compute for one destination point.  
  _The OpenAPI spec explicitly confirms maxRoutesPerDestinationPoint is optional with default 1._
- **C-0300** — code: Attaches a communication adapter to the vehicle identified by the NAME path parameter using the driver class specified by the newValue query parameter, returning an empty text/plain response. (`V1RequestHandler.java:171-174`)  
  doc (vehicles.yaml:394): PUT /vehicles/{NAME}/commAdapter/attachment: query parameter 'newValue'; required; type string. The description class name of the vehicle driver that is to be attached.  
  _Matches PUT /vehicles/{NAME}/commAdapter/attachment with newValue specifying the driver class name._
- **C-0400** — code: Gets communication adapter attachment information for the vehicle specified by NAME, returning vehicle name, available adapter class names, and attached adapter class name. (`V1RequestHandler.java:175-178`)  
  doc (vehicles.yaml:355): GET /vehicles/{NAME}/commAdapter/attachmentInformation — Retrieves the driver attachment information of this vehicle.  
  _GET /vehicles/{NAME}/commAdapter/attachmentInformation endpoint retrieves driver attachment information for the vehicle._
- **C-0500** — code: Enables or disables the communication adapter of the vehicle specified by path parameter NAME according to boolean query parameter newValue. (`V1RequestHandler.java:179-179`)  
  doc (vehicles.yaml:437): PUT /vehicles/{NAME}/commAdapter/enabled: query parameter 'newValue'; required; type boolean. The vehicle driver's new enabled state.  
  _PUT /vehicles/{NAME}/commAdapter/enabled with query parameter newValue (boolean) enables/disables the vehicle driver/comm adapter._
- **C-0600** — code: Sends a message to the communication adapter of the vehicle specified by NAME using the request body. (`V1RequestHandler.java:180-180`)  
  doc (vehicles.yaml:461): POST /vehicles/{NAME}/commAdapter/message — Sends a message to the named vehicle's driver.  
  _POST /vehicles/{NAME}/commAdapter/message sends a message to the named vehicle's driver/communication adapter._
- **C-0700** — code: Sets the paused state of the vehicle specified by NAME based on query parameter newValue and returns an empty text response. (`V1RequestHandler.java:181-181`)  
  doc (vehicles.yaml:194): PUT /vehicles/{NAME}/paused — Sets the paused state for the named vehicle.  
  _PUT /vehicles/{NAME}/paused sets the paused state for the named vehicle based on boolean query parameter newValue._
- **C-0800** — code: Updates the integration level of the vehicle specified by NAME to the value provided in the newValue query parameter. (`V1RequestHandler.java:182-182`)  
  doc (vehicles.yaml:165): PUT /vehicles/{NAME}/integrationLevel: query parameter 'newValue'; required; type string. The vehicle's new integration level.  
  _PUT /vehicles/{NAME}/integrationLevel updates the vehicle's integration level using query parameter newValue._
- **C-0900** — code: POST /v1/vehicles/{NAME}/withdrawal requests the withdrawal of any transport order currently assigned to the specified vehicle. (`V1RequestHandler.java:183-183`)  
  doc (vehicles.yaml:78): POST /vehicles/{NAME}/withdrawal — Withdraws a transport order processed by the vehicle with the given name.  
  _D-1518 explicitly documents POST /vehicles/{NAME}/withdrawal as withdrawing a transport order processed by the vehicle with the given name._
- **C-1000** — code: Clients can request rerouting for a vehicle identified by NAME via POST /v1/vehicles/{NAME}/rerouteRequest, returning an empty response body. (`V1RequestHandler.java:184-184`)  
  doc (vehicles.yaml:119): POST /vehicles/{NAME}/rerouteRequest — Reroutes a vehicle with the given name.  
  _D-1524 and D-1527 document POST /vehicles/{NAME}/rerouteRequest rerouting the named vehicle and returning 200 (empty response)._
- **C-1001** — code: The optional forced query parameter specifies whether to perform forced rerouting instead of regular rerouting, defaulting to false if omitted or not equal to true. (`V1RequestHandler.java:835-837`)  
  doc (vehicles.yaml:128): POST /vehicles/{NAME}/rerouteRequest: query parameter 'forced'; optional; default false; type boolean. Whether the vehicle should be rerouted even if it's not where it is expected to be. Forced rerouting of a vehicle from its current position can disrupt traffic management if used inappropriately. O  
  _D-1526 documents query parameter 'forced' as optional, boolean, default false, controlling forced rerouting._
- **C-1200** — code: Updates the acceptable order types and their priorities for the vehicle identified by the NAME path parameter using the provided request body, returning an empty text response. (`V1RequestHandler.java:186-189`)  
  doc (vehicles.yaml:271): PUT /vehicles/{NAME}/acceptableOrderTypes — Sets the acceptable order types for the named vehicle.  
  _Matches D-1546 and D-1551 which document setting acceptable order types for the named vehicle via PUT request body._
- **C-1300** — code: Updates the energy level threshold set of the vehicle identified by the NAME path parameter using the provided critical, good, sufficiently recharged, and fully recharged threshold values. (`V1RequestHandler.java:190-193`)  
  doc (vehicles.yaml:313): PUT /vehicles/{NAME}/energyLevelThresholdSet — Sets energy level threshold values for the named vehicle (in percent).  
  _D-1552 specifies PUT /vehicles/{NAME}/energyLevelThresholdSet sets energy level threshold values for the named vehicle._
- **C-1400** — code: Updates the envelope key of the vehicle identified by NAME to the value specified in the newValue query parameter. (`V1RequestHandler.java:194-194`)  
  doc (vehicles.yaml:556): PUT /vehicles/{NAME}/envelopeKey: query parameter 'newValue'; optional; type string. The vehicle's new envelope key.  
  _D-1587 defines PUT /vehicles/{NAME}/envelopeKey query parameter 'newValue' to update the vehicle's envelope key._
- **C-1500** — code: Retrieves the vehicle state and details for the vehicle identified by the NAME path parameter. (`V1RequestHandler.java:195-195`)  
  doc (vehicles.yaml:51): GET /vehicles/{NAME}: path parameter 'NAME'; required; type string. The name of the vehicle to be retrieved.  
  _D-1515 documents GET /vehicles/{NAME} to retrieve the vehicle by name._
- **C-1601** — code: When the procState query parameter is specified, only vehicles matching the given processing state are returned. (`V1RequestHandler.java:609-609`)  
  doc (vehicles.yaml:12): GET /vehicles: query parameter 'procState'; optional; default null; type string. The processing state of the vehicles to be retrieved.  
  _D-1511 specifies GET /vehicles parameter procState for the processing state of vehicles to be retrieved._
- **C-1602** — code: When the procState query parameter is omitted, all vehicles are returned regardless of their processing state. (`VehicleHandler.java:86-88`)  
  doc (vehicles.yaml:12): GET /vehicles: query parameter 'procState'; optional; default null; type string. The processing state of the vehicles to be retrieved.  
  _OpenAPI documentation specifies that procState is optional with default null for retrieving vehicles by processing state._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:272): environmental-entities:EnvironmentalEntity.layout.layerId; optional; type integer  
  _Layout layer ID is part of the environmental entity layout data model in the OpenAPI specification._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:348): environmental-entities:EnvironmentalEntityEnvelope.vertices; required; type array. The sequence of vertices this envelope consists of.  
  _Environmental entity envelope vertices are required and represented as an array/polygon list in the OpenAPI schema._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:365): environmental-entities:EnvironmentalEntityPose.position; required  
  _Environmental entity pose defines a position in the OpenAPI schema._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:369): environmental-entities:EnvironmentalEntityPose.orientationAngle; required; type number  
  _Environmental entity pose defines orientation angle in the OpenAPI schema._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:94): events:OrderStatusMessage.type; optional; type string; default TransportOrder  
  _Order status message type specifies default TransportOrder in the OpenAPI schema._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:94): events:OrderStatusMessage.type is one of: TransportOrder.  
  _OrderStatusMessage type enum is TransportOrder._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:99): events:OrderStatusMessage.sequenceNumber; optional  
  _OrderStatusMessage defines optional sequenceNumber._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:101): events:OrderStatusMessage.dispensable; optional; type boolean. Whether this order is dispensable (may be withdrawn automatically).  
  _OrderStatusMessage defines optional dispensable boolean flag._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:105): events:OrderStatusMessage.orderName; optional; type string. The (optional) transport order name  
  _OrderStatusMessage defines optional orderName string._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:109): events:OrderStatusMessage.orderType; optional; type string. The type of the transport order.  
  _OrderStatusMessage defines optional orderType string._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:113): events:OrderStatusMessage.orderState; optional; type string. The transport order's current state  
  _Claim C-0040 verifies that transport order status messages communicate order state._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:130): events:OrderStatusMessage.processingVehicleName; optional; type string. The processing vehicle's name  
  _Claim C-0040 confirms transport order status messages include the executing/processing vehicle name._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:134): events:OrderStatusMessage.peripheralReservationToken; optional; type string. An (optional) token for reserving peripheral devices while processing this transport order.  
  _Claim C-0040 confirms transport order status messages include the peripheral reservation token._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:138): events:OrderStatusMessage.wrappingSequence; optional; type string. The order sequence this transport order belongs to. May be `null` in case this order isn't part of any sequence.  
  _Claim C-0040 confirms transport order status messages include the wrapping sequence._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (events.yaml:144): events:OrderStatusMessage.destinations; optional; type array. The transport order's destinations  
  _Claim C-0040 confirms transport order status messages include destinations._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (events.yaml:151): events:OrderStatusMessage.properties; optional; type array. The transport order's properties  
  _Transport order status messages include order properties, supported by C-0009/C-0027._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:174): events:VehicleStatusMessage.type is one of: Vehicle.  
  _Vehicle status messages report vehicle type 'Vehicle' as part of the status message structure._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:181): events:VehicleStatusMessage.vehicleName; optional; type string. The vehicle's name  
  _Claim C-0035 verifies that vehicle status messages broadcast vehicle name._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:185): events:VehicleStatusMessage.properties; optional; type array. The vehicle's properties  
  _Claim C-0035 verifies that vehicle status messages broadcast vehicle properties._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:190): events:VehicleStatusMessage.boundingBox; optional  
  _Claim C-0035 verifies that vehicle status messages include bounding box._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:192): events:VehicleStatusMessage.energyLevelGood; optional; type integer. The value (in %) at/above which the vehicle's energy level is considered 'good'.  
  _Claim C-0035 verifies vehicle status messages broadcast energy thresholds including energyLevelGood._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:197): events:VehicleStatusMessage.energyLevelCritical; optional; type integer. The value (in %) at/below which the vehicle's energy level is considered 'critical'.
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:202): events:VehicleStatusMessage.energyLevelSufficientlyRecharged; optional; type integer. The value (in %) at/below which the vehicle's energy level is considered 'sufficiently recharged'.
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:212): events:VehicleStatusMessage.energyLevel; optional; type integer. The vehicle's remaining energy (in %).
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:216): events:VehicleStatusMessage.integrationLevel; optional; type string. The vehicle's integration level.
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:224): events:VehicleStatusMessage.paused; optional; type boolean. Whether the vehicle is paused.
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:228): events:VehicleStatusMessage.procState; optional; type string. The vehicle's current processing state
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (events.yaml:236): events:VehicleStatusMessage.procStateTimestamp; optional; type string; format date-time. The time at which the vehicle has entered its current processing state (expressed according to ISO 8601).
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:241): events:VehicleStatusMessage.transportOrderName; optional; type string. The name of the transport order the vehicle currently processes
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:245): events:VehicleStatusMessage.position; optional; type string. The name of the point the vehicle currently occupies
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:249): events:VehicleStatusMessage.precisePosition; optional
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:251): events:VehicleStatusMessage.vehicleOrientationAngle; optional. The vehicle's current orientation angle (-360..360). May be a string ("NaN") if the vehicle hasn't provided an orientation angle.
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:260): events:VehicleStatusMessage.state; optional; type string. The vehicle's current state
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (events.yaml:270): events:VehicleStatusMessage.stateTimestamp; optional; type string; format date-time. The time at which the vehicle has entered its current state (expressed according to ISO 8601).  
  _Supported by C-0034 and C-0035 which specify vehicle stateTimestamps / state timestamps in vehicle state and status messages._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:275): events:VehicleStatusMessage.allocatedResources; optional; type array. The resources already allocated by the vehicle.  
  _Supported by C-0035 and C-0034 confirming allocatedResources in vehicle status messages and state responses._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:281): events:VehicleStatusMessage.claimedResources; optional; type array. The resources claimed - i.e. not yet allocated - for the vehicle's route.  
  _Supported by C-0035 and C-0034 confirming claimedResources in vehicle status messages._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:287): events:VehicleStatusMessage.acceptableOrderTypes; optional; type array  
  _Supported by C-0035 confirming acceptableOrderTypes in vehicle status messages._
- **C-0035** — code: Vehicle status messages broadcast vehicle name, state, processing state, energy thresholds, positions, orientation angle, bounding box, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and assigned transport order name. (`VehicleStatusMessage.java:19-140`)  
  doc (events.yaml:292): events:VehicleStatusMessage.envelopeKey; optional; type string. The envelope key for this vehicle.  
  _Supported by C-0035 confirming envelopeKey in vehicle status messages._
- **C-0025** — code: An acceptable order type transfer object defines the name of the order type and the vehicle's execution priority for that type. (`AcceptableOrderTypeTO.java:10-23`)  
  doc (events.yaml:325): events:AcceptableOrderTypes.acceptableOrderTypes; required; type array  
  _Supported by C-0025 and C-0030 describing acceptable order types array structure._
- **C-0025** — code: An acceptable order type transfer object defines the name of the order type and the vehicle's execution priority for that type. (`AcceptableOrderTypeTO.java:10-23`)  
  doc (events.yaml:330): events:AcceptableOrderTypes.acceptableOrderTypes[].name; required; type string. The name of the order type.  
  _Supported by C-0025 defining acceptable order type name property._
- **C-0025** — code: An acceptable order type transfer object defines the name of the order type and the vehicle's execution priority for that type. (`AcceptableOrderTypeTO.java:10-23`)  
  doc (events.yaml:333): events:AcceptableOrderTypes.acceptableOrderTypes[].priority; required; type integer. The priority of the order type, with a lower value indicating a higher priority.  
  _Supported by C-0025 defining acceptable order type priority where lower value indicates higher priority._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:161): order-sequences:OrderSequenceState.name; required; type string. The name of the order sequence.  
  _Supported by C-0039 describing OrderSequenceState representation._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:165): order-sequences:OrderSequenceState.type; required; type string. The type of the order sequence. Deprecated: Use `orderTypes` instead.  
  _Supported by C-0039 covering order sequence type fields and states._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:172): order-sequences:OrderSequenceState.orderTypes; required; type array  
  _Supported by C-0039 covering order types in order sequence state._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:178): order-sequences:OrderSequenceState.orders; required; type array  
  _Supported by C-0039 covering processed order list/orders in order sequence state._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:184): order-sequences:OrderSequenceState.creationTime; required; type string; format date-time. The point of time at which this order sequence was created (expressed according to ISO 8601).  
  _Supported by C-0039 and OrderSequence state tracking creation/finish timestamps._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:189): order-sequences:OrderSequenceState.finishedTime; required; type string; format date-time. The point of time at which this order sequence has been processed completely (expressed according to ISO 8601).  
  _Supported by C-0039 regarding finish status and timestamps._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:200): order-sequences:OrderSequenceState.complete; required; type boolean. Indicates whether this order sequence is complete and will not be extended by more orders.  
  _Supported by C-0039 detailing order sequence execution completion and complete status._
- **C-0039** — code: Order sequence SSE events broadcast changes to order sequences including execution completion, finish status, failure fatality, processed order list, finished index, and intended or processing vehicle assignments. (`OrderSequenceEventTO.java:15-65`)  
  doc (order-sequences.yaml:208): order-sequences:OrderSequenceState.failureFatal; required; type boolean. Indicates whether the failure of one order in this sequence is fatal to all subsequent orders.  
  _Supported by C-0039 detailing failure fatality tracking on order sequences._
- **C-0027** — code: A generic Property object defines an arbitrary key-value string pair. (`Property.java:10-26`)  
  doc (order-sequences.yaml:223): order-sequences:OrderSequenceState.properties; required; type array. The order sequences properties  
  _Supported by generic properties modeling in C-0027._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (plant-model.yaml:957): plant-model:PlantModelPath.peripheralOperations[].executionTrigger; optional; type string  
  _Supported by peripheral operation execution trigger modeling in C-0018._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (plant-model.yaml:962): plant-model:PlantModelPath.peripheralOperations[].completionRequired; optional; type boolean  
  _Supported by peripheral operation completion requirements in C-0042._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (plant-model.yaml:1465): plant-model:PlantModelVehicle.layout.routeColor; optional; type string  
  _Supported by vehicle layout route color specification in C-0036._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:134): sse:VehicleEvent.currentObjectState; optional  
  _Supported by C-0036 which covers vehicle SSE events delivering currentObjectState._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:136): sse:VehicleEvent.previousObjectState; optional  
  _C-0036 confirms vehicle SSE events deliver the previous and current object state of a vehicle._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:141): sse:Vehicle.name; required; type string  
  _C-0036 and C-0034 confirm vehicle name is present in vehicle state representations._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:143): sse:Vehicle.properties; required; type object  
  _C-0036 and C-0034 confirm vehicle properties are included._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (sse.yaml:147): sse:Vehicle.history; required  
  _C-0037 and C-0036 confirm vehicle history is provided._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:149): sse:Vehicle.boundingBox; required  
  _C-0036 confirms vehicle boundingBox is included._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:151): sse:Vehicle.energyLevelThresholdSet; required  
  _C-0036 confirms energy thresholds/energyLevelThresholdSet is included in vehicle SSE events._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (sse.yaml:153): sse:Vehicle.energyLevel; required; type integer  
  _C-0034 confirms energy level integer is reported on vehicles._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:157): sse:Vehicle.maxVelocity; required; type integer  
  _C-0036 confirms velocity limits (maxVelocity) are included in vehicle SSE events._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:161): sse:Vehicle.maxReverseVelocity; required; type integer  
  _C-0036 confirms velocity limits (maxReverseVelocity) are included in vehicle SSE events._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (sse.yaml:165): sse:Vehicle.rechargeOperation; required; type string  
  _C-0034 and C-0036 confirm vehicle state details include rechargeOperation._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:167): sse:Vehicle.loadHandlingDevices; required; type array  
  _C-0036 confirms loadHandlingDevices list/array is included in vehicle SSE events._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:171): sse:Vehicle.state; required  
  _C-0036 and C-0013 confirm vehicle operational state is included._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:173): sse:Vehicle.procState; required  
  _Supported by C-0036 (and C-0014/C-0034) which details vehicle SSE state containing procState._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:175): sse:Vehicle.integrationLevel; required  
  _Supported by C-0036 (and C-0012/C-0034) which includes vehicle integration level in SSE events._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:177): sse:Vehicle.paused; required; type boolean  
  _Supported by C-0036 (and C-0034/C-0035) confirming paused boolean status in vehicle state._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:179): sse:Vehicle.transportOrder; required; type string; nullable  
  _Supported by C-0036 confirming vehicle SSE events contain nullable transport order name reference._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:182): sse:Vehicle.orderSequence; required; type string; nullable  
  _Supported by C-0036 confirming vehicle SSE events include assigned order sequence reference._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:185): sse:Vehicle.acceptableOrderTypes; required; type array  
  _Supported by C-0036 (and C-0025/C-0034/C-0035) confirming acceptable order types array in vehicle state._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:189): sse:Vehicle.claimedResources; required; type array  
  _Supported by C-0036 (and C-0034/C-0035) confirming claimed resources array in vehicle state._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:195): sse:Vehicle.allocatedResources; required; type array  
  _Supported by C-0036 (and C-0034/C-0035) confirming allocated resources array in vehicle state._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:201): sse:Vehicle.currentPosition; required; type string; nullable  
  _Supported by C-0036 (and C-0034/C-0035) confirming current position property in vehicle state._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:204): sse:Vehicle.pose; required  
  _Supported by C-0036 confirming pose (coordinates and orientation angle) in vehicle SSE events._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:206): sse:Vehicle.envelopeKey; required; type string; nullable  
  _Supported by C-0036 (and C-0034/C-0035) confirming nullable envelopeKey in vehicle state._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:209): sse:Vehicle.layout; required  
  _Supported by C-0036 confirming layout information (such as route colors) in vehicle SSE events._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (sse.yaml:238): sse:BoundingBox.length; required; type integer  
  _BoundingBox length dimension is defined and serialized as integer in the model._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (sse.yaml:242): sse:BoundingBox.width; required; type integer  
  _BoundingBox width dimension is defined and serialized as integer in the model._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (sse.yaml:246): sse:BoundingBox.height; required; type integer  
  _BoundingBox height dimension is defined and serialized as integer in the model._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (sse.yaml:250): sse:BoundingBox.referenceOffset; required  
  _BoundingBox referenceOffset is defined and included in the transfer object._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (sse.yaml:260): sse:EnergyLevelThresholdSet.energyLevelCritical; required; type integer  
  _EnergyLevelThresholdSet defines critical energy level threshold._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (sse.yaml:264): sse:EnergyLevelThresholdSet.energyLevelGood; required; type integer  
  _EnergyLevelThresholdSet defines good energy level threshold._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (sse.yaml:268): sse:EnergyLevelThresholdSet.energyLevelFullyRecharged; required; type integer  
  _EnergyLevelThresholdSet defines fully recharged energy level threshold._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (sse.yaml:272): sse:EnergyLevelThresholdSet.energyLevelSufficientlyRecharged; required; type integer  
  _EnergyLevelThresholdSet defines sufficiently recharged energy level threshold._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:284): sse:LoadHandlingDevice.label; required; type string  
  _LoadHandlingDevice has required string label in the vehicle model/state._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:286): sse:LoadHandlingDevice.full; required; type boolean  
  _LoadHandlingDevice has required boolean full flag._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (sse.yaml:319): sse:TimestampedVehicleState.state; required; type string  
  _TimestampedVehicleState includes vehicle state string._
- **C-0014** — code: A vehicle's processing state is IDLE when no order is assigned, AWAITING_ORDER when assigned to a transport order but awaiting the next drive order, or PROCESSING_ORDER when actively executing a drive order. (`Vehicle.java:1420-1435`)  
  doc (sse.yaml:344): sse:TimestampedVehicleProcState.state is one of: IDLE, AWAITING_ORDER, PROCESSING_ORDER.  
  _TimestampedVehicleProcState.state is one of IDLE, AWAITING_ORDER, PROCESSING_ORDER as defined by vehicle processing states._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:394): sse:TransportOrder.name; required; type string  
  _TransportOrder.name is present and required in transport order status/event payloads._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:396): sse:TransportOrder.properties; required; type object  
  _TransportOrder properties map is included in transport order model representations._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (sse.yaml:400): sse:TransportOrder.history; required  
  _Object history entries sequence is supported by C-0037._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:402): sse:TransportOrder.type; required; type string  
  _Order type field is part of TransportOrder status messages as confirmed in C-0040._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:404): sse:TransportOrder.dependencies; required; type array  
  _Dependencies array is supported by TransportOrder model representations._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:408): sse:TransportOrder.driveOrders; required; type array  
  _Drive orders list is supported in transport order SSE events per C-0041._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:412): sse:TransportOrder.peripheralReservationToken; required; type string; nullable  
  _Peripheral reservation token is communicated on transport order status messages per C-0040._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:415): sse:TransportOrder.currentDriveOrderIndex; required; type integer  
  _Current drive order index is part of transport order state tracking per C-0041._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:417): sse:TransportOrder.currentRouteStepIndex; required; type integer  
  _Current route step index is streamed in transport order SSE events per C-0041._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:419): sse:TransportOrder.state; required  
  _Supported by C-0041 and C-0016 which establish TransportOrder SSE events stream order state._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:424): sse:TransportOrder.deadline; required; type string; format date-time  
  _Supported by C-0041 which confirms TransportOrder SSE events stream deadline._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:430): sse:TransportOrder.intendedVehicle; required; type string; nullable  
  _Supported by C-0040 which confirms TransportOrder reports intended vehicle._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:433): sse:TransportOrder.processingVehicle; required; type string; nullable  
  _Supported by C-0040 which confirms TransportOrder reports executing/processing vehicle._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:436): sse:TransportOrder.wrappingSequence; required; type string; nullable  
  _Supported by C-0040 which confirms TransportOrder reports wrapping sequence._
- **C-0040** — code: Transport order status messages communicate order state, destinations, intended vehicle, executing vehicle, peripheral reservation token, dispensable flag, order type, and wrapping sequence. (`OrderStatusMessage.java:14-48`)  
  doc (sse.yaml:439): sse:TransportOrder.dispensable; required; type boolean  
  _Supported by C-0040 which confirms TransportOrder reports dispensable flag._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (sse.yaml:464): sse:DriveOrder.destination; required  
  _Supported by C-0009 and C-0010 which confirm destination state is reported for drive orders._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:468): sse:DriveOrder.route; required  
  _Supported by C-0041 which confirms TransportOrder/DriveOrder SSE events include route steps._
- **C-0010** — code: A drive order and its mapped destination state can be PRISTINE (unprocessed), TRAVELLING (vehicle moving towards destination), OPERATING (executing an operation), FINISHED (successfully completed), or FAILED. (`DriveOrder.java:347-369`)  
  doc (sse.yaml:470): sse:DriveOrder.state; required  
  _DriveOrder.state is confirmed by claim C-0010 and C-0041._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (sse.yaml:481): sse:DriveOrderDestination.destination; required; type string  
  _DriveOrderDestination destination property is confirmed by claim C-0009._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (sse.yaml:483): sse:DriveOrderDestination.operation; required; type string  
  _DriveOrderDestination operation property is confirmed by claim C-0009._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (sse.yaml:485): sse:DriveOrderDestination.properties; required; type object  
  _DriveOrderDestination properties field is confirmed by claim C-0009._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:497): sse:Route.steps; required; type array  
  _Route.steps property in transport order drive order SSE streaming is confirmed by claim C-0041._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (sse.yaml:501): sse:Route.costs; required; type integer; format int64  
  _Route.costs is confirmed by claims C-0033 and C-0041._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (sse.yaml:510): sse:RouteStep.path; required; type string  
  _RouteStep.path is confirmed by claims C-0033 and C-0041._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (sse.yaml:512): sse:RouteStep.sourcePoint; required; type string  
  _RouteStep.sourcePoint is confirmed by claims C-0033 and C-0041._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (sse.yaml:514): sse:RouteStep.destinationPoint; required; type string  
  _RouteStep.destinationPoint is confirmed by claims C-0033 and C-0041._
- **C-0015** — code: A vehicle's orientation on a route step or in the model is reported as FORWARD, BACKWARD, or UNDEFINED. (`Vehicle.java:1441-1457`)  
  doc (sse.yaml:516): sse:RouteStep.vehicleOrientation; required; type string  
  _RouteStep.vehicleOrientation is confirmed by claims C-0015, C-0033, and C-0041._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:519): sse:RouteStep.routeIndex; required; type integer  
  _RouteStep.routeIndex is confirmed by claim C-0041._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (sse.yaml:521): sse:RouteStep.costs; required; type integer; format int64  
  _RouteStep.costs is confirmed by claim C-0033._
- **C-0041** — code: Transport order SSE events stream state updates for transport orders and drive orders, including deadline, route steps, and current route step index. (`TransportOrderEventTO.java:18-105`)  
  doc (sse.yaml:524): sse:RouteStep.executionAllowed; required; type boolean  
  _RouteStep executionAllowed is implemented as part of RouteStep in transport order SSE events (C-0041)._
- **C-0019** — code: Rerouting of a transport order route step is categorized as either REGULAR or FORCED. (`ReroutingType.java:10-21`)  
  doc (sse.yaml:526): sse:RouteStep.reroutingType; required; type string; nullable  
  _RouteStep reroutingType is implemented in the model and SSE events (C-0019, C-0041)._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (sse.yaml:630): sse:PeripheralJob.name; required; type string  
  _PeripheralJob name property is implemented and broadcast via peripheral job SSE events (C-0042, C-0043)._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (sse.yaml:632): sse:PeripheralJob.properties; required; type object  
  _PeripheralJob properties mapping is implemented and broadcast via peripheral job SSE events (C-0042)._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (sse.yaml:636): sse:PeripheralJob.history; required  
  _PeripheralJob history property is implemented via ObjectHistory (C-0037, C-0042)._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (sse.yaml:638): sse:PeripheralJob.reservationToken; required; type string  
  _PeripheralJob reservationToken property is implemented and sent in peripheral job SSE events (C-0042, C-0043)._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (sse.yaml:641): sse:PeripheralJob.relatedVehicle; required; type string; nullable  
  _PeripheralJob relatedVehicle property is implemented and nullable in peripheral job SSE events (C-0042, C-0043)._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (sse.yaml:644): sse:PeripheralJob.relatedTransportOrder; required; type string; nullable  
  _PeripheralJob relatedTransportOrder property is implemented and nullable in peripheral job SSE events (C-0042, C-0043)._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (sse.yaml:647): sse:PeripheralJob.peripheralOperation; required  
  _PeripheralJob peripheralOperation structure is implemented and broadcast via peripheral job SSE events (C-0042, C-0043)._
- **C-0017** — code: A peripheral job lifecycle is represented by the states TO_BE_PROCESSED, BEING_PROCESSED, FINISHED, and FAILED. (`PeripheralJob.java:420-438`)  
  doc (sse.yaml:649): sse:PeripheralJob.state; required  
  _PeripheralJob state is implemented and transitions through peripheral job lifecycle states (C-0017, C-0042)._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (sse.yaml:651): sse:PeripheralJob.creationTime; required; type string; format date-time  
  _PeripheralJob creationTime timestamp is implemented in peripheral job representation (C-0043, C-0042)._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (sse.yaml:654): sse:PeripheralJob.finishedTime; required; type string; format date-time  
  _PeripheralJob finishedTime timestamp is implemented in peripheral job representation (C-0043, C-0042)._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (sse.yaml:671): sse:PeripheralOperation.location; required; type string  
  _Peripheral operation location is reported by the peripheral operation structure._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (sse.yaml:673): sse:PeripheralOperation.operation; required; type string  
  _Peripheral operation operation name is reported by the peripheral operation structure._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (sse.yaml:675): sse:PeripheralOperation.executionTrigger; required  
  _Peripheral operation executionTrigger is defined and reported on peripheral operations._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (sse.yaml:677): sse:PeripheralOperation.completionRequired; required; type boolean  
  _Peripheral operation completionRequired boolean flag is supported and reported._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (sse.yaml:684): sse:PeripheralOperationExecutionTrigger is one of: IMMEDIATE, AFTER_ALLOCATION, AFTER_MOVEMENT.  
  _PeripheralOperationExecutionTrigger values are IMMEDIATE, AFTER_ALLOCATION, AFTER_MOVEMENT._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:709): sse:EnvironmentalEntity.name; required; type string  
  _Environmental entity name is reported in environmental entity structures._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:711): sse:EnvironmentalEntity.properties; required; type object  
  _Environmental entity properties are reported in environmental entity structures._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (sse.yaml:715): sse:EnvironmentalEntity.history; required  
  _Environmental entity history is reported as an object history structure._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:717): sse:EnvironmentalEntity.envelope; required  
  _Environmental entity envelope is reported in environmental entity structures._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:719): sse:EnvironmentalEntity.pose; required  
  _Environmental entity pose is reported in environmental entity structures._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:721): sse:EnvironmentalEntity.type; required; type string. This entity's type.  
  _Environmental entity type is reported in environmental entity structures._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:721): sse:EnvironmentalEntity.type is one of: OBJECT, ZONE. This entity's type.  
  _Environmental entity types OBJECT and ZONE match C-0038._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:727): sse:EnvironmentalEntity.integrationLevel; required; type string. The entity's integration level.  
  _C-0038 confirms environmental entity SSE events include the entity's integration level._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:741): sse:EnvironmentalEntity.createdTime; required; type string; format date-time  
  _C-0038 confirms environmental entity SSE events include creation timestamps._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:744): sse:EnvironmentalEntity.retiredTime; required; type string; format date-time  
  _C-0038 confirms environmental entity SSE events include retirement timestamps._
- **C-1600** — code: GET /v1/vehicles retrieves a list of all vehicles and their states, sorted in ascending order by vehicle name. (`V1RequestHandler.java:196-196`)  
  doc (vehicles.yaml:9): GET /vehicles — Retrieves a set of vehicles.  
  _C-1600 confirms GET /vehicles retrieves the vehicles._
- **C-1600** — code: GET /v1/vehicles retrieves a list of all vehicles and their states, sorted in ascending order by vehicle name. (`V1RequestHandler.java:196-196`)  
  doc (vehicles.yaml:26): GET /vehicles returns 200: Successful response  
  _C-1600 confirms GET /vehicles returns 200 on successful vehicle retrieval._
- **C-1603** — code: If procState is not a valid vehicle processing state name, the request fails with HTTP 400 Bad Request. (`VehicleHandler.java:86-88`)  
  doc (vehicles.yaml:35): GET /vehicles returns 400: The submitted data is invalid. (example: Could not parse input.)  
  _C-1603 (and C-0001) confirms GET /vehicles returns HTTP 400 Bad Request on invalid submitted input parameters._
- **C-1500** — code: Retrieves the vehicle state and details for the vehicle identified by the NAME path parameter. (`V1RequestHandler.java:195-195`)  
  doc (vehicles.yaml:48): GET /vehicles/{NAME} — Retrieves the vehicle with the given name.  
  _C-1500 confirms GET /vehicles/{NAME} retrieves the vehicle with the specified name._
- **C-1500** — code: Retrieves the vehicle state and details for the vehicle identified by the NAME path parameter. (`V1RequestHandler.java:195-195`)  
  doc (vehicles.yaml:59): GET /vehicles/{NAME} returns 200: Successful operation  
  _C-1500 confirms GET /vehicles/{NAME} successfully returns vehicle details (HTTP 200)._
- **C-1501** — code: Returns HTTP 404 if no vehicle with the specified name exists. (`VehicleHandler.java:112-112`)  
  doc (vehicles.yaml:65): GET /vehicles/{NAME} returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _C-1501 (and C-0003) confirms GET /vehicles/{NAME} returns 404 when referencing an unknown vehicle._
- **C-0900** — code: POST /v1/vehicles/{NAME}/withdrawal requests the withdrawal of any transport order currently assigned to the specified vehicle. (`V1RequestHandler.java:183-183`)  
  doc (vehicles.yaml:104): POST /vehicles/{NAME}/withdrawal returns 200: Successful operation  
  _C-0900 confirms POST /vehicles/{NAME}/withdrawal returns successful 200 response upon requesting withdrawal._
- **C-0800** — code: Updates the integration level of the vehicle specified by NAME to the value provided in the newValue query parameter. (`V1RequestHandler.java:182-182`)  
  doc (vehicles.yaml:156): PUT /vehicles/{NAME}/integrationLevel — Sets a new integration level for the named vehicle.  
  _Supported by C-0800: updates the integration level of the named vehicle via PUT /vehicles/{NAME}/integrationLevel._
- **C-0800** — code: Updates the integration level of the vehicle specified by NAME to the value provided in the newValue query parameter. (`V1RequestHandler.java:182-182`)  
  doc (vehicles.yaml:179): PUT /vehicles/{NAME}/integrationLevel returns 200: Successful operation  
  _Supported by C-0800: returns an empty text response with HTTP 200 on success._
- **C-0801** — code: Returns HTTP 404 when no vehicle exists with the specified NAME. (`VehicleHandler.java:123-124`)  
  doc (vehicles.yaml:181): PUT /vehicles/{NAME}/integrationLevel returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by C-0801: returns HTTP 404 when no vehicle exists with the specified name._
- **C-0700** — code: Sets the paused state of the vehicle specified by NAME based on query parameter newValue and returns an empty text response. (`V1RequestHandler.java:181-181`)  
  doc (vehicles.yaml:212): PUT /vehicles/{NAME}/paused returns 200: Successful operation  
  _Supported by C-0700: returns 200 on successful pause update._
- **C-0701** — code: Returns HTTP 404 if no vehicle with the given NAME exists. (`VehicleHandler.java:140-141`)  
  doc (vehicles.yaml:214): PUT /vehicles/{NAME}/paused returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by C-0701: returns 404 if no vehicle with the given NAME exists._
- **C-1100** — code: Updates the set of acceptable order types for the vehicle identified by NAME with priority 0 for each specified order type, returning an empty plain text response. (`V1RequestHandler.java:185-185`)  
  doc (vehicles.yaml:228): PUT /vehicles/{NAME}/allowedOrderTypes — Sets the allowed order types for the named vehicle.  
  _Supported by C-1100 and C-0031: updates allowed order types for the named vehicle._
- **C-1100** — code: Updates the set of acceptable order types for the vehicle identified by NAME with priority 0 for each specified order type, returning an empty plain text response. (`V1RequestHandler.java:185-185`)  
  doc (vehicles.yaml:246): PUT /vehicles/{NAME}/allowedOrderTypes returns 200: Successful operation  
  _Supported by C-1100: returns HTTP 200 on success._
- **C-0001** — code: When an operation encounters an IllegalArgumentException, the Web API responds with HTTP 400 Bad Request and serializes the exception. (`ServiceWebApi.java:175-178`)  
  doc (vehicles.yaml:248): PUT /vehicles/{NAME}/allowedOrderTypes returns 400: The submitted data is invalid. (example: Could not parse JSON input.)  
  _Supported by C-0001: invalid request data/unparseable JSON returns HTTP 400 Bad Request._
- **C-1101** — code: Returns HTTP status 404 if no vehicle with the specified NAME exists. (`VehicleHandler.java:244-245`)  
  doc (vehicles.yaml:258): PUT /vehicles/{NAME}/allowedOrderTypes returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by C-1101: returns HTTP 404 if no vehicle with the specified NAME exists._
- **C-0031** — code: Updating a vehicle's allowed order types accepts a list of order type names. (`PutVehicleAllowedOrderTypesTO.java:11-24`)  
  doc (vehicles.yaml:238): PUT /vehicles/{NAME}/allowedOrderTypes request body → AllowedOrderTypes". The list of all order types to be allowed.  
  _Supported by C-0031 and C-1100: accepts a list of order type names in the request body._
- **C-1200** — code: Updates the acceptable order types and their priorities for the vehicle identified by the NAME path parameter using the provided request body, returning an empty text response. (`V1RequestHandler.java:186-189`)  
  doc (vehicles.yaml:288): PUT /vehicles/{NAME}/acceptableOrderTypes returns 200: Successful operation  
  _Supported by C-1200: PUT /vehicles/{NAME}/acceptableOrderTypes returns HTTP 200 on success._
- **C-0001** — code: When an operation encounters an IllegalArgumentException, the Web API responds with HTTP 400 Bad Request and serializes the exception. (`ServiceWebApi.java:175-178`)  
  doc (vehicles.yaml:290): PUT /vehicles/{NAME}/acceptableOrderTypes returns 400: The submitted data is invalid. (example: Could not parse JSON input.)  
  _Claim C-0001 verifies that invalid request data / IllegalArgumentException causes HTTP 400 Bad Request._
- **C-1201** — code: Returns HTTP 404 if no vehicle with the given NAME exists. (`VehicleHandler.java:244-245`)  
  doc (vehicles.yaml:300): PUT /vehicles/{NAME}/acceptableOrderTypes returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Claim C-1201 confirms that PUT acceptableOrderTypes returns 404 when the vehicle does not exist._
- **C-1300** — code: Updates the energy level threshold set of the vehicle identified by the NAME path parameter using the provided critical, good, sufficiently recharged, and fully recharged threshold values. (`V1RequestHandler.java:190-193`)  
  doc (vehicles.yaml:330): PUT /vehicles/{NAME}/energyLevelThresholdSet returns 200: Successful operation  
  _Claim C-1300 confirms successful updating of vehicle energy level threshold set returning 200._
- **C-1301** — code: Rejects the request with HTTP status 400 Bad Request if any of energyLevelCritical, energyLevelGood, energyLevelSufficientlyRecharged, or energyLevelFullyRecharged is not between 0 and 100 inclusive. (`Vehicle.java:1537-1560`)  
  doc (vehicles.yaml:332): PUT /vehicles/{NAME}/energyLevelThresholdSet returns 400: The submitted data is invalid. (example: Could not parse JSON input.)  
  _Claim C-1301 and C-0001 confirm returning 400 when invalid input is submitted for energyLevelThresholdSet._
- **C-1302** — code: Returns HTTP status 404 Not Found if no vehicle exists with the specified NAME. (`VehicleHandler.java:270-271`)  
  doc (vehicles.yaml:342): PUT /vehicles/{NAME}/energyLevelThresholdSet returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Claim C-1302 confirms PUT energyLevelThresholdSet returns 404 when the vehicle is not found._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (vehicles.yaml:322): PUT /vehicles/{NAME}/energyLevelThresholdSet request body → EnergyLevelThresholdSet". The new set of energy level thresholds.  
  _Claims C-0032 and C-1300 confirm the EnergyLevelThresholdSet request body for updating thresholds._
- **C-0400** — code: Gets communication adapter attachment information for the vehicle specified by NAME, returning vehicle name, available adapter class names, and attached adapter class name. (`V1RequestHandler.java:175-178`)  
  doc (vehicles.yaml:358): GET /vehicles/{NAME}/commAdapter/attachmentInformation: path parameter 'NAME'; required; type string. The name of the vehicle.  
  _Claim C-0400 confirms GET commAdapter/attachmentInformation takes path parameter NAME for the vehicle name._
- **C-0400** — code: Gets communication adapter attachment information for the vehicle specified by NAME, returning vehicle name, available adapter class names, and attached adapter class name. (`V1RequestHandler.java:175-178`)  
  doc (vehicles.yaml:366): GET /vehicles/{NAME}/commAdapter/attachmentInformation returns 200: Successful operation  
  _Claim C-0400 confirms successful operation returning HTTP 200._
- **C-0402** — code: Returns HTTP 404 with error details if the specified vehicle does not exist. (`VehicleHandler.java:184-185`)  
  doc (vehicles.yaml:372): GET /vehicles/{NAME}/commAdapter/attachmentInformation returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Claim C-0402 confirms returning 404 if the vehicle does not exist._
- **C-0300** — code: Attaches a communication adapter to the vehicle identified by the NAME path parameter using the driver class specified by the newValue query parameter, returning an empty text/plain response. (`V1RequestHandler.java:171-174`)  
  doc (vehicles.yaml:385): PUT /vehicles/{NAME}/commAdapter/attachment — Attaches the given vehicle driver to this vehicle.  
  _Claim C-0300 confirms attaching the given vehicle driver to this vehicle._
- **C-0300** — code: Attaches a communication adapter to the vehicle identified by the NAME path parameter using the driver class specified by the newValue query parameter, returning an empty text/plain response. (`V1RequestHandler.java:171-174`)  
  doc (vehicles.yaml:388): PUT /vehicles/{NAME}/commAdapter/attachment: path parameter 'NAME'; required; type string. The name of the vehicle.  
  _Claim C-0300 confirms path parameter NAME identifies the vehicle._
- **C-0300** — code: Attaches a communication adapter to the vehicle identified by the NAME path parameter using the driver class specified by the newValue query parameter, returning an empty text/plain response. (`V1RequestHandler.java:171-174`)  
  doc (vehicles.yaml:403): PUT /vehicles/{NAME}/commAdapter/attachment returns 200: Successful operation  
  _Claim C-0300 confirms PUT /vehicles/{NAME}/commAdapter/attachment succeeds with 200._
- **C-0301** — code: Returns HTTP 404 (Not Found) if no vehicle with the specified name exists. (`VehicleHandler.java:197-198`)  
  doc (vehicles.yaml:405): PUT /vehicles/{NAME}/commAdapter/attachment returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by C-0301 which verifies that PUT /vehicles/{NAME}/commAdapter/attachment returns HTTP 404 if no vehicle with the specified name exists._
- **C-0302** — code: Returns HTTP 400 (Bad Request) if the driver class specified by newValue is not among the available communication adapters for the vehicle. (`VehicleHandler.java:204-208`)  
  doc (vehicles.yaml:415): PUT /vehicles/{NAME}/commAdapter/attachment returns 400: The submitted value is invalid. (example: "Unknown vehicle driver class name: org.opentcs.someVehicle.driver11")  
  _Supported by C-0302 which confirms returning HTTP 400 Bad Request when the driver class specified by newValue is not among the available communication adapters._
- **C-0500** — code: Enables or disables the communication adapter of the vehicle specified by path parameter NAME according to boolean query parameter newValue. (`V1RequestHandler.java:179-179`)  
  doc (vehicles.yaml:428): PUT /vehicles/{NAME}/commAdapter/enabled — Sets the enabled state for the named vehicle's driver.  
  _Supported by C-0500 which verifies that PUT /vehicles/{NAME}/commAdapter/enabled enables or disables the communication adapter of the vehicle specified by NAME._
- **C-0500** — code: Enables or disables the communication adapter of the vehicle specified by path parameter NAME according to boolean query parameter newValue. (`V1RequestHandler.java:179-179`)  
  doc (vehicles.yaml:431): PUT /vehicles/{NAME}/commAdapter/enabled: path parameter 'NAME'; required; type string. The name of the vehicle.  
  _Supported by C-0500 which verifies the path parameter NAME identifying the vehicle._
- **C-0500** — code: Enables or disables the communication adapter of the vehicle specified by path parameter NAME according to boolean query parameter newValue. (`V1RequestHandler.java:179-179`)  
  doc (vehicles.yaml:446): PUT /vehicles/{NAME}/commAdapter/enabled returns 200: Successful operation  
  _Supported by C-0500 which confirms successful operation execution._
- **C-0501** — code: Responds with HTTP status 404 if no vehicle with the specified name exists. (`VehicleHandler.java:167-168`)  
  doc (vehicles.yaml:448): PUT /vehicles/{NAME}/commAdapter/enabled returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by C-0501 which verifies that PUT /vehicles/{NAME}/commAdapter/enabled returns HTTP 404 if no vehicle with the specified name exists._
- **C-0600** — code: Sends a message to the communication adapter of the vehicle specified by NAME using the request body. (`V1RequestHandler.java:180-180`)  
  doc (vehicles.yaml:478): POST /vehicles/{NAME}/commAdapter/message returns 200: Successful operation  
  _Supported by C-0600 which verifies successful operation when sending a message to a vehicle comm adapter._
- **C-0601** — code: Returns HTTP 404 if no vehicle with the given NAME exists. (`VehicleHandler.java:222-223`)  
  doc (vehicles.yaml:480): POST /vehicles/{NAME}/commAdapter/message returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by C-0601 which verifies that POST /vehicles/{NAME}/commAdapter/message returns HTTP 404 if no vehicle with the given NAME exists._
- **C-0200** — code: Computes candidate routes from a source point to one or more destination points for the vehicle specified by the path parameter NAME. (`V1RequestHandler.java:170-170`)  
  doc (vehicles.yaml:495): POST /vehicles/{NAME}/routeComputationQuery: path parameter 'NAME'; required; type string. The name of the vehicle.  
  _Supported by C-0200 which verifies the NAME path parameter specifying the vehicle for route computation._
- **C-0200** — code: Computes candidate routes from a source point to one or more destination points for the vehicle specified by the path parameter NAME. (`V1RequestHandler.java:170-170`)  
  doc (vehicles.yaml:518): POST /vehicles/{NAME}/routeComputationQuery returns 200: Successful operation  
  _Supported by C-0200 which verifies POST /vehicles/{NAME}/routeComputationQuery computes routes and returns a successful response._
- **C-0208** — code: Responds with HTTP status 404 if the specified sourcePoint does not exist in the plant model. (`VehicleHandler.java:306-311`)  
  doc (vehicles.yaml:534): POST /vehicles/{NAME}/routeComputationQuery returns 404: Referencing object that could not be found. (example: "Unknown source point: Point-X")  
  _Supported by C-0208, C-0209, and C-0210 which confirm returning HTTP 404 when referenced points or resources cannot be found._
- **C-1400** — code: Updates the envelope key of the vehicle identified by NAME to the value specified in the newValue query parameter. (`V1RequestHandler.java:194-194`)  
  doc (vehicles.yaml:547): PUT /vehicles/{NAME}/envelopeKey — Sets the envelope key for this vehicle.  
  _Supported by C-1400 which verifies that PUT /vehicles/{NAME}/envelopeKey updates the envelope key for the specified vehicle._
- **C-1400** — code: Updates the envelope key of the vehicle identified by NAME to the value specified in the newValue query parameter. (`V1RequestHandler.java:194-194`)  
  doc (vehicles.yaml:550): PUT /vehicles/{NAME}/envelopeKey: path parameter 'NAME'; required; type string. The name of the vehicle.  
  _Supported by C-1400 which updates the envelope key of the vehicle identified by NAME._
- **C-1400** — code: Updates the envelope key of the vehicle identified by NAME to the value specified in the newValue query parameter. (`V1RequestHandler.java:194-194`)  
  doc (vehicles.yaml:565): PUT /vehicles/{NAME}/envelopeKey returns 200: Successful operation  
  _PUT /vehicles/{NAME}/envelopeKey successfully updates the vehicle envelope key, returning HTTP 200._
- **C-1401** — code: Responds with HTTP status 404 if no vehicle with the specified NAME exists. (`VehicleHandler.java:153-154`)  
  doc (vehicles.yaml:567): PUT /vehicles/{NAME}/envelopeKey returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _C-1401 confirms that HTTP 404 is returned if no vehicle with the specified NAME exists._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:595): vehicles:VehicleState.name; required; type string. The name of the vehicle  
  _C-0034 confirms that the vehicle representation includes the vehicle's name._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:599): vehicles:VehicleState.properties; required; type object. A set of properties (key-value pairs) associated with this object.  
  _C-0034 confirms that the vehicle state includes properties (key-value pairs)._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (vehicles.yaml:604): vehicles:VehicleState.length; required; type integer. The vehicle's length (in mm).  
  _C-0026 confirms that a vehicle's bounding box and vehicle geometry defines length._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (vehicles.yaml:609): vehicles:VehicleState.boundingBox; required  
  _C-0026 and C-0034 confirm that boundingBox is present in the vehicle state transfer object._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (vehicles.yaml:611): vehicles:VehicleState.energyLevelGood; required; type integer. The value (in %) at/above which the vehicle's energy level is considered 'good'.  
  _C-0032 and C-0034 confirm the presence and meaning of energyLevelGood._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (vehicles.yaml:616): vehicles:VehicleState.energyLevelCritical; required; type integer. The value (in %) at/below which the vehicle's energy level is considered 'critical'.  
  _C-0032 and C-0034 confirm the presence and meaning of energyLevelCritical._
- **C-0032** — code: Updating vehicle energy level thresholds sets critical, good, sufficiently recharged, and fully recharged percentages. (`PutVehicleEnergyLevelThresholdSetTO.java:9-23`)  
  doc (vehicles.yaml:621): vehicles:VehicleState.energyLevelSufficientlyRecharged; required; type integer. The value (in %) at/below which the vehicle's energy level is considered 'sufficiently recharged'.  
  _C-0032 and C-0034 confirm the presence and meaning of energyLevelSufficientlyRecharged._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:631): vehicles:VehicleState.energyLevel; required; type integer. The vehicle's remaining energy (in %).  
  _C-0034 confirms that retrieving a vehicle returns its current energyLevel percentage._
- **C-0012** — code: A vehicle integration level defines whether the vehicle is ignored (TO_BE_IGNORED), noticed without reserving resources (TO_BE_NOTICED), respected by reserving resources for reported positions (TO_BE_RESPECTED), or utilized by the dispatcher for order assignments (TO_BE_UTILIZED). (`Vehicle.java:1396-1415`)  
  doc (vehicles.yaml:635): vehicles:VehicleState.integrationLevel; required; type string. The vehicle's integration level.  
  _C-0012 and C-0034 confirm that the vehicle's integration level is reported as part of VehicleState._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:643): vehicles:VehicleState.paused; required; type boolean. Whether the vehicle is paused.  
  _Supported by C-0034, which confirms vehicle state retrieval returns the vehicle's pause status._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:647): vehicles:VehicleState.procState; required; type string. The vehicle's current processing state.  
  _Supported by C-0034, which confirms vehicle state retrieval returns the processing state._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:655): vehicles:VehicleState.procStateTimestamp; required; type string; format date-time. The time at which the vehicle has entered its current state (expressed according to ISO 8601).  
  _Supported by C-0034, which confirms vehicle state retrieval returns state timestamps._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:660): vehicles:VehicleState.transportOrder; optional; type string. The name of the transport order the vehicle is currently processing.  
  _Supported by C-0034, which confirms vehicle state retrieval returns the assigned transport order._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:664): vehicles:VehicleState.currentPosition; optional; type string. The name of the point which the vehicle currently occupies.  
  _Supported by C-0034, which confirms vehicle state retrieval returns the current position._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:668): vehicles:VehicleState.precisePosition; optional  
  _Supported by C-0034, which confirms vehicle state retrieval returns precise 3D position coordinates._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:670): vehicles:VehicleState.orientationAngle; required. The vehicle's current orientation angle (-360..360). May be a string ("NaN") if the vehicle hasn't provided an orientation angle.  
  _Supported by C-0034, which confirms vehicle state retrieval returns the orientation angle._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:679): vehicles:VehicleState.state; required; type string. The vehicle's current state.  
  _Supported by C-0034, which confirms vehicle state retrieval returns the vehicle state._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:689): vehicles:VehicleState.stateTimestamp; required; type string; format date-time. The time at which the vehicle has entered its current processing state (expressed according to ISO 8601).  
  _Supported by C-0034, which confirms vehicle state retrieval returns state timestamps._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:694): vehicles:VehicleState.allocatedResources; required; type array. The resources already allocated by the vehicle.  
  _Supported by C-0034, which confirms vehicle state retrieval returns allocated resources._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:700): vehicles:VehicleState.claimedResources; required; type array. The resources claimed - i.e. not yet allocated - for the vehicle's route.  
  _Supported by C-0034, which confirms vehicle state retrieval returns claimed resources._
- **C-0031** — code: Updating a vehicle's allowed order types accepts a list of order type names. (`PutVehicleAllowedOrderTypesTO.java:11-24`)  
  doc (vehicles.yaml:706): vehicles:VehicleState.allowedOrderTypes; required; type array. Deprecated and will be removed in favor of the `acceptableOrderTypes` property.  
  _Supported by C-0031 (updating allowed order types) and C-0034/C-0030 (acceptable order types replacing allowed order types)._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (vehicles.yaml:720): vehicles:VehicleState.envelopeKey; optional; type string. The envelope key for this vehicle.  
  _Claim C-0034 confirms that retrieving a vehicle returns its envelope key._
- **C-0031** — code: Updating a vehicle's allowed order types accepts a list of order type names. (`PutVehicleAllowedOrderTypesTO.java:11-24`)  
  doc (vehicles.yaml:749): vehicles:AllowedOrderTypes.orderTypes; required; type array  
  _Claim C-0031 confirms updating a vehicle's allowed order types accepts a list of order type names._
- **C-0028** — code: The vehicle attachment info response reports the vehicle name, the currently attached communication adapter, and the list of available communication adapters. (`GetVehicleAttachmentInfoResponseTO.java:10-23`)  
  doc (vehicles.yaml:816): vehicles:AttachmentInformation.vehicleName; required; type string. The name of the vehicle.  
  _Claims C-0028 and C-0400 confirm the attachment info response reports vehicleName._
- **C-0028** — code: The vehicle attachment info response reports the vehicle name, the currently attached communication adapter, and the list of available communication adapters. (`GetVehicleAttachmentInfoResponseTO.java:10-23`)  
  doc (vehicles.yaml:820): vehicles:AttachmentInformation.availableCommAdapters; required; type array. The list of drivers (as names of description classes) available for this vehicle.  
  _Claims C-0028 and C-0400 confirm the attachment info response reports the list of available communication adapters._
- **C-0201** — code: Accepts a JSON request body containing a required list of destination points and optional source point and resources to avoid. (`V1RequestHandler.java:326-345`)  
  doc (vehicles.yaml:838): vehicles:RoutesRequest.sourcePoint; optional; type string. The (optional) starting point for route computation. If `null` or not set, the vehicle's current position will be used.  
  _Claims C-0201 and C-0206 confirm sourcePoint is optional in RoutesRequest and defaults to the vehicle's current position if null/omitted._
- **C-0201** — code: Accepts a JSON request body containing a required list of destination points and optional source point and resources to avoid. (`V1RequestHandler.java:326-345`)  
  doc (vehicles.yaml:844): vehicles:RoutesRequest.destinationPoints; required; type array. The destination point for each route to be computed.  
  _Claim C-0201 confirms that destinationPoints is a required list in the JSON request body._
- **C-0201** — code: Accepts a JSON request body containing a required list of destination points and optional source point and resources to avoid. (`V1RequestHandler.java:326-345`)  
  doc (vehicles.yaml:853): vehicles:RoutesRequest.resourcesToAvoid; optional; type array. The resources to be avoided for each route.  
  _Claim C-0201 confirms that resourcesToAvoid is an optional list in the routes request._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:867): vehicles:RoutesResponse.routes; optional; type array. The list of computed routes.  
  _Claim C-0033 confirms that route calculation returns routes containing computed route data._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:905): vehicles:Route.destinationPoint; required; type string. The computed route's destination point.  
  _Claim C-0033 confirms each computed route includes the destination point._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:909): vehicles:Route.costs; required; type integer; format int64. The costs for the computed route, or `-1`, if no route could be computed.  
  _Claim C-0033 confirms each computed route includes total costs (or -1 if no route)._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:914): vehicles:Route.steps; required; type array. An array containing the computed route's steps, or `null`, if no route could be computed.  
  _Claim C-0033 confirms each computed route contains steps (or null if no route)._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:929): vehicles:Step.path; optional; type string. The path to travel for this step.  
  _Claim C-0033 confirms route steps report the path to travel._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:933): vehicles:Step.sourcePoint; optional; type string. The source point for this step.  
  _Supported by C-0033 which states that route steps include source point._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:937): vehicles:Step.destinationPoint; required; type string. The destination point for this step.  
  _Supported by C-0033 which states that route steps include destination point._
- **C-0015** — code: A vehicle's orientation on a route step or in the model is reported as FORWARD, BACKWARD, or UNDEFINED. (`Vehicle.java:1441-1457`)  
  doc (vehicles.yaml:941): vehicles:Step.vehicleOrientation; required; type string; default UNDEFINED  
  _Supported by C-0015 and C-0033 covering vehicle orientation on route steps._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (common.yaml:58): common:PrecisePosition.x; required; type integer. The position's X coordinate  
  _Supported by C-0034 covering precise 3D position coordinates._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (common.yaml:62): common:PrecisePosition.y; required; type integer. The position's Y coordinate  
  _Supported by C-0034 covering precise 3D position coordinates._
- **C-0034** — code: Retrieving a vehicle returns its name, state, processing state, energy levels, integration level, current position, precise 3D position coordinates, orientation angle, bounding box, assigned transport order, allocated and claimed resources, pause status, envelope key, acceptable order types, properties, and state timestamps. (`GetVehicleResponseTO.java:23-145`)  
  doc (common.yaml:66): common:PrecisePosition.z; required; type integer. The position's Z coordinate  
  _Supported by C-0034 covering precise 3D position coordinates._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (common.yaml:78): common:DestinationState.locationName; required; type string. The name of the destination location  
  _Supported by C-0009 which states destination state reports target location name._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (common.yaml:82): common:DestinationState.operation; required; type string. The destination operation  
  _Supported by C-0009 which states destination state reports the operation to be performed._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (common.yaml:86): common:DestinationState.state; required; type string. The drive order's state  
  _Supported by C-0009 and C-0010 covering destination state and drive order states._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (common.yaml:96): common:DestinationState.properties; optional; type array. The drive order's properties  
  _Supported by C-0009 which reports key-value properties for destination state._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (common.yaml:112): common:PeripheralJobState.name; required; type string. The name of the peripheral job.  
  _Supported by C-0043 covering peripheral job status messages and specifications._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (common.yaml:116): common:PeripheralJobState.reservationToken; required; type string. A token that may be used to reserve a peripheral device. A peripheral device that is reserved for a specific token can only process jobs which match that reservation token.  
  _Supported by C-0042 and C-0043 which define reservation tokens for peripheral jobs and devices._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (common.yaml:122): common:PeripheralJobState.relatedVehicle; required; type string. The name of the vehicle for which the peripheral job was created. May be `null`, if the job wasn't created in the context of a transport order being processed by a vehicle.  
  _Supported by C-0043 describing PeripheralJobState reporting relatedVehicle which may be null._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (common.yaml:128): common:PeripheralJobState.relatedTransportOrder; required; type string. The name of the transport order for which the peripheral job was created. May be `null`, if the job wasn't created in the context of a transport order being processed by a vehicle.  
  _Supported by C-0043 describing PeripheralJobState reporting relatedTransportOrder which may be null._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (common.yaml:134): common:PeripheralJobState.peripheralOperation; required  
  _Supported by C-0043 describing PeripheralJobState reporting peripheralOperation specifications._
- **C-0017** — code: A peripheral job lifecycle is represented by the states TO_BE_PROCESSED, BEING_PROCESSED, FINISHED, and FAILED. (`PeripheralJob.java:420-438`)  
  doc (common.yaml:136): common:PeripheralJobState.state; required; type string. The peripheral job's current state.  
  _Supported by C-0017 describing peripheral job states TO_BE_PROCESSED, BEING_PROCESSED, FINISHED, and FAILED._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (common.yaml:144): common:PeripheralJobState.creationTime; required; type string; format date-time. The point of time at which this peripheral job was created (expressed according to ISO 8601).  
  _Supported by C-0043 describing PeripheralJobState reporting creationTime._
- **C-0043** — code: Peripheral job status messages report job creation/completion timestamps, related vehicle and order names, reservation tokens, and operation specifications including completion requirements. (`PeripheralJobStatusMessage.java:13-30`)  
  doc (common.yaml:149): common:PeripheralJobState.finishedTime; required; type string; format date-time. The point of time at which processing of this peripheral job was finished (expressed according to ISO 8601).  
  _Supported by C-0043 describing PeripheralJobState reporting completion/finishedTime._
- **C-0027** — code: A generic Property object defines an arbitrary key-value string pair. (`Property.java:10-26`)  
  doc (common.yaml:154): common:PeripheralJobState.properties; optional; type array. The peripheral job's properties.  
  _Supported by C-0027 describing key-value properties representation on model transfer objects._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (common.yaml:232): common:Couple.x; required; type integer; format int64. The Couple's x value.  
  _Supported by C-0026 describing the Couple x coordinate in vehicle transfer objects._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (common.yaml:237): common:Couple.y; required; type integer; format int64. The Couple's y value.  
  _Supported by C-0026 describing the Couple y coordinate in vehicle transfer objects._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (common.yaml:280): common:ObjectHistoryEntry.timestamp; required; type string; format date-time  
  _Supported by C-0037 describing ObjectHistoryEntry timestamp._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:282): environmental-entities:EnvironmentalEntityEnvelope.vertices; required; type array. The sequence of vertices this envelope consists of.  
  _Supported by C-0038 describing environmental entity envelopes and polygon vertices._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:299): environmental-entities:EnvironmentalEntityPose.position; required  
  _EnvironmentalEntityPose.position is part of the pose structure confirmed in C-0038._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (environmental-entities.yaml:303): environmental-entities:EnvironmentalEntityPose.orientationAngle; required; type number  
  _EnvironmentalEntityPose.orientationAngle is part of the entity pose structure confirmed in C-0038._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (plant-model.yaml:491): plant-model:PlantModelPath.peripheralOperations[].operation; required; type string  
  _Peripheral operation name/operation specification matches peripheral operations model and status specifications._
- **C-0009** — code: A destination state in a transport order reports its target location name, the operation to be performed, its current state, and key-value properties. (`DestinationState.java:16-25`)  
  doc (plant-model.yaml:494): plant-model:PlantModelPath.peripheralOperations[].locationName; required; type string  
  _Location name on peripheral operations matches peripheral destination operation structure._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (plant-model.yaml:497): plant-model:PlantModelPath.peripheralOperations[].executionTrigger; optional; type string  
  _Peripheral operation executionTrigger optional field is confirmed by execution trigger specifications in C-0018._
- **C-0042** — code: Peripheral job SSE events stream updates for peripheral operations, indicating completion requirements, execution triggers, related vehicles, transport orders, and reservation tokens. (`PeripheralJobEventTO.java:14-79`)  
  doc (plant-model.yaml:502): plant-model:PlantModelPath.peripheralOperations[].completionRequired; optional; type boolean  
  _CompletionRequired boolean field is confirmed by peripheral job / operation structure in C-0042 and C-0043._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (plant-model.yaml:747): plant-model:PlantModelVehicle.layout.routeColor; optional; type string  
  _Layout routeColor optional property is confirmed in C-0036._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:186): sse:VehicleEvent.currentObjectState; optional  
  _SSE VehicleEvent.currentObjectState is confirmed in C-0036._
- **C-0036** — code: Vehicle SSE events deliver the previous and current object state of a vehicle, including pose (position coordinates and orientation angle), bounding box, velocity limits, layout route colors, load handling device fullness, energy thresholds, and assigned transport order or order sequence. (`VehicleEventTO.java:42-120`)  
  doc (sse.yaml:188): sse:VehicleEvent.previousObjectState; optional  
  _SSE VehicleEvent.previousObjectState is confirmed in C-0036._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:247): sse:EnvironmentalEntity.name; required; type string  
  _EnvironmentalEntity name is required string in entity SSE events/state representation (C-0038)._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:249): sse:EnvironmentalEntity.properties; required; type object  
  _EnvironmentalEntity properties object is confirmed in C-0038._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (sse.yaml:253): sse:EnvironmentalEntity.history; required  
  _Supported by C-0037, which establishes the history object transfer structure for model entities._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:255): sse:EnvironmentalEntity.envelope; required  
  _Supported by C-0038, which states environmental entity SSE events include envelope polygon vertices._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:257): sse:EnvironmentalEntity.pose; required  
  _Supported by C-0038, which specifies environmental entity SSE events contain pose information._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:259): sse:EnvironmentalEntity.type; required; type string. This entity's type.  
  _Supported by C-0038, which specifies environmental entity SSE events publish entity type._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:259): sse:EnvironmentalEntity.type is one of: OBJECT, ZONE. This entity's type.  
  _Supported by C-0038, which defines environmental entity type as OBJECT or ZONE._
- **C-0011** — code: An environmental entity integration level indicates whether the entity is ignored (TO_BE_IGNORED), displayed without impacting traffic management (TO_BE_NOTICED), or accounted for during traffic management (TO_BE_RESPECTED). (`EnvironmentalEntity.java:458-471`)  
  doc (sse.yaml:265): sse:EnvironmentalEntity.integrationLevel; required; type string. The entity's integration level.  
  _Supported by C-0011 and C-0038, which establish environmental entity integration levels._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:279): sse:EnvironmentalEntity.createdTime; required; type string; format date-time  
  _Supported by C-0038, which confirms environmental entity SSE events publish creation timestamps._
- **C-0038** — code: Environmental entity SSE events publish previous and current states of entities (typed as OBJECT or ZONE), with pose, envelope polygon vertices, creation and retirement timestamps, integration level, and properties. (`EnvironmentalEntityEventTO.java:18-112`)  
  doc (sse.yaml:282): sse:EnvironmentalEntity.retiredTime; required; type string; format date-time  
  _Supported by C-0038, which confirms environmental entity SSE events publish retirement timestamps._
- **C-0028** — code: The vehicle attachment info response reports the vehicle name, the currently attached communication adapter, and the list of available communication adapters. (`GetVehicleAttachmentInfoResponseTO.java:10-23`)  
  doc (vehicles.yaml:600): vehicles:AttachmentInformation.vehicleName; required; type string. The name of the vehicle.  
  _Supported by C-0028 and C-0400, which specify the attachment info response reports vehicleName._
- **C-0028** — code: The vehicle attachment info response reports the vehicle name, the currently attached communication adapter, and the list of available communication adapters. (`GetVehicleAttachmentInfoResponseTO.java:10-23`)  
  doc (vehicles.yaml:604): vehicles:AttachmentInformation.availableCommAdapters; required; type array. The list of drivers (as names of description classes) available for this vehicle.  
  _Supported by C-0028 and C-0400, which confirm availableCommAdapters is returned as a list of available driver class names._
- **C-0206** — code: Defaults the source point to the vehicle's current position if no sourcePoint is provided in the request body. (`VehicleHandler.java:299-304`)  
  doc (vehicles.yaml:622): vehicles:RoutesRequest.sourcePoint; optional; type string. The (optional) starting point for route computation. If `null` or not set, the vehicle's current position will be used.  
  _Supported by C-0201 and C-0206: sourcePoint is optional and defaults to the vehicle's current position if null or not set._
- **C-0201** — code: Accepts a JSON request body containing a required list of destination points and optional source point and resources to avoid. (`V1RequestHandler.java:326-345`)  
  doc (vehicles.yaml:628): vehicles:RoutesRequest.destinationPoints; required; type array. The destination point for each route to be computed.  
  _Supported by C-0201: destinationPoints is a required list of destination points for route computation._
- **C-0201** — code: Accepts a JSON request body containing a required list of destination points and optional source point and resources to avoid. (`V1RequestHandler.java:326-345`)  
  doc (vehicles.yaml:637): vehicles:RoutesRequest.resourcesToAvoid; optional; type array. The resources to be avoided for each route.  
  _Supported by C-0201 and C-0033: resourcesToAvoid is an optional list of resources to avoid._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:651): vehicles:RoutesResponse.routes; optional; type array. The list of computed routes.  
  _Supported by C-0033: returns computed routes containing steps and costs._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:689): vehicles:Route.destinationPoint; required; type string. The computed route's destination point.  
  _Supported by C-0033: computed route includes destination point._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:693): vehicles:Route.costs; required; type integer; format int64. The costs for the computed route, or `-1`, if no route could be computed.  
  _Supported by C-0033: Route object includes costs (int64) for the computed route._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:698): vehicles:Route.steps; required; type array; nullable. An array containing the computed route's steps, or `null`, if no route could be computed.  
  _Supported by C-0033: Route contains steps, or null if no route could be computed._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:712): vehicles:Step.path; required; type string; nullable. The path to travel for this step.  
  _Supported by C-0033: Step includes path to travel._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:717): vehicles:Step.sourcePoint; required; type string; nullable. The source point for this step.  
  _Supported by C-0033: Step includes source point._
- **C-0033** — code: Vehicle route calculation computes routes from a source point to destinations avoiding specified resources, returning steps with source, path, destination, orientation, and total costs. (`PostVehicleRoutesRequestTO.java:13-26`)  
  doc (vehicles.yaml:722): vehicles:Step.destinationPoint; required; type string. The destination point for this step.  
  _Supported by C-0033: Step includes destination point._
- **C-0015** — code: A vehicle's orientation on a route step or in the model is reported as FORWARD, BACKWARD, or UNDEFINED. (`Vehicle.java:1441-1457`)  
  doc (vehicles.yaml:726): vehicles:Step.vehicleOrientation; required; type string; default UNDEFINED  
  _Supported by C-0015 and C-0033: Step specifies vehicleOrientation defaulting to UNDEFINED._
- **C-0029** — code: Posting a message to a vehicle communication adapter accepts a message type and a list of key-value parameters. (`PostVehicleCommAdapterMessageRequestTO.java:13-26`)  
  doc (vehicles.yaml:742): vehicles:VehicleCommAdapterMessage.type; required; type string. The message's type  
  _Supported by C-0029: VehicleCommAdapterMessage requires a message type._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (common.yaml:51): common:Couple.x; required; type integer  
  _Supported by C-0026 which defines vehicle bounding box offset couple (x, y)._
- **C-0026** — code: A vehicle bounding box transfer object defines length, width, and height dimensions along with an (x, y) reference offset couple. (`BoundingBoxTO.java:10-25`)  
  doc (common.yaml:53): common:Couple.y; required; type integer  
  _Supported by C-0026 which defines vehicle bounding box offset couple (x, y)._
- **C-0037** — code: Object history transfers a sequence of chronological entries comprising event codes, supplementary details, and event timestamps. (`ObjectHistoryTO.java:12-36`)  
  doc (common.yaml:70): common:ObjectHistoryEntry.timestamp; required; type string; format date-time  
  _Supported by C-0037 which confirms ObjectHistoryEntry timestamp._
- **C-0016** — code: A transport order lifecycle transitions through RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, and UNROUTABLE states. (`TransportOrder.java:990-1033`)  
  doc (developers-guide-03_reference.adoc:302): Reference › System requirements › Transport orders in openTCS › A transport order's life cycle: If the vehicle successfully processes the transport order as a whole, it is marked as `FINISHED`.  
  _Supported by C-0016 regarding transport order lifecycle transitioning to FINISHED._
- **C-0018** — code: A peripheral operation execution trigger specifies whether the operation triggers IMMEDIATE, AFTER_ALLOCATION, or AFTER_MOVEMENT. (`PeripheralOperationTO.java:83-87`)  
  doc (users-guide-01_tutorials.adoc:308): Tutorials › Constructing a new plant model › Operating the plant › Creating a peripheral job › Implicit creation of peripheral jobs: `AFTER_MOVEMENT`: The peripheral job is created as soon as the vehicle has completely traversed the path and reported the corresponding movement command as executed (i  
  _Supported by C-0018 regarding AFTER_MOVEMENT peripheral operation execution trigger._
- **C-0016** — code: A transport order lifecycle transitions through RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, and UNROUTABLE states. (`TransportOrder.java:990-1033`)  
  doc (users-guide-03_reference.adoc:545): Reference › Supported navigation principles / vehicle types › Default strategies › Default dispatcher › Immediate transport order assignment: The transport order's state must be `DISPATCHABLE`.  
  _Statement describes the required DISPATCHABLE state for immediate transport order assignment, consistent with the transport order lifecycle defined in C-0016._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (users-guide-03_reference.adoc:552): Reference › Supported navigation principles / vehicle types › Default strategies › Default dispatcher › Immediate transport order assignment: The vehicle's state must be `IDLE` or `CHARGING`.  
  _Statement describes the required IDLE or CHARGING states for vehicle order assignment, consistent with vehicle operational states in C-0013._
- **C-0006** — code: A Block type can be SINGLE_VEHICLE_ONLY, restricting resource usage in the block to one vehicle at a time, or SAME_DIRECTION_ONLY, permitting multiple vehicles only if they travel through the block in the same direction. (`Block.java:204-215`)  
  doc (users-guide-03_reference.adoc:625): Reference › Supported navigation principles / vehicle types › Default strategies › Default scheduler › Allocating resources: Check if the requested resources are part of a block with the type `SINGLE_VEHICLE_ONLY`. If not, skip this check. If yes, expand the requested resource set to the effective r  
  _Statement describes resource allocation behavior for SINGLE_VEHICLE_ONLY blocks, consistent with block semantics in C-0006._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (users-guide-03_reference.adoc:668): Reference › Supported navigation principles / vehicle types › Default strategies › Default peripheral job dispatcher: A peripheral device's state must be `IDLE`.  
  _Statement requires IDLE state for peripheral job assignment, consistent with state definitions in C-0013._
- **C-0014** — code: A vehicle's processing state is IDLE when no order is assigned, AWAITING_ORDER when assigned to a transport order but awaiting the next drive order, or PROCESSING_ORDER when actively executing a drive order. (`Vehicle.java:1420-1435`)  
  doc (users-guide-03_reference.adoc:669): Reference › Supported navigation principles / vehicle types › Default strategies › Default peripheral job dispatcher: A peripheral device's processing state must be `IDLE`.  
  _Statement requires IDLE processing state for peripheral job assignment, consistent with processing state definitions in C-0014._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-1.1-03-runtime-behavior.adoc:36): Runtime behaviour › Topics › Topic `connection`: Upon receiving a connection state of `ONLINE`, the driver will report the vehicle's state as `IDLE` to the kernel.  
  _Statement indicates reporting vehicle state as IDLE upon receiving connection state ONLINE, consistent with vehicle state IDLE defined in C-0013._
- **C-0027** — code: A generic Property object defines an arbitrary key-value string pair. (`Property.java:10-26`)  
  doc (vda5050-1.1-03-runtime-behavior.adoc:103): Runtime behaviour › Topics › Topic `state` › Mapping of state information to vehicle properties: The property with the key `vda5050:information.info` will contain a concatenated list of ``infoType``s of all `info` elements with an `infoLevel` of `INFO`.  
  _Statement describes setting vehicle properties key-value pair vda5050:information.info, consistent with general property key-value pair support in C-0027._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-1.1-03-runtime-behavior.adoc:110): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `ERROR`: Set if the vehicle reports at least one `error` with an `errorLevel` of `FATAL`.  
  _Statement specifies mapping fatal errors to vehicle state ERROR, consistent with vehicle state ERROR in C-0013._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-1.1-03-runtime-behavior.adoc:111): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `UNAVAILABLE`: Set if the vehicle reports an `operatingMode` other than `AUTOMATIC` or `SEMIAUTOMATIC`.  
  _Statement specifies mapping non-automatic operating modes to vehicle state UNAVAILABLE, consistent with vehicle state UNAVAILABLE in C-0013._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-1.1-03-runtime-behavior.adoc:112): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `CHARGING`: Set if the vehicle reports `charging = true`.  
  _Statement specifies mapping charging = true to vehicle state CHARGING, consistent with vehicle state CHARGING in C-0013._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-1.1-03-runtime-behavior.adoc:114): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `IDLE`: In all other cases.  
  _Statement specifies default fallback vehicle state IDLE, consistent with vehicle state IDLE in C-0013._
- **C-0016** — code: A transport order lifecycle transitions through RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, and UNROUTABLE states. (`TransportOrder.java:990-1033`)  
  doc (vda5050-1.1-03-runtime-behavior.adoc:148): Runtime behaviour › Topics › Topic `order` › Actions at locations / transport order destinations: NOTE: With actions at transport order destinations, the destination's operation also has to be used with the corresponding location's location type element in the plant model. If the destination's opera  
  _Statement specifies marking an order as UNROUTABLE if an operation is not supported by the location type, consistent with the UNROUTABLE order lifecycle state in C-0016._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-2.0-03_runtime-behavior.adoc:107): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `ERROR`: Set if the vehicle reports at least one `error` with an `errorLevel` of `FATAL`.  
  _Supported by C-0013 establishing ERROR as an operational vehicle state when a failure condition occurs._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-2.0-03_runtime-behavior.adoc:108): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `UNAVAILABLE`: Set if the vehicle reports an `operatingMode` other than `AUTOMATIC` or `SEMIAUTOMATIC`.  
  _Supported by C-0013 defining UNAVAILABLE as an operational vehicle state when unavailable for orders._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-2.0-03_runtime-behavior.adoc:109): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `CHARGING`: Set if the vehicle reports `charging = true`.  
  _Supported by C-0013 defining CHARGING as an operational vehicle state when recharging/refueling._
- **C-0013** — code: A vehicle's operational state can be UNKNOWN (communication lost), UNAVAILABLE (not in error but unavailable for orders), ERROR (failure condition), IDLE (ready for movement orders), EXECUTING (processing movement orders), or CHARGING (recharging or refueling). (`Vehicle.java:1363-1391`)  
  doc (vda5050-2.0-03_runtime-behavior.adoc:111): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `IDLE`: In all other cases.  
  _Supported by C-0013 defining IDLE as an operational vehicle state when ready for movement orders._
- **C-0016** — code: A transport order lifecycle transitions through RAW, ACTIVE, DISPATCHABLE, BEING_PROCESSED, WITHDRAWN, FINISHED, FAILED, and UNROUTABLE states. (`TransportOrder.java:990-1033`)  
  doc (vda5050-2.0-03_runtime-behavior.adoc:145): Runtime behaviour › Topics › Topic `order` › Actions at locations / transport order destinations: NOTE: With actions at transport order destinations, the destination's operation also has to be used with the corresponding location's location type element in the plant model. If the destination's opera  
  _Supported by C-0016 which confirms that UNROUTABLE is a transport order state when order requirements cannot be met._

## Structural diff (deterministic)

- Endpoints: 16 matched, code-only none, doc-only none
- `GetVehicleResponseTO.allowedOrderTypes` vs schema `vehicles:VehicleState`: property documented, not in code type
- `GetVehicleResponseTO.length` vs schema `vehicles:VehicleState`: property documented, not in code type
- `OrderSequenceEventTO.OrderSequenceTO.history` vs schema `order-sequences:OrderSequenceState`: field in code, not in documented schema
- `OrderStatusMessage.sequenceNumber` vs schema `events:OrderStatusMessage`: property documented, not in code type
- `OrderStatusMessage.type` vs schema `events:OrderStatusMessage`: property documented, not in code type
- `VehicleEventTO.TimestampedVehicleProcStateTO`: no matching documented schema
- `VehicleStatusMessage.orientationAngle` vs schema `events:VehicleStatusMessage`: field in code, not in documented schema
- `VehicleStatusMessage.sequenceNumber` vs schema `events:VehicleStatusMessage`: property documented, not in code type
- `VehicleStatusMessage.type` vs schema `events:VehicleStatusMessage`: property documented, not in code type
- `VehicleStatusMessage.vehicleOrientationAngle` vs schema `events:VehicleStatusMessage`: property documented, not in code type
- Enum `AngleProperty.Unit`: values not documented as an enum
- Enum `CommandProcessingTracker.PendingCommandState`: values not documented as an enum
- Enum `DefaultDispatcherConfiguration.ReroutingImpossibleStrategy`: values not documented as an enum
- Enum `Kernel.State`: values not documented as an enum
- Enum `KernelControlCenterApplication.ConnectionState`: values not documented as an enum
- Enum `KernelStateChangeEvent.State`: values not documented as an enum
- Enum `LengthProperty.Unit`: values not documented as an enum
- Enum `LocalVehicleEntry.Attribute`: values not documented as an enum
- Enum `ModelRestorationProgressStatus`: values not documented as an enum
- Enum `OperationMode`: values not documented as an enum
- Enum `OperationsDeskApplication.ConnectionState`: values not documented as an enum
- Enum `PercentProperty.Unit`: values not documented as an enum
- Enum `PeripheralProcessModel.Attribute`: values not documented as an enum
- Enum `PortalManager.ConnectionMode`: values not documented as an enum
- Enum `PortalManager.ConnectionState`: values not documented as an enum
- Enum `SpeedProperty.Unit`: values not documented as an enum
- Enum `SystemModel.FolderKey`: values not documented as an enum
- Enum `SystemModelTransitionEvent.Stage`: values not documented as an enum
- Enum `TCSObjectEvent.Type`: values not documented as an enum
- Enum `UserMessageHelper.Type`: values not documented as an enum
- Enum `VehicleEntry.Attribute`: values not documented as an enum
- Enum `VehicleProcessModel.Attribute`: values not documented as an enum
- POST /v1/vehicles/dispatcher/trigger: code can return [400] not listed in the spec (documented: [200])
- PUT /v1/vehicles/{NAME}/commAdapter/enabled: code can return [400] not listed in the spec (documented: [200, 404])
- PUT /v1/vehicles/{NAME}/integrationLevel: code can return [400] not listed in the spec (documented: [200, 404])
- POST /v1/vehicles/{NAME}/rerouteRequest: code can return [400] not listed in the spec (documented: [200, 404])
- PUT /v1/vehicles/{NAME}/envelopeKey: code can return [400] not listed in the spec (documented: [200, 404])
