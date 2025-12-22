# Scenario
- Server shares a fixed-layout POD struct whose memory is mutated in place.
- Data fields limited to primitive scalars and fixed-size arrays; Vec and String are disallowed.
- Gap: transport mechanics, struct alignment, and endianness are not specified and must be confirmed with the server contract.

# When to Use
- When deterministic binary layout and minimal allocation overhead are required.
- When both sides can pin the struct definition at compile time and revisions are infrequent.
- When shared memory or zero-copy updates are preferable over message-based diffs.

# Recommended Approach
- Define a repr(C) struct mirroring the server-provided layout with explicit padding comments.
- Use consts for array lengths to avoid accidental resizing and enable compile-time checks.
- Employ unsafe read/write only inside well-reviewed abstractions to maintain invariants.
- Document ownership rules so concurrent writers coordinate via external synchronization.

# Implementation Outline
- Mirror the server header in a local module with unit tests verifying size_of and align_of.
- Map incoming bytes via bytemuck-style casts or ptr::read_unaligned, gated behind validation.
- Update fields by mutating the mapped struct in place under the appropriate lock.
- Provide read-only views for consumers through & references or snapshot copies when needed.

# Tradeoffs/Risks
- Tight coupling to server layout increases cost of schema evolution; versioning plan required.
- Unsafe casting errors or missed padding can corrupt state; thorough review is mandatory.
- Concurrency model unspecified; race conditions likely unless external synchronization exists.

# Validation Checklist
- Struct size, alignment, and field offsets confirmed against server specification.
- Array bounds and numeric ranges validated before each in-place mutation.
- Endianness, transport reliability, and update cadence clarified with the server team.
- Regression tests ensure mutating helper APIs preserve invariants.
