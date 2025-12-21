# Scenario 2.4 guidance: merge into persistent state - incremental state evolution - (struct, dyn)

## Scenario
- Apply updates to a struct that includes a Vec field, keeping the struct persistent and evolving incrementally.

## When to Use
- You need to apply small deltas without rebuilding the full struct.
- The Vec length can grow or shrink between updates.

## Recommended Approach
- Keep a single persistent struct instance and mutate it in place.
- Model updates as an enum or patch struct with optional fields; use pattern matching to apply variants safely.
- Update the Vec with iterators and combinators (retain, extend, splice, zip) or indexed writes.
- Guard concurrent access with Mutex/RwLock or single-threaded interior mutability in WASM.

## Implementation Outline
- Define the state struct with a Vec field and an Update enum for replace, append, remove, and patch.
- On update, match the variant and mutate scalar fields directly.
- For Vec changes, apply in-place transforms and reserve capacity before growth.
- If elements are keyed, maintain an index map and update by key to avoid full scans.

## Tradeoffs/Risks
- Update logic and concurrency control add complexity.
- Vec reallocations can move data and add churn if capacity is not managed.
- Incorrect diff application can cause state drift over time.

## Validation Checklist
- [ ] Confirm Vec updates (append, remove, replace) are applied correctly.
- [ ] Ensure state evolves correctly across multiple updates.
- [ ] Verify memory usage and allocation churn stay within bounds.
