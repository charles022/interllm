# Scenario
- Insert into DataFrame.
- Context: Client holds a DataFrame; server sends inserts plus deletions.
- Objective: Apply inserts and removals so the client matches server state.

# When to Use
- Sync client tables to server state without a full refresh.
- Payloads include both new rows and removals.
- Consistency matters; updates must be atomic.

# Recommended Approach
- Receive a batch payload with insert rows and delete keys.
- Parse into DataFrame-friendly structures (Arrow/Polars).
- Apply inserts and deletes in a transactional sequence.
- Batch operations to reduce fragmentation and reallocation.

# Implementation Outline
- Validate payload schema and version/epoch.
- Start a transaction or stage updates in a new table.
- Insert new rows (buffer then append or upsert by key).
- Remove rows using key-based filters or an anti-join.
- Commit by swapping the updated table into place.
- Roll back on error and report failure.

# Tradeoffs/Risks
- Large batches can spike memory and CPU.
- Incorrect delete keys or non-atomic apply can corrupt state.
- Requires stable row identifiers for deletes and upserts.

# Validation Checklist
- [ ] Inserted rows appear exactly once.
- [ ] Deleted rows are no longer present.
- [ ] Row counts or checksums match the server state.
- [ ] Failure paths leave the original table intact.
