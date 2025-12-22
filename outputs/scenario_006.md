## Scenario
- Client maintains a large persistent state while the server streams small updates that must be merged in place to evolve tables and structs without relocating the whole state.

## When to Use
- When update payloads are small compared with the persistent baseline and downtime or memory churn from full replacements is unacceptable.
- When both static and dynamic structures require gradual evolution without reallocating backing storage.

## Recommended Approach
- Preserve the existing memory layout; map each update to precise offsets within the persistent state.
- Maintain schema metadata (field positions, table indices) so updates resolve directly to target slots.
- Buffer incoming updates just long enough to validate structure before committing them in place.

## Implementation Outline
- Identify the affected table or struct segment using stored metadata.
- Validate update compatibility (type, bounds, cardinality) against the persistent state.
- Apply field-level writes or row-level patches directly, avoiding bulk copies.
- Record minimal change markers to support recovery if an update fails mid-merge.

## Tradeoffs/Risks
- In-place mutation can leave the state partially updated if error handling or recovery markers are missing.
- Schema drift must be controlled to prevent misaligned writes.
- Gap: scenario does not specify concurrency handling for simultaneous updates.

## Validation Checklist
- Confirm metadata accuracy prior to each merge operation.
- Verify applied updates preserve table/struct invariants and expected sizes.
- Ensure recovery markers are cleared after successful commits.
- Stress-test incremental merges for both static and dynamic structures to confirm no reallocations occur.
