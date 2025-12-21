# Guidance Note

## Scenario
Append new data to an existing DataFrame (incremental growth)

## When to Use
- When a client maintains an existing Polars DataFrame.
- When a server provides additional rows of data since the last update.

## Recommended Approach
- Use Polars specific functions to append new rows to the existing DataFrame efficiently.

## Implementation Outline
- Receive the new rows from the server.
- Convert the new data into a Polars DataFrame if not already in that format.
- Use the `vstack` or `concat` function in Polars to append the new rows to the existing DataFrame.
- Ensure that the column schema of the new data matches the existing DataFrame.

## Tradeoffs/Risks
- Care must be taken to ensure schema compatibility to avoid runtime errors.
- Incremental updates may lead to fragmentation over time, requiring periodic optimizations.

## Validation Checklist
- [ ] Confirm that the existing DataFrame and new data have compatible schemas.
- [ ] Verify that data is appended in the correct order.
- [ ] Ensure no data loss during the append operation.
- [ ] Conduct performance testing to assess any potential degradation.
