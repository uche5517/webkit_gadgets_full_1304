# WebKit ROP Gadgets — PS4 Firmware 13.04

Full ROP gadget dump from the **13.04 `libSceNKWebKit.sprx.decrypted`** (68 MB) binary, generated with **ROPgadget v7.7** (Capstone disassembler).

```
source   : libSceNKWebKit.sprx.decrypted  (68 MB, SHA256 48FA2ACE61DFB459...)
tool     : ROPgadget v7.7
command  : ROPgadget --binary libSceNKWebKit.sprx.decrypted --depth 8 --only "pop|ret|leave"
entries  : 10,394 unique gadgets
```

## Important

- **Addresses are file RVAs (virtual addresses), not file offsets.**
  This SPRX ELF maps with `vaddr = fileoff - 0x4000`, so to reference a gadget from a
  file offset add `0x4000`, e.g. the `pop rdi ; ret` at RVA `0x5c480` lives at
  file offset `0x60480`.
- This is the **WebKit** binary (the JS/JS engine module the browser process loads).
  It is a different module from `libkernel_web.sprx` — do not mix the two address spaces.
- Sony did **not** update WebKit for 13.x, so these RVAs are identical for
  13.00/13.02/13.04 and are assumed shared with 13.50/13.52.

## Verified "hot" gadgets

These byte-verified gadgets are used by the runtime ROP chain and are confirmed
both by Python byte-pattern scan and ROPgadget disassembly at `RVA + 0x4000`:

| gadget                     | RVA        | bytes     |
|----------------------------|------------|-----------|
| `pop rdi ; ret`            | `0x005c480`| `5f c3`   |
| `pop rsi ; ret`            | `0x006e45e`| `5e c3`   |
| `pop rdx ; ret`            | `0x012c5ba`| `5a c3`   |
| `pop rcx ; ret`            | `0x001bade`| `59 c3`   |
| `pop rax ; ret`            | `0x0010504`| `58 c3`   |
| `pop r8 ; ret`             | `0x009b311`| `47 58 c3`|
| `pop r9 ; ret`             | `0x01dcfb1`| `47 59 c3`|
| `leave ; ret`              | `0x00182f7`| `c9 c3`   |
| `mov qword ptr [rdi], rax ; ret` | `0x000548b` | `48 89 07 c3` |
| `push rdx ; pop rsp ; ret` (pivot) | `0x2abccaa` | `52 5c c3` |
