# code2doc run summary — Vehicles (/v1/vehicles)

Mode: full. Verification: blind, isolated subagent calls.

## Scope
- Entry points: 16 (POST /v1/vehicles/dispatcher/trigger, POST /v1/vehicles/{NAME}/routeComputationQuery, PUT /v1/vehicles/{NAME}/commAdapter/attachment, GET /v1/vehicles/{NAME}/commAdapter/attachmentInformation, PUT /v1/vehicles/{NAME}/commAdapter/enabled, POST /v1/vehicles/{NAME}/commAdapter/message, PUT /v1/vehicles/{NAME}/paused, PUT /v1/vehicles/{NAME}/integrationLevel, POST /v1/vehicles/{NAME}/withdrawal, POST /v1/vehicles/{NAME}/rerouteRequest, PUT /v1/vehicles/{NAME}/allowedOrderTypes, PUT /v1/vehicles/{NAME}/acceptableOrderTypes, PUT /v1/vehicles/{NAME}/energyLevelThresholdSet, PUT /v1/vehicles/{NAME}/envelopeKey, GET /v1/vehicles/{NAME}, GET /v1/vehicles)
- Slice: 1009 methods in 316 files; boundaries: none

## Facts and coverage
- Facts: 287 counted; coverage 0.0%; by kind {'body': '0/5', 'const': '0/15', 'endpoint': '0/16', 'enum': '0/218', 'error': '0/21', 'param': '0/7', 'status': '0/5'}

## Claims
- Status: {'draft': 157}
- Verified by type: {}
- Verifier verdicts: {}

## Reconciliation
- {'DOC_ONLY': 8, 'CONFIRMED': 49}

## Documents
- `product.md`: citation errors 0, grounding fixes 8, claims cited 0

## Model usage
- 174 calls, 2,353,187 input / 73,648 output / 422,886 thinking tokens
- By stage: {'extract': {'calls': 132, 'in': 2228312, 'out': 170899}, 'verify': {'calls': 11, 'in': 47843, 'out': 63867}, 'reconcile': {'calls': 28, 'in': 72017, 'out': 254255}, 'write': {'calls': 1, 'in': 1686, 'out': 4262}, 'ground': {'calls': 1, 'in': 811, 'out': 2121}, 'fix': {'calls': 1, 'in': 2518, 'out': 1130}}

## Agents and prompts

| Agent | Model | Thinking |
|---|---|---|
| claim_extractor | gemini-3.1-pro-preview | high |
| claim_verifier | gemini-3.1-pro-preview | high |
| reconciler | gemini-3.8-flash | high |
| doc_writer | gemini-3.1-pro-preview | high |
| grounding_checker | gemini-3.8-flash | high |

Prompt versions (layers used, content hash):

- `context/product` a6ff23d5404f: registry:opentcs v1
- `prompts/claim_extractor` 2c426ebad56c: claim_extractor.md@default, claim-schema.md@default
- `prompts/claim_verifier` 729857f24ef4: claim_verifier.md@default
- `prompts/doc_writer` 4de9921ea965: doc_writer.md@default
- `prompts/grounding_checker` 7dee035eaa13: grounding_checker.md@default
- `prompts/reconciler` 1404066e28f6: reconciler.md@default, reconciliation.md@default
- `templates/product` 3337c809e6d8: product.md@default, _disposition.md@default
