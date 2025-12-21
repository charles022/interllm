# Scenario 1.3 guidance: fixed-size shared struct

Use this when you need a stable, predictable memory layout and can avoid dynamic allocation. Keep the payload a POD snapshot that is immutable after serialization.

## Implementation
- Define a struct using fixed-width primitives (u8/u16/u32/u64, i32, f32/f64) and fixed arrays for strings and buffers.
- Lock the layout with repr(C) or the equivalent for your language; add explicit padding fields to control alignment.
- Include a schema version and struct size field to guard against mismatch.
- Add compile-time or build-time size and alignment checks.

## Transfer
- Serialize the struct as raw bytes; document endianness (for example, little) and IEEE-754 for floats.
- Fill fields once, then transmit an immutable snapshot.

## Client mapping
- Validate payload size and version before mapping.
- If alignment differs, copy into an aligned buffer before casting; otherwise zero-copy is acceptable.
- Provide explicit translation for any higher-level types that do not deserialize predictably.

## Tradeoffs and risks
- Structure changes require synchronized updates on server and client.
- Platform-specific alignment and packing can break compatibility.
- Fixed arrays limit flexibility and may waste space.

## Validation checklist
- [ ] Verify explicit sizes, alignment, and padding.
- [ ] Test on all target platforms for layout parity.
- [ ] Confirm immutability and checksum or hash for corruption detection.
