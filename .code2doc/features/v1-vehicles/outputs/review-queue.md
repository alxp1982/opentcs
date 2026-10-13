# SME review queue — Vehicles (/v1/vehicles)

Only exceptions are listed. Record decisions in `dispositions.jsonl` as `{"item","disposition","note","sme"}` with disposition INTENDED, DOC_BUG or CODE_BUG.

## 1. Code vs. published docs

| Item | Verdict | Finding | Suggested |
|---|---|---|---|
| D-2397 | DOC_ONLY | UserNotificationLevel enum values (INFORMATIONAL, NOTEWORTHY, IMPORTANT) describe schema values not covered by verified claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0093 | DOC_ONLY | Changelog entry regarding clearing vehicle order reservations when integration level changes to non-TO_BE_UTILIZED is not verified by current claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0144 | DOC_ONLY | Historical changelog note regarding vehicle processing state and integration level TO_BE_UTILIZED. | DOC_BUG (stale doc) or CODE_BUG |
| D-0158 | DOC_ONLY | Changelog note regarding removal of reset position function in Kernel Control Center. | DOC_BUG (stale doc) or CODE_BUG |
| D-0159 | DOC_ONLY | Changelog note on introduction of vehicle integration level property in version 4.10. | DOC_BUG (stale doc) or CODE_BUG |
| D-0187 | DOC_ONLY | Changelog note on initial processing state in version 2.4. | DOC_BUG (stale doc) or CODE_BUG |
| D-0733 | DOC_ONLY | Documentation describes VDA 5050 driver runtime behavior for instantActions cancelOrder, which is not verified by any kernel Web API feature claims in scope. | DOC_BUG (stale doc) or CODE_BUG |
| D-0776 | DOC_ONLY | Documentation specifies mapping of VDA 5050 state info elements to vehicle property vda5050:information.info, not covered by any verified claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0779 | DOC_ONLY | Documentation specifies VDA 5050 vehicle state mapping to ERROR on FATAL errorLevel, not covered by any verified claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0780 | DOC_ONLY | Documentation specifies VDA 5050 vehicle state mapping to UNAVAILABLE when operatingMode is not AUTOMATIC or SEMIAUTOMATIC, not covered by verified claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0781 | DOC_ONLY | Documentation specifies VDA 5050 vehicle state mapping to CHARGING when charging is true, not covered by verified claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0783 | DOC_ONLY | Documentation specifies VDA 5050 vehicle state mapping to IDLE in all other cases, not covered by verified claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0814 | DOC_ONLY | Documentation specifies VDA 5050 vehicle protocol requirements for reflecting instant actions in actionStates, which is driver/protocol behavior not verified by kernel Web API claims. | DOC_BUG (stale doc) or CODE_BUG |
| D-0818 | DOC_ONLY | Documentation describes VDA 5050 v2.0 driver runtime behavior for cancelOrder instant actions, not covered by verified claims. | DOC_BUG (stale doc) or CODE_BUG |

## 2. Claims the verifier corrected or rejected

| Claim | Verdict | Reason |
|---|---|---|
| C-0017 | REJECTED | The cited enum definitions in PeripheralJobEventTO.java lines 71-73 and PeripheralOperationTO.java lines 84-86 only list the constants IMMEDIATE, AFTER_ALLOCATION, and AFTER_MOVEMENT; they contain no code defining trigger semantics or execu |
| C-0019 | REJECTED | PortalManager.java lines 69-83 define enum constants CONNECTING, CONNECTED, DISCONNECTING, and DISCONNECTED; no transitions or state progression logic are present in the cited code. |
| C-0020 | REJECTED | PortalManager.java lines 94-102 only define the enum constants AUTO, MANUAL, and RECONNECT without executable connection logic, dialog display, or parameter handling. |
| C-0022 | REJECTED | SystemModel.java lines 254-286 merely define enum values without code showing that plant elements are organized into folders. |
| C-0024 | REJECTED | TCSObjectEvent.java lines 125-133 define enum constants OBJECT_CREATED, OBJECT_MODIFIED, and OBJECT_REMOVED; no executable code producing or indicating lifecycle events is shown. |
| C-0026 | REJECTED | UserMessageHelper.java lines 153-165 define enum constants PLAIN, INFO, ERROR, and QUESTION without executable dialog presentation code. |
| C-0027 | REJECTED | UserNotification.java lines 138-146 only define enum constants INFORMATIONAL, NOTEWORTHY, and IMPORTANT, showing no executable code associating them with notifications. |
| C-0028 | REJECTED | Vehicle.java lines 1401-1414, VehicleEventTO.java lines 139-142, and VehicleTO.java lines 200-203 list enum constants; the behavior (ignoring positions, reserving resources, order assignment) is described only in comments, not executable co |
| C-0030 | REJECTED | Vehicle.java lines 1369-1390, VehicleEventTO.java lines 146-151, and VehicleTO.java lines 188-193 define enum constants; they do not contain code reporting a vehicle's operational state. |
| C-0036 | CORRECTED | Lines 49-59 show fields name, properties, history, envelope, pose, type, integrationLevel, layout, createdTime, and retiredTime, but layout layer ID is not defined in the cited lines (layout is of type LayoutTO without visible layer ID in l |
| C-0042 | CORRECTED | Lines 48-57 define fields name, properties, history, reservationToken, relatedVehicle, relatedTransportOrder, peripheralOperation, state, creationTime, and finishedTime, but the internal details of PeripheralOperationTO (location, operation |
| C-0040 | CORRECTED | Lines 49-60 of OrderSequenceEventTO.java define fields name, properties, history, type, orderTypes, orders, finishedIndex, complete, finished, failureFatal, intendedVehicle, and processingVehicle, but do not contain creation/finished times. |
| C-0041 | REJECTED | Lines 19-31 of OrderStatusMessage.java show fields orderName, dispensable, peripheralReservationToken, wrappingSequence, orderType, intendedVehicle, and processingVehicleName, but do not show order state or destination list. |
| C-0047 | REJECTED | Lines 18-31 of PutVehicleAcceptableOrderTypesTO.java show a list of AcceptableOrderTypeTO, but the cited lines do not show that AcceptableOrderTypeTO contains priorities. |
| C-0901 | CORRECTED | Lines 827-829 in V1RequestHandler.java show parsing the immediate query parameter with Boolean.parseBoolean (which defaults to false), but do not show any behavior about aborting a transport order. |

## 3. Verified claims with confidence below 0.7

None.

## 4. Facts not explained by claims

- `enum:PeripheralJobEventTO.PeripheralJobTO.PeripheralOperationTO.ExecutionTriggerTO.AFTER_ALLOCATION` ([`PeripheralJobEventTO.java:72`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/PeripheralJobEventTO.java#L72))
- `enum:PeripheralJobEventTO.PeripheralJobTO.PeripheralOperationTO.ExecutionTriggerTO.AFTER_MOVEMENT` ([`PeripheralJobEventTO.java:73`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/PeripheralJobEventTO.java#L73))
- `enum:PeripheralJobEventTO.PeripheralJobTO.PeripheralOperationTO.ExecutionTriggerTO.IMMEDIATE` ([`PeripheralJobEventTO.java:71`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/PeripheralJobEventTO.java#L71))
- `enum:PeripheralOperationTO.ExecutionTrigger.AFTER_ALLOCATION` ([`PeripheralOperationTO.java:85`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/PeripheralOperationTO.java#L85))
- `enum:PeripheralOperationTO.ExecutionTrigger.AFTER_MOVEMENT` ([`PeripheralOperationTO.java:86`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/PeripheralOperationTO.java#L86))
- `enum:PeripheralOperationTO.ExecutionTrigger.IMMEDIATE` ([`PeripheralOperationTO.java:84`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/PeripheralOperationTO.java#L84))
- `enum:PortalManager.ConnectionMode.AUTO` ([`PortalManager.java:94`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-common/src/main/java/org/opentcs/common/PortalManager.java#L94))
- `enum:PortalManager.ConnectionMode.MANUAL` ([`PortalManager.java:98`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-common/src/main/java/org/opentcs/common/PortalManager.java#L98))
- `enum:PortalManager.ConnectionMode.RECONNECT` ([`PortalManager.java:102`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-common/src/main/java/org/opentcs/common/PortalManager.java#L102))
- `enum:PortalManager.ConnectionState.CONNECTED` ([`PortalManager.java:73`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-common/src/main/java/org/opentcs/common/PortalManager.java#L73))
- `enum:PortalManager.ConnectionState.CONNECTING` ([`PortalManager.java:69`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-common/src/main/java/org/opentcs/common/PortalManager.java#L69))
- `enum:PortalManager.ConnectionState.DISCONNECTED` ([`PortalManager.java:83`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-common/src/main/java/org/opentcs/common/PortalManager.java#L83))
- `enum:PortalManager.ConnectionState.DISCONNECTING` ([`PortalManager.java:77`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-common/src/main/java/org/opentcs/common/PortalManager.java#L77))
- `enum:SystemModel.FolderKey.BLOCKS` ([`SystemModel.java:282`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L282))
- `enum:SystemModel.FolderKey.LAYOUT` ([`SystemModel.java:258`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L258))
- `enum:SystemModel.FolderKey.LINKS` ([`SystemModel.java:274`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L274))
- `enum:SystemModel.FolderKey.LOCATIONS` ([`SystemModel.java:266`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L266))
- `enum:SystemModel.FolderKey.LOCATION_TYPES` ([`SystemModel.java:278`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L278))
- `enum:SystemModel.FolderKey.OTHER_GRAPHICAL_ELEMENTS` ([`SystemModel.java:286`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L286))
- `enum:SystemModel.FolderKey.PATHS` ([`SystemModel.java:270`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L270))
- `enum:SystemModel.FolderKey.POINTS` ([`SystemModel.java:262`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L262))
- `enum:SystemModel.FolderKey.VEHICLES` ([`SystemModel.java:254`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/model/SystemModel.java#L254))
- `enum:TCSObjectEvent.Type.OBJECT_CREATED` ([`TCSObjectEvent.java:125`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/TCSObjectEvent.java#L125))
- `enum:TCSObjectEvent.Type.OBJECT_MODIFIED` ([`TCSObjectEvent.java:129`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/TCSObjectEvent.java#L129))
- `enum:TCSObjectEvent.Type.OBJECT_REMOVED` ([`TCSObjectEvent.java:133`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/TCSObjectEvent.java#L133))
- `enum:UserMessageHelper.Type.ERROR` ([`UserMessageHelper.java:161`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/util/UserMessageHelper.java#L161))
- `enum:UserMessageHelper.Type.INFO` ([`UserMessageHelper.java:157`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/util/UserMessageHelper.java#L157))
- `enum:UserMessageHelper.Type.PLAIN` ([`UserMessageHelper.java:153`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/util/UserMessageHelper.java#L153))
- `enum:UserMessageHelper.Type.QUESTION` ([`UserMessageHelper.java:165`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-plantoverview-common/src/main/java/org/opentcs/guing/common/util/UserMessageHelper.java#L165))
- `enum:UserNotification.Level.IMPORTANT` ([`UserNotification.java:146`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/notification/UserNotification.java#L146))
- `enum:UserNotification.Level.INFORMATIONAL` ([`UserNotification.java:138`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/notification/UserNotification.java#L138))
- `enum:UserNotification.Level.NOTEWORTHY` ([`UserNotification.java:142`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/notification/UserNotification.java#L142))
- `enum:Vehicle.IntegrationLevel.TO_BE_IGNORED` ([`Vehicle.java:1401`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1401))
- `enum:Vehicle.IntegrationLevel.TO_BE_NOTICED` ([`Vehicle.java:1406`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1406))
- `enum:Vehicle.IntegrationLevel.TO_BE_RESPECTED` ([`Vehicle.java:1410`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1410))
- `enum:Vehicle.IntegrationLevel.TO_BE_UTILIZED` ([`Vehicle.java:1414`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1414))
- `enum:Vehicle.State.CHARGING` ([`Vehicle.java:1390`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1390))
- `enum:Vehicle.State.ERROR` ([`Vehicle.java:1378`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1378))
- `enum:Vehicle.State.EXECUTING` ([`Vehicle.java:1386`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1386))
- `enum:Vehicle.State.IDLE` ([`Vehicle.java:1382`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1382))
- `enum:Vehicle.State.UNAVAILABLE` ([`Vehicle.java:1374`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1374))
- `enum:Vehicle.State.UNKNOWN` ([`Vehicle.java:1369`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/data/model/Vehicle.java#L1369))
- `enum:VehicleEventTO.IntegrationLevelTO.TO_BE_IGNORED` ([`VehicleEventTO.java:139`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L139))
- `enum:VehicleEventTO.IntegrationLevelTO.TO_BE_NOTICED` ([`VehicleEventTO.java:140`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L140))
- `enum:VehicleEventTO.IntegrationLevelTO.TO_BE_RESPECTED` ([`VehicleEventTO.java:141`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L141))
- `enum:VehicleEventTO.IntegrationLevelTO.TO_BE_UTILIZED` ([`VehicleEventTO.java:142`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L142))
- `enum:VehicleEventTO.StateTO.CHARGING` ([`VehicleEventTO.java:151`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L151))
- `enum:VehicleEventTO.StateTO.ERROR` ([`VehicleEventTO.java:148`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L148))
- `enum:VehicleEventTO.StateTO.EXECUTING` ([`VehicleEventTO.java:150`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L150))
- `enum:VehicleEventTO.StateTO.IDLE` ([`VehicleEventTO.java:149`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L149))
- `enum:VehicleEventTO.StateTO.UNAVAILABLE` ([`VehicleEventTO.java:147`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L147))
- `enum:VehicleEventTO.StateTO.UNKNOWN` ([`VehicleEventTO.java:146`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/sse/VehicleEventTO.java#L146))
- `enum:VehicleTO.IntegrationLevel.TO_BE_IGNORED` ([`VehicleTO.java:200`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L200))
- `enum:VehicleTO.IntegrationLevel.TO_BE_NOTICED` ([`VehicleTO.java:201`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L201))
- `enum:VehicleTO.IntegrationLevel.TO_BE_RESPECTED` ([`VehicleTO.java:202`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L202))
- `enum:VehicleTO.IntegrationLevel.TO_BE_UTILIZED` ([`VehicleTO.java:203`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L203))
- `enum:VehicleTO.State.CHARGING` ([`VehicleTO.java:193`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L193))
- `enum:VehicleTO.State.ERROR` ([`VehicleTO.java:190`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L190))
- `enum:VehicleTO.State.EXECUTING` ([`VehicleTO.java:192`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L192))
- `enum:VehicleTO.State.IDLE` ([`VehicleTO.java:191`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L191))
- `enum:VehicleTO.State.UNAVAILABLE` ([`VehicleTO.java:189`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L189))
- `enum:VehicleTO.State.UNKNOWN` ([`VehicleTO.java:188`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/plantmodel/VehicleTO.java#L188))
- `error:IllegalArgumentException:String.format(
              String.valueOf(messageTemplate)` ([`Assertions.java:46`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-api-base/src/main/java/org/opentcs/util/Assertions.java#L46))
- `error:IllegalArgumentException:Updating a vehicle's envelope key while the vehicle is processing an order or ` ([`StandardVehicleService.java:420`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel/src/main/java/org/opentcs/kernel/services/StandardVehicleService.java#L420))
- `field:OrderStatusMessage.destinations` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.dispensable` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.intendedVehicle` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.orderName` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.orderState` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.orderType` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.peripheralReservationToken` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.processingVehicleName` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.properties` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:OrderStatusMessage.wrappingSequence` ([`OrderStatusMessage.java:15`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/getevents/OrderStatusMessage.java#L15))
- `field:PutVehicleAcceptableOrderTypesTO.acceptableOrderTypes` ([`PutVehicleAcceptableOrderTypesTO.java:16`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/PutVehicleAcceptableOrderTypesTO.java#L16))

## 5. Facts ignored as non-behavior (check the reasons)

- `const:Dispatcher.PROPKEY_PARKING_POSITION_PRIORITY`: Internal dispatcher strategy constant for parking position priority property, deep inside dispatcher internals.
- `const:DriveOrder.Destination.OP_MOVE`: Internal dispatcher operation constant used in deep drive order routability heuristics.
- `const:DriveOrder.Destination.OP_NOP`: Internal dispatcher operation constant used in deep drive order routability heuristics.
- `const:OrderConstants.TYPE_CHARGE`: Internal transport order type constant set when creating automatic recharge orders during dispatching.
- `const:OrderConstantsTO.TYPE_NONE`: Default type constant for TransportOrderCreationTO, not configurable or observable through this endpoint.
- `error:IllegalArgumentException:: Not part of a sequence`: Internal exception thrown in private TransportOrderUtil.skipOrderInSequence, unreachable as an observable API error for this trigger endpoint.
- `const:DriveOrder.Destination.OP_PARK`: Transport order destination operation constant used in DefaultRouter for order routing, not relevant to route computation query.
- `const:ObjectPropConstants.TRANSPORT_ORDER_RESOURCES_TO_AVOID`: Transport order property key constant for avoiding resources, not used in the route computation query call path.
- `const:ResourceAvoidanceExtractor.ResourcesToAvoid.EMPTY`: Internal sentinel instance indicating no resources to avoid in router helper.
- `const:CloseFileAction.ID`: Desktop GUI action identifier in operationsdesk, unrelated to vehicle comm adapter attachment.
- `const:CloseFileAction.ID_MODEL_CLOSING`: Desktop GUI action identifier in operationsdesk, unrelated to vehicle comm adapter attachment.
- `const:I18nPlantOverview.MENU_PATH`: Desktop GUI menu path constant, unrelated to vehicle comm adapter attachment.
- `const:LoopbackAdapterConstants.PROPKEY_INITIAL_POSITION`: Property key constant used during loopback adapter initialization, not part of attachment endpoint behavior.
- `const:OpenTCSView.NAME`: Desktop GUI view name constant, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_ENVIRONMENTAL_ENTITIES`: SSE event type constant for environmental entities, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_LOCATIONS`: SSE event type constant for locations, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_ORDER_SEQUENCES`: SSE event type constant for order sequences, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_PATHS`: SSE event type constant for paths, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_PERIPHERAL_JOBS`: SSE event type constant for peripheral jobs, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_TRANSPORT_ORDERS`: SSE event type constant for transport orders, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_USER_NOTIFICATIONS`: SSE event type constant for user notifications, unrelated to vehicle comm adapter attachment.
- `const:SseConstants.EVENT_TYPE_VEHICLES`: SSE event type constant for vehicles, unrelated to vehicle comm adapter attachment.
- `error:IllegalArgumentException:: Could not allocate now although permission previously granted: `: Internal vehicle controller resource allocation error message, unrelated to comm adapter attachment.
- `error:IllegalArgumentException:String.format(\n              String.valueOf(messageTemplate)`: Generic assertion error formatting helper in Assertions utility class.
- `error:IllegalArgumentException:Unhandled model class: `: Desktop plant overview GUI model adapter error message, unrelated to comm adapter attachment.
- `error:IllegalArgumentException:Unhandled state: `: Desktop operations desk GUI state change event error message, unrelated to comm adapter attachment.
- `error:IllegalArgumentException:currentObjectState == null but eventType != OBJECT_REMOVED`: Internal TCSObjectEvent constructor invariant check.
- `error:IllegalArgumentException:previousObjectState == null but eventType != OBJECT_CREATED`: Internal TCSObjectEvent constructor invariant check.
- `error:ObjectUnknownException:Unknown vehicle: `: Error message thrown in transport order dispatcher handler, not this unit's handler.
- `error:ServiceUnavailableException:Could not connect to portal`: Desktop plant overview client connection error, unrelated to comm adapter attachment.
- `const:OrderConstants.TYPE_ANY`: Default acceptable order type constant in Vehicle constructor, not related to setting the vehicle paused state.
- `error:ObjectUnknownException:name`: Internal repository lookup error not reachable from this vehicle withdrawal path
- `error:ObjectUnknownException:ref`: Internal repository lookup error for object references not reachable as an independent product failure
