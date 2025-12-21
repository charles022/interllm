# Scenario 3 guidance: replace entire client DataFrame (full refresh)

## Scenario
- The Rust/WASM client contains an existing DataFrame.
- The server provides a full replacement dataset.
- The replacement is in the same Arrow format as the original.

## When to Use
- Use when the entire dataset becomes outdated or corrupt.
- Ideal for periodic complete refreshes to ensure data integrity.

## Recommended Approach
- Validate new data from the server to match existing schema.
- Clear the current DataFrame in the client.
- Load the new DataFrame received from the server.

## Implementation Outline
- Step 1: Receive new data.
- Step 2: Verify that schema matches existing DataFrame.
- Step 3: Clear or replace current DataFrame.
- Step 4: Load new data into the DataFrame.

## Tradeoffs/Risks
- Tradeoff: Full refresh might be resource-intensive depending on data size.
- Risk: Potential data loss during transition.

## Validation Checklist
- Ensure that the schema of the new dataset is verified.
- Confirm that the DataFrame is entirely replaced without residuals.
- Validate performance implications on client operations.
