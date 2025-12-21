# Scenario 5.2 guidance: chunked streaming assembly

## Scenario
- Server sends the total payload size before streaming.
- Payload arrives in fixed-size chunks.
- Each chunk is appended to a Rust/WASM buffer or builder object.
- JS clears the chunk buffer after handoff.
- Repeat until the full payload is assembled.
- Avoids double allocation during JS-to-WASM copying and keeps memory stable.

## When to Use
- Memory efficiency is a hard requirement.
- Payloads are large enough to require streaming.
- WebAssembly handles the assembly work for performance reasons.

## Recommended Approach
- Read and validate total payload size up front.
- Pre-size or reserve the Rust/WASM target buffer when possible.
- Copy chunk bytes into the Rust/WASM object immediately on receipt.
- Clear or reuse the JS chunk buffer right after the handoff.
- Track total bytes written and stop exactly at the declared size.

## Implementation Outline
- Server includes total size (header or first control message).
- Client allocates/initializes a Rust/WASM assembly object with capacity.
- JS receives a fixed-size chunk and hands it to Rust/WASM.
- Rust/WASM appends bytes at the current offset and updates counters.
- JS clears or reuses the chunk buffer.
- Loop until the total size is reached; finalize the assembled payload.

## Tradeoffs/Risks
- Requires strict bookkeeping across JS and Rust/WASM boundaries.
- Incorrect size or offset tracking can corrupt the assembled data.
- Mismanaged buffer clearing can lead to leaks or stalled GC.

## Validation Checklist
- [ ] Total size is received and validated before streaming begins.
- [ ] Rust/WASM appends chunks with correct offsets and no reallocation spikes.
- [ ] JS chunk buffers are cleared or reused immediately after transfer.
- [ ] Assembled payload size matches the declared total size.
- [ ] Memory usage remains stable across the full transfer.
