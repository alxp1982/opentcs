# code2doc run summary — Vehicles (/v1/vehicles)

Mode: full. Verification: blind, isolated subagent calls.

Code documented: [opentcs @ 742b0e6d](https://github.com/alxp1982/opentcs/commit/742b0e6df9ccbabc660e585a85e55d585c0642fc)

## Scope
- Entry points: 16 (POST /v1/vehicles/dispatcher/trigger, POST /v1/vehicles/{NAME}/routeComputationQuery, PUT /v1/vehicles/{NAME}/commAdapter/attachment, GET /v1/vehicles/{NAME}/commAdapter/attachmentInformation, PUT /v1/vehicles/{NAME}/commAdapter/enabled, POST /v1/vehicles/{NAME}/commAdapter/message, PUT /v1/vehicles/{NAME}/paused, PUT /v1/vehicles/{NAME}/integrationLevel, POST /v1/vehicles/{NAME}/withdrawal, POST /v1/vehicles/{NAME}/rerouteRequest, PUT /v1/vehicles/{NAME}/allowedOrderTypes, PUT /v1/vehicles/{NAME}/acceptableOrderTypes, PUT /v1/vehicles/{NAME}/energyLevelThresholdSet, PUT /v1/vehicles/{NAME}/envelopeKey, GET /v1/vehicles/{NAME}, GET /v1/vehicles)
- Slice: 1009 methods in 316 files; boundaries: none

## Facts and coverage
- Facts: 531 counted; coverage 85.9%; by kind {'body': '5/5', 'const': '5/5', 'endpoint': '16/16', 'enum': '156/218', 'error': '18/20', 'field': '244/255', 'param': '7/7', 'status': '5/5'}

## Claims
- Status: {'verified': 112, 'rejected': 11}
- Verified by type: {'rule': 31, 'state': 14, 'capability': 20, 'error': 39, 'default': 4, 'integration': 3, 'validation': 1}
- Verifier verdicts: {'CONFIRMED': 108, 'REJECTED': 11, 'CORRECTED': 4}

## Reconciliation
- {'CONFIRMED': 344, 'UNDOCUMENTED': 52, 'DOC_ONLY': 14}

## Documents
- `product.md`: citation errors 0, grounding fixes 0, claims cited 80
- `customer-guide.md`: citation errors 0, grounding fixes 2, claims cited 76

## Model usage
- 255 calls, 4,358,353 input / 97,444 output / 0 thinking tokens
- By stage: {'extract': {'calls': 201, 'in': 3880129, 'out': 38776}, 'verify': {'calls': 11, 'in': 66671, 'out': 12442}, 'reconcile': {'calls': 37, 'in': 353643, 'out': 30566}, 'write': {'calls': 2, 'in': 19163, 'out': 8255}, 'ground': {'calls': 2, 'in': 15009, 'out': 294}, 'repair': {'calls': 1, 'in': 11744, 'out': 3554}, 'fix': {'calls': 1, 'in': 11994, 'out': 3557}}

## Agents and prompts

| Agent | Model | Thinking |
|---|---|---|
| claim_extractor | gemini-3.8-flash | low |
| claim_verifier | gemini-3.8-flash | low |
| reconciler | gemini-3.8-flash | low |
| doc_writer | gemini-3.8-flash | low |
| grounding_checker | gemini-3.8-flash | low |

Prompt versions (layers used, content hash):

- `context/product` 6ed957379561: registry:opentcs v2
- `prompts/claim_extractor` 2c426ebad56c: claim_extractor.md@default, claim-schema.md@default
- `prompts/claim_verifier` 729857f24ef4: claim_verifier.md@default
- `prompts/doc_writer` 395bdfa4cfae: doc_writer.md@default
- `prompts/grounding_checker` 7dee035eaa13: grounding_checker.md@default
- `prompts/reconciler` 1404066e28f6: reconciler.md@default, reconciliation.md@default
- `templates/customer-guide` 8ad4f12e0e11: customer-guide.md@default, _disposition.md@default
- `templates/product` 5701cebfd425: product.md@default, _disposition.md@default

## Choices made automatically
- 20 claims were first rejected because their cited lines stopped short of the code; 18 held up when verified with a wider range of lines.
