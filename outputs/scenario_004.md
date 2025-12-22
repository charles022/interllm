## Scenario
- Server supplies a fixed-layout POD struct as shared read-only state.
- Content is restricted to fixed-size arrays; no Vec or String usage is permissible.
- Specific field definitions and the transport protocol are not provided (gap).

## When to Use
- When both endpoints require deterministic binary layout with no runtime allocation.
- When read-only access suffices and update cadence is controlled by the server.

## Recommended Approach
- Share a single struct definition across components using consistent alignment and packing directives.
- Restrict client interactions to const views, avoiding mutation or copies beyond required alignment safety.
- Document array bounds and expected encoding explicitly since dynamic metadata is unavailable.

## Implementation Outline
- Acquire the authoritative struct schema from the server team (missing detail to obtain).
- Generate language-specific bindings that honor POD semantics and fixed array extents.
- Implement a deserializer that performs direct memory mapping or memcpy without heap allocation.
- Validate struct size, alignment, and checksum or version before exposing data to consumers.

## Tradeoffs/Risks
- Any schema change requires coordinated redeployment because layout is baked into binaries.
- Lack of dynamic fields limits extensibility; additional data mandates new arrays or struct revisions.
- Misaligned packing directives can corrupt interpretation of the shared state.

## Validation Checklist
- Struct size and alignment match the server contract.
- Fixed array lengths are verified against the specification.
- Memory access remains read-only throughout the code path.
- Versioning or checksum of incoming data is confirmed.
