# Reconciliation: v1-vehicles — code vs. published documentation

| Verdict | Count |
|---|---|
| CONTRADICTION | 0 |
| UNDOCUMENTED | 0 |
| DOC_ONLY | 8 |
| AMBIGUOUS | 0 |
| CONFIRMED | 49 |

## Doc Only

- **D-0899**  
  doc (events.yaml:94): events:OrderStatusMessage.type; optional; type string; default TransportOrder  
  _The documentation specifies an optional 'type' property with default 'TransportOrder' on OrderStatusMessage, but the code type OrderStatusMessage does not have this property._
- **D-0900**  
  doc (events.yaml:94): events:OrderStatusMessage.type is one of: TransportOrder.  
  _The documentation specifies allowed values for OrderStatusMessage.type, but the code type OrderStatusMessage does not define a 'type' property._
- **D-0901**  
  doc (events.yaml:99): events:OrderStatusMessage.sequenceNumber; optional  
  _The documentation specifies 'sequenceNumber' on OrderStatusMessage, but the code type OrderStatusMessage does not have a sequenceNumber property._
- **D-1599**  
  doc (vehicles.yaml:621): vehicles:VehicleState.energyLevelSufficientlyRecharged; required; type integer. The value (in %) at/below which the vehicle's energy level is considered 'sufficiently recharged'.  
  _Documentation states 'at/below which' the vehicle's energy level is considered 'sufficiently recharged', but code implements 'at/above which' (Vehicle.java: isEnergyLevelSufficientlyRecharged checks energyLevel >= energyLevelSufficientlyRecharged)._
- **D-1600**  
  doc (vehicles.yaml:626): vehicles:VehicleState.energyLevelFullyRecharged; required; type integer. The value (in %) at/below which the vehicle's energy level is considered 'fully recharged'.  
  _Documentation states 'at/below which' the vehicle's energy level is considered 'fully recharged', but code implements 'at/above which' (Vehicle.java: isEnergyLevelFullyRecharged checks energyLevel >= energyLevelFullyRecharged)._
- **D-2193**  
  doc (vehicles.yaml:622): vehicles:RoutesRequest.sourcePoint; optional; type string. The (optional) starting point for route computation. If `null` or not set, the vehicle's current position will be used.  
  _The documentation specifies fallback behavior using the vehicle's current position if sourcePoint is null or not set, but the codebase does not implement a RoutesRequest or vehicle route computation endpoint._
- **D-0188**  
  doc (changelog.adoc:1276): openTCS: Change Log › Version 2.4 (2013-02-07): Improved kernel methods for withdrawing orders from vehicles and allow setting a vehicle's processing state to `UNAVAILABLE` to prevent it being dispatched again immediately.  
  _The feature code does not implement an UNAVAILABLE vehicle processing state; Vehicle.ProcState only includes UNCOMMISSIONED, IDLE, AWAITING_ORDER, and PROCESSING_ORDER._
- **D-0189**  
  doc (changelog.adoc:1277): openTCS: Change Log › Version 2.4 (2013-02-07): Added kernel method dispatchVehicle() to allow vehicles in state `UNAVAILABLE` to be dispatched again.  
  _The feature code does not support dispatching vehicles in state UNAVAILABLE as the UNAVAILABLE processing state does not exist in openTCS._

## Confirmed

- **D-1516**  
  doc (vehicles.yaml:59): GET /vehicles/{NAME} returns 200: Successful operation  
  _Implemented in VehicleHandler.getVehicleByName: returns HTTP 200 with the VehicleState representation on success._
- **D-1517**  
  doc (vehicles.yaml:65): GET /vehicles/{NAME} returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Implemented in VehicleHandler.getVehicleByName: throws ObjectUnknownException when the vehicle name is not found, mapped to HTTP 404._
- **D-1522**  
  doc (vehicles.yaml:104): POST /vehicles/{NAME}/withdrawal returns 200: Successful operation  
  _Implemented in VehicleHandler.postVehicleWithdrawal: returns HTTP 200 upon successfully withdrawing the vehicle._
- **D-1524**  
  doc (vehicles.yaml:119): POST /vehicles/{NAME}/rerouteRequest — Reroutes a vehicle with the given name.  
  _Implemented in VehicleHandler.postVehicleRerouteRequest: triggers vehicle rerouting via DispatcherService.reroute._
- **D-1527**  
  doc (vehicles.yaml:141): POST /vehicles/{NAME}/rerouteRequest returns 200: Successful operation  
  _Implemented in VehicleHandler.postVehicleRerouteRequest: returns HTTP 200 upon successfully initiating the reroute._
- **D-1528**  
  doc (vehicles.yaml:143): POST /vehicles/{NAME}/rerouteRequest returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Implemented in VehicleHandler.postVehicleRerouteRequest: throws ObjectUnknownException when the vehicle is not found, returning HTTP 404._
- **D-1529**  
  doc (vehicles.yaml:156): PUT /vehicles/{NAME}/integrationLevel — Sets a new integration level for the named vehicle.  
  _Implemented in VehicleHandler.putVehicleIntegrationLevel: updates the vehicle integration level via VehicleService.updateVehicleIntegrationLevel._
- **D-1532**  
  doc (vehicles.yaml:179): PUT /vehicles/{NAME}/integrationLevel returns 200: Successful operation  
  _Implemented in VehicleHandler.putVehicleIntegrationLevel: returns HTTP 200 upon successfully updating the integration level._
- **D-1533**  
  doc (vehicles.yaml:181): PUT /vehicles/{NAME}/integrationLevel returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Implemented in VehicleHandler.putVehicleIntegrationLevel: throws ObjectUnknownException when the vehicle is not found, returning HTTP 404._
- **D-1537**  
  doc (vehicles.yaml:212): PUT /vehicles/{NAME}/paused returns 200: Successful operation  
  _Implemented in VehicleHandler.putVehiclePaused: returns HTTP 200 upon successfully setting the paused flag._
- **D-1542**  
  doc (vehicles.yaml:246): PUT /vehicles/{NAME}/allowedOrderTypes returns 200: Successful operation  
  _Implemented in VehicleHandler.putVehicleAllowedOrderTypes: updates acceptable order types and returns HTTP 200 on success._
- **D-1543**  
  doc (vehicles.yaml:248): PUT /vehicles/{NAME}/allowedOrderTypes returns 400: The submitted data is invalid. (example: Could not parse JSON input.)  
  _Supported by code: VehicleHandler returns 400 when request body cannot be parsed as JSON._
- **D-1544**  
  doc (vehicles.yaml:258): PUT /vehicles/{NAME}/allowedOrderTypes returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by code: VehicleHandler throws ObjectUnknownException returning 404 when the vehicle is not found._
- **D-1548**  
  doc (vehicles.yaml:288): PUT /vehicles/{NAME}/acceptableOrderTypes returns 200: Successful operation  
  _Supported by code: VehicleHandler returns 200 upon successfully updating acceptable order types._
- **D-1549**  
  doc (vehicles.yaml:290): PUT /vehicles/{NAME}/acceptableOrderTypes returns 400: The submitted data is invalid. (example: Could not parse JSON input.)  
  _Supported by code: VehicleHandler returns 400 when request body cannot be parsed as JSON._
- **D-1550**  
  doc (vehicles.yaml:300): PUT /vehicles/{NAME}/acceptableOrderTypes returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by code: VehicleHandler throws ObjectUnknownException returning 404 when the vehicle is not found._
- **D-1554**  
  doc (vehicles.yaml:330): PUT /vehicles/{NAME}/energyLevelThresholdSet returns 200: Successful operation  
  _Supported by code: VehicleHandler returns 200 upon successfully updating energy level threshold set._
- **D-1555**  
  doc (vehicles.yaml:332): PUT /vehicles/{NAME}/energyLevelThresholdSet returns 400: The submitted data is invalid. (example: Could not parse JSON input.)  
  _Supported by code: VehicleHandler returns 400 when request body cannot be parsed as JSON._
- **D-1556**  
  doc (vehicles.yaml:342): PUT /vehicles/{NAME}/energyLevelThresholdSet returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by code: VehicleHandler throws ObjectUnknownException returning 404 when the vehicle is not found._
- **D-1562**  
  doc (vehicles.yaml:385): PUT /vehicles/{NAME}/commAdapter/attachment — Attaches the given vehicle driver to this vehicle.  
  _Supported by code: VehicleHandler.putVehicleCommAdapterAttachment attaches the specified driver to the vehicle._
- **D-1565**  
  doc (vehicles.yaml:403): PUT /vehicles/{NAME}/commAdapter/attachment returns 200: Successful operation  
  _Supported by code: VehicleHandler returns 200 upon successfully attaching the comm adapter._
- **D-1566**  
  doc (vehicles.yaml:405): PUT /vehicles/{NAME}/commAdapter/attachment returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Supported by code: VehicleHandler throws ObjectUnknownException returning 404 when the vehicle is not found._
- **D-1567**  
  doc (vehicles.yaml:415): PUT /vehicles/{NAME}/commAdapter/attachment returns 400: The submitted value is invalid. (example: "Unknown vehicle driver class name: org.opentcs.someVehicle.driver11")  
  _Supported by code: VehicleHandler throws IllegalArgumentException returning 400 when the driver class name is invalid or unknown._
- **D-1581**  
  doc (vehicles.yaml:518): POST /vehicles/{NAME}/routeComputationQuery returns 200: Successful operation  
  _Implemented in VehicleHandler: POST /vehicles/{NAME}/routeComputationQuery computes a route using RouterService and returns 200 with the computation result._
- **D-1583**  
  doc (vehicles.yaml:534): POST /vehicles/{NAME}/routeComputationQuery returns 404: Referencing object that could not be found. (example: "Unknown source point: Point-X")  
  _Implemented in VehicleHandler: throws ObjectUnknownException resulting in HTTP 404 when the vehicle, source point, or destination point cannot be found._
- **D-1585**  
  doc (vehicles.yaml:547): PUT /vehicles/{NAME}/envelopeKey — Sets the envelope key for this vehicle.  
  _Implemented in VehicleHandler: PUT /vehicles/{NAME}/envelopeKey sets the vehicle envelope key via VehicleService.updateVehicleEnvelopeKey._
- **D-1588**  
  doc (vehicles.yaml:565): PUT /vehicles/{NAME}/envelopeKey returns 200: Successful operation  
  _Implemented in VehicleHandler: PUT /vehicles/{NAME}/envelopeKey returns HTTP 200 upon successful operation._
- **D-1589**  
  doc (vehicles.yaml:567): PUT /vehicles/{NAME}/envelopeKey returns 404: Referencing object that could not be found. (example: Could not find vehicle 'Vehicle-0001'.)  
  _Implemented in VehicleHandler: throws ObjectUnknownException (HTTP 404) when the vehicle is not found (e.g. "Could not find vehicle 'Vehicle-0001'.")._
- **D-0093**  
  doc (changelog.adoc:573): openTCS: Change Log › Version 5.10 (2023-09-23): Ensure that order reservations for vehicles are properly cleared in case a vehicle's integration level is changed to anything other than `TO_BE_UTILIZED`.  
  _Implemented in code: order reservations for a vehicle are cleared when its integration level changes to anything other than TO_BE_UTILIZED._
- **D-0158**  
  doc (changelog.adoc:953): openTCS: Change Log › Version 4.11 (2018-12-04): Remove the Kernel Control Center's function to reset the position of a vehicle. Users should now set the vehicle's integration level to `TO_BE_IGNORED`, instead.  
  _Implemented in code: vehicle position resetting requires the vehicle integration level to be set to TO_BE_IGNORED._
- **D-0159**  
  doc (changelog.adoc:963): openTCS: Change Log › Version 4.10 (2018-08-07): Introduce an explicit _integration level_ property for vehicles that expresses to what degree a vehicle should be integrated into the system. (Setting the integration level to `TO_BE_UTILIZED` replaces the manual dispatching that was previously used t  
  _Implemented in code: Vehicle defines an integrationLevel property, and setting it to TO_BE_UTILIZED enables the vehicle to be dispatched._
- **D-0277**  
  doc (developers-guide-03_reference.adoc:302): Reference › System requirements › Transport orders in openTCS › A transport order's life cycle: If the vehicle successfully processes the transport order as a whole, it is marked as `FINISHED`.  
  _Implemented in code: when a vehicle successfully processes a transport order as a whole, the transport order state is updated to FINISHED._
- **D-0316**  
  doc (users-guide-01_tutorials.adoc:306): Tutorials › Constructing a new plant model › Operating the plant › Creating a peripheral job › Implicit creation of peripheral jobs: `AFTER_ALLOCATION` (previously named `BEFORE_MOVEMENT`): The peripheral job is created as soon as the resource for the corresponding path has been allocated for the ve  
  _The feature code implements implicit peripheral job creation with execution trigger AFTER_ALLOCATION upon allocation of the corresponding path resource to a vehicle._
- **D-0317**  
  doc (users-guide-01_tutorials.adoc:308): Tutorials › Constructing a new plant model › Operating the plant › Creating a peripheral job › Implicit creation of peripheral jobs: `AFTER_MOVEMENT`: The peripheral job is created as soon as the vehicle has completely traversed the path and reported the corresponding movement command as executed (i  
  _The feature code implements implicit peripheral job creation with execution trigger AFTER_MOVEMENT upon completion of the vehicle movement command for the path._
- **D-0588**  
  doc (users-guide-03_reference.adoc:668): Reference › Supported navigation principles / vehicle types › Default strategies › Default peripheral job dispatcher: A peripheral device's state must be `IDLE`.  
  _The feature code in DefaultPeripheralJobDispatcher checks that a peripheral device's state is IDLE before assigning a peripheral job to it._
- **D-0691**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:103): Runtime behaviour › Topics › Topic `state` › Mapping of state information to vehicle properties: The property with the key `vda5050:information.info` will contain a concatenated list of ``infoType``s of all `info` elements with an `infoLevel` of `INFO`.  
  _The feature code in the VDA 5050 comm adapter maps state information with infoLevel INFO to the vehicle property vda5050:information.info as a concatenated list of infoTypes._
- **D-0695**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:111): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `UNAVAILABLE`: Set if the vehicle reports an `operatingMode` other than `AUTOMATIC` or `SEMIAUTOMATIC`.  
  _The feature code in the VDA 5050 comm adapter sets the vehicle state to UNAVAILABLE when the reported operatingMode is neither AUTOMATIC nor SEMIAUTOMATIC._
- **D-0696**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:112): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `CHARGING`: Set if the vehicle reports `charging = true`.  
  _The feature code in the VDA 5050 comm adapter sets the vehicle state to CHARGING when the vehicle reports charging = true._
- **D-0698**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:114): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `IDLE`: In all other cases.  
  _The feature code in the VDA 5050 comm adapter defaults the vehicle state to IDLE when no other specific state condition applies._
- **D-0707**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:148): Runtime behaviour › Topics › Topic `order` › Actions at locations / transport order destinations: NOTE: With actions at transport order destinations, the destination's operation also has to be used with the corresponding location's location type element in the plant model. If the destination's opera  
  _The feature code in DefaultRouter and DefaultDispatcher verifies destination operations against the location type's allowed operations, marking the transport order as UNROUTABLE if an operation is not supported._
- **D-0729**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:340): Runtime behaviour › Topics › Topic `instantActions`: The vehicle is expected to confirm that it has received an instant action by listing it in the `actionStates` array of its state message. In case the vehicle rejects an instant action, it must still reflect that instant action in its `actionStates  
  _The feature code in the VDA 5050 comm adapter inspects the vehicle's actionStates array to verify receipt of instant actions and handles actionStatus FAILED._
- **D-0733**  
  doc (vda5050-1.1-03-runtime-behavior.adoc:356): Runtime behaviour › Topics › Topic `instantActions` › `cancelOrder`: The vehicle driver transmits the `cancelOrder` instant action until the vehicle reflects it in its `actionStates` array. Once the vehicle reflects the action (with any `actionStatus`, including a non-terminal one such as `RUNNING`)  
  _The VDA 5050 vehicle driver implements periodic retransmission of cancelOrder until acknowledged in actionStates, and waits for a terminal actionStatus (FINISHED or FAILED) before sending subsequent orders._
- **D-0776**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:100): Runtime behaviour › Topics › Topic `state` › Mapping of state information to vehicle properties: The property with the key `vda5050:information.info` will contain a concatenated list of ``infoType``s of all `info` elements with an `infoLevel` of `INFO`.  
  _The feature code in the VDA 5050 v2.0 comm adapter maps state information with infoLevel INFO to the vehicle property vda5050:information.info as a concatenated list of infoTypes._
- **D-0780**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:108): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `UNAVAILABLE`: Set if the vehicle reports an `operatingMode` other than `AUTOMATIC` or `SEMIAUTOMATIC`.  
  _The feature code in the VDA 5050 v2.0 comm adapter sets the vehicle state to UNAVAILABLE when the reported operatingMode is neither AUTOMATIC nor SEMIAUTOMATIC._
- **D-0781**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:109): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `CHARGING`: Set if the vehicle reports `charging = true`.  
  _Implemented in openTCS VDA 5050 adapter (VehicleStateResolver/StateMapping): the vehicle state is set to CHARGING when the state's batteryState reports charging = true (provided higher-precedence states ERROR, UNAVAILABLE, and EXECUTING do not apply)._
- **D-0783**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:111): Runtime behaviour › Topics › Topic `state` › Mapping of vehicle state: `IDLE`: In all other cases.  
  _Implemented in openTCS VDA 5050 adapter (VehicleStateResolver/StateMapping): vehicle state defaults to IDLE in all cases where ERROR, UNAVAILABLE, EXECUTING, or CHARGING conditions are not met._
- **D-0792**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:145): Runtime behaviour › Topics › Topic `order` › Actions at locations / transport order destinations: NOTE: With actions at transport order destinations, the destination's operation also has to be used with the corresponding location's location type element in the plant model. If the destination's opera  
  _Implemented in openTCS router and dispatcher (StandardRouter/DefaultDispatcher): routability check ensures destination operations are supported by the target location type's allowed operations, marking the order as UNROUTABLE if unsupported._
- **D-0814**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:337): Runtime behaviour › Topics › Topic `instantActions`: The vehicle is expected to confirm that it has received an instant action by listing it in the `actionStates` array of its state message. In case the vehicle rejects an instant action, it must still reflect that instant action in its `actionStates  
  _Implemented in openTCS VDA 5050 adapter: the driver inspects incoming state messages' actionStates array to verify instant action acknowledgement and handles actionStatus FAILED as rejected/failed execution._
- **D-0818**  
  doc (vda5050-2.0-03_runtime-behavior.adoc:353): Runtime behaviour › Topics › Topic `instantActions` › `cancelOrder`: The vehicle driver transmits the `cancelOrder` instant action until the vehicle reflects it in its `actionStates` array. Once the vehicle reflects the action (with any `actionStatus`, including a non-terminal one such as `RUNNING`)  
  _Implemented in openTCS VDA 5050 adapter: order cancellation periodically transmits the cancelOrder instant action until it appears in actionStates, stops resending upon reflection, and waits for a terminal status (FINISHED or FAILED) before dispatching new orders._

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
