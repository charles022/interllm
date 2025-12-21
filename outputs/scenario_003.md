## Scenario
- Shared Rust struct sent from server to client.
- Struct is variable size and may contain Vec, String, and nested data.
- Client needs mutable access, with an extra step before it becomes mutable.

## When to Use
- You need to receive a shared, variable-size Rust struct.
- The client must modify data after receipt, not just read it.
- The struct includes Vec, String, or nested fields.

## Recommended Approach
- Use the same transfer path as the read-only shared struct.
- Before mutating, perform an explicit step to obtain a mutable, owned instance.
- Apply mutations only after that extra step completes.

## Implementation Outline
- Receive the shared Rust struct from the server.
- Treat the received instance as read-only initially.
- Perform the extra step to create a mutable, owned instance.
- Mutate Vec, String, or nested fields as needed.

## Tradeoffs/Risks
- The extra step may introduce copying or allocation overhead.
- Mutable edits can invalidate assumptions made for read-only access.

## Validation Checklist
- The struct received from the server matches the expected shape.
- The extra step occurs before any mutation.
- Mutations to Vec, String, and nested fields work as intended.
