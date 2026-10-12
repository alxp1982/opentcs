# code2doc run summary — Vehicles (/v1/vehicles)

Mode: full. Verification: blind, isolated subagent calls.

## Scope
- Entry points: 16 (POST /v1/vehicles/dispatcher/trigger, POST /v1/vehicles/{NAME}/routeComputationQuery, PUT /v1/vehicles/{NAME}/commAdapter/attachment, GET /v1/vehicles/{NAME}/commAdapter/attachmentInformation, PUT /v1/vehicles/{NAME}/commAdapter/enabled, POST /v1/vehicles/{NAME}/commAdapter/message, PUT /v1/vehicles/{NAME}/paused, PUT /v1/vehicles/{NAME}/integrationLevel, POST /v1/vehicles/{NAME}/withdrawal, POST /v1/vehicles/{NAME}/rerouteRequest, PUT /v1/vehicles/{NAME}/allowedOrderTypes, PUT /v1/vehicles/{NAME}/acceptableOrderTypes, PUT /v1/vehicles/{NAME}/energyLevelThresholdSet, PUT /v1/vehicles/{NAME}/envelopeKey, GET /v1/vehicles/{NAME}, GET /v1/vehicles)
- Slice: 1009 methods in 316 files; boundaries: none

## Facts and coverage
- Facts: 474 counted; coverage 98.1%; by kind {'body': '5/5', 'const': '9/10', 'endpoint': '16/16', 'enum': '150/155', 'error': '19/21', 'field': '255/255', 'param': '6/7', 'status': '5/5'}

## Claims
- Status: {'verified': 108, 'rejected': 2}
- Verified by type: {'error': 40, 'rule': 15, 'state': 14, 'integration': 9, 'capability': 23, 'default': 6, 'validation': 1}
- Verifier verdicts: {'CONFIRMED': 105, 'CORRECTED': 3, 'REJECTED': 2}

## Reconciliation
- {'CONFIRMED': 346, 'UNDOCUMENTED': 44, 'CONTRADICTION': 5, 'AMBIGUOUS': 3, 'DOC_ONLY': 4}

## Documents
- `product.md`: citation errors 0, grounding fixes 0, claims cited 81

## Model usage
- 243 calls, 4,645,337 input / 102,179 output / 0 thinking tokens
- By stage: {'extract': {'calls': 195, 'in': 4250122, 'out': 52158}, 'verify': {'calls': 9, 'in': 57896, 'out': 10118}, 'reconcile': {'calls': 36, 'in': 301825, 'out': 29163}, 'write': {'calls': 1, 'in': 10492, 'out': 5353}, 'repair': {'calls': 1, 'in': 15894, 'out': 5378}, 'ground': {'calls': 1, 'in': 9108, 'out': 9}}

## Agents and prompts

| Agent | Model | Thinking |
|---|---|---|
| claim_extractor | gemini-3.8-flash | low |
| claim_verifier | gemini-3.8-flash | low |
| reconciler | gemini-3.8-flash | low |
| doc_writer | gemini-3.8-flash | low |
| grounding_checker | gemini-3.8-flash | low |

Prompt versions (layers used, content hash):

- `context/product` a6ff23d5404f: registry:opentcs v1
- `prompts/claim_extractor` 2c426ebad56c: claim_extractor.md@default, claim-schema.md@default
- `prompts/claim_verifier` 729857f24ef4: claim_verifier.md@default
- `prompts/doc_writer` 395bdfa4cfae: doc_writer.md@default
- `prompts/grounding_checker` 7dee035eaa13: grounding_checker.md@default
- `prompts/reconciler` 1404066e28f6: reconciler.md@default, reconciliation.md@default
- `templates/product` 5701cebfd425: product.md@default, _disposition.md@default

## Choices made automatically
- 7 claims were first rejected because their cited lines stopped short of the code; 5 held up when verified with a wider range of lines.
