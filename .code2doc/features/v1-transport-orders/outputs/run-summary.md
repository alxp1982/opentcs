# code2doc run summary — Transport Orders (/v1/transportOrders)

Mode: full. Verification: blind, isolated subagent calls.

Code documented: [opentcs @ 742b0e6d](https://github.com/alxp1982/opentcs/commit/742b0e6df9ccbabc660e585a85e55d585c0642fc)

## Scope
- Entry points: 7 (POST /v1/transportOrders/dispatcher/trigger, POST /v1/transportOrders/{NAME}/immediateAssignment, POST /v1/transportOrders/{NAME}/withdrawal, POST /v1/transportOrders/{NAME}, PUT /v1/transportOrders/{NAME}/intendedVehicle, GET /v1/transportOrders/{NAME}, GET /v1/transportOrders)
- Slice: 130 methods in 31 files; boundaries: org.opentcs.strategies, org.opentcs.data

## Facts and coverage
- Facts: 132 counted; coverage 100.0%; by kind {'body': '1/1', 'const': '8/8', 'endpoint': '7/7', 'enum': '38/38', 'error': '9/9', 'field': '59/59', 'param': '5/5', 'status': '5/5'}

## Claims
- Status: {'verified': 51}
- Verified by type: {'error': 17, 'rule': 15, 'capability': 9, 'state': 6, 'default': 1, 'validation': 3}
- Verifier verdicts: {'CONFIRMED': 51}

## Reconciliation
- {'AMBIGUOUS': 2, 'UNDOCUMENTED': 18, 'CONFIRMED': 77, 'CONTRADICTION': 2}

## Documents
- `product.md`: citation errors 0, grounding fixes 0, claims cited 51
- `operations-ps.md`: citation errors 0, grounding fixes 2, claims cited 42
- `customer-guide.md`: citation errors 0, grounding fixes 0, claims cited 48

## Model usage
- 114 calls, 1,580,511 input / 45,661 output / 0 thinking tokens
- By stage: {'extract': {'calls': 92, 'in': 1440397, 'out': 15566}, 'verify': {'calls': 4, 'in': 23289, 'out': 4198}, 'reconcile': {'calls': 9, 'in': 60383, 'out': 7595}, 'write': {'calls': 3, 'in': 15173, 'out': 9027}, 'ground': {'calls': 3, 'in': 16783, 'out': 259}, 'repair': {'calls': 2, 'in': 15931, 'out': 5927}, 'fix': {'calls': 1, 'in': 8555, 'out': 3089}}

## Agents and prompts

| Agent | Model | Thinking |
|---|---|---|
| claim_extractor | gemini-3.8-flash | low |
| claim_verifier | gemini-3.8-flash | low |
| reconciler | gemini-3.8-flash | low |
| doc_writer | gemini-3.8-flash | low |
| grounding_checker | gemini-3.8-flash | low |

Prompt versions (layers used, content hash):

- `context/product` d516f4d7f4e3: registry:opentcs v1
- `prompts/claim_extractor` 2c426ebad56c: claim_extractor.md@default, claim-schema.md@default
- `prompts/claim_verifier` 729857f24ef4: claim_verifier.md@default
- `prompts/doc_writer` 395bdfa4cfae: doc_writer.md@default
- `prompts/grounding_checker` 7dee035eaa13: grounding_checker.md@default
- `prompts/reconciler` 1404066e28f6: reconciler.md@default, reconciliation.md@default
- `templates/customer-guide` 8ad4f12e0e11: customer-guide.md@default, _disposition.md@default
- `templates/operations-ps` dfad8efabc44: operations-ps.md@default, _disposition.md@default
- `templates/product` 5701cebfd425: product.md@default, _disposition.md@default

## Choices made automatically
- Added boundary `org.opentcs.strategies`: it dominated the slice; its internals are described only from the outside.
- Added boundary `org.opentcs.data`: it dominated the slice; its internals are described only from the outside.
