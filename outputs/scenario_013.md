# Scenario 3.2 guidance: replace DataFrame with same fixed row count

## Scenario
- Replace a client DataFrame with a new DataFrame of the same fixed row count.
- The server provides a full replacement dataset with identical row cardinality.
- Column schema is consistent or can be mapped without changing row count.

## When to Use
- Row count must remain constant for UI layout, indexing, or buffer reuse.
- Batch or time-series updates require a full refresh without resizing.
- You need deterministic row-to-row correspondence across refreshes.

## Recommended Approach
- Validate that old and new DataFrames have identical row counts before swap.
- Verify column types and ordering; apply explicit mapping if needed.
- Prefer atomic replacement (swap or in-place update) to avoid partial state.

## Implementation Outline
- Step 1: Receive the replacement DataFrame.
- Step 2: Compare row counts; fail fast on mismatch.
- Step 3: Align column names, order, and types as required.
- Step 4: Replace the existing DataFrame using assignment or in-place update.
- Step 5: Run post-replacement integrity checks.

## Tradeoffs/Risks
- Strict row count checks can reject valid updates if upstream is inconsistent.
- Column mapping or casting adds overhead for large tables.
- Non-atomic updates can expose mixed old/new data to consumers.

## Validation Checklist
- [ ] Old and new row counts match exactly.
- [ ] Column schema and ordering are confirmed or mapped.
- [ ] Replacement completes without errors.
- [ ] Spot checks or summary stats confirm data integrity.
