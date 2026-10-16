# SME review queue — Transport Orders (/v1/transportOrders)

Only exceptions are listed. Record decisions in `dispositions.jsonl` as `{"item","disposition","note","sme"}` with disposition INTENDED, DOC_BUG or CODE_BUG.

## 1. Code vs. published docs

| Item | Verdict | Finding | Suggested |
|---|---|---|---|
| C-0001 | AMBIGUOUS | Candidate statements document HTTP 400 for specific endpoints when input is invalid, but do not document the global Web API exception handler behavior mapping any IllegalArgumentException to HTTP 400. [`ServiceWebApi.java:175-178`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/ServiceWebApi.java#L175-L178) | clarify docs |
| C-0003 | AMBIGUOUS | Endpoints individually document HTTP 404 when referenced entities are not found, but a general exception mapping rule for ObjectUnknownException across the Web API is not explicitly stated. [`ServiceWebApi.java:187-190`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/ServiceWebApi.java#L187-L190) | clarify docs |
| C-0006 | CONTRADICTION | Code states property value is optional, whereas OpenAPI schema specifies Property.value as required ('common:Property.value; required; type string.'). [`Property.java:16-18`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/shared/Property.java#L16-L18) | DOC_BUG or CODE_BUG |
| C-0010 | CONTRADICTION | Code specifies destination state can be PRISTINE, TRAVELLING, OPERATING, FINISHED, FAILED; documentation includes ACTIVE in DestinationState ('PRISTINE, ACTIVE, TRAVELLING, OPERATING, FINISHED, FAILED'). [`DestinationState.java:100-108`](https://github.com/alxp1982/opentcs/blob/742b0e6df9ccbabc660e585a85e55d585c0642fc/opentcs-kernel-extension-http-services/src/main/java/org/opentcs/kernel/extensions/servicewebapi/v1/binding/shared/DestinationState.java#L100-L108) | DOC_BUG or CODE_BUG |

## 2. Claims the verifier corrected or rejected

None.

## 3. Verified claims with confidence below 0.7

None.

## 4. Facts not explained by claims

None.

## 5. Facts ignored as non-behavior (check the reasons)

- `error:ObjectUnknownException:name`: Unused internal repository lookup error not reachable from this call path.
- `error:ObjectUnknownException:ref`: Internal repository error for missing reference when order was already verified to exist.
- `error:IllegalArgumentException:%s: Cannot change integration level to %s while processing orders.`: Unreachable from this endpoint because the vehicle is updated to TO_BE_RESPECTED, whereas the exception is only thrown for TO_BE_IGNORED or TO_BE_NOTICED.
- `error:ObjectUnknownException:Unknown transport order: `: Anchored in TransportOrderDispatcherHandler.java:54 which belongs to immediate assignment rather than this unit's call path
