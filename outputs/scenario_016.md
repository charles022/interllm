# Scenario
- Append to DataFrame: Append with batching to avoid fragmentation (Arrow Builders hybrid)
- Context: Client receives very small updates at high frequency.
- Objective: Avoid per-update vstack to reduce fragmentation and SIMD penalties.

# When to Use
- Server sends high-frequency, small-sized updates.
- Need efficient appends without per-update vstack operations.

# Recommended Approach
- Buffer rows with Arrow Builders.
- Reserve contiguous memory upfront in builders.
- Batch flush based on row count or time threshold.
- Append flushed batches to the main table.

# Implementation Outline
- Initialize Arrow Builders with reserved capacity.
- Set row-count and/or time-based flush thresholds.
- Buffer incoming updates in builders.
- Flush to a batch on threshold and append to the main DataFrame.
- Repeat buffering and flushing for subsequent updates.

# Tradeoffs/Risks
- Reduces fragmentation and SIMD penalties.
- Requires careful threshold tuning to balance memory overhead and latency.

# Validation Checklist
- [ ] Confirm builders reserve contiguous memory upfront.
- [ ] Ensure flush thresholds are tuned for the workload.
- [ ] Verify efficient appends without per-update vstack operations.
