# ADR 0001: Hard fork jacob-pro/cspice-rs as a standalone repository

- Status: Accepted
- Date: 2026-10-24
- Decision makers: cislunarspace maintainers

## Context

[jacob-pro/cspice-rs](https://github.com/jacob-pro/cspice-rs) is one of the Rust community's primary bindings to NAIF CSPICE (crates `cspice` 0.1.0 / `cspice-sys` 1.0.4), but has had no commits and no issue handling since 2022. When e2m2e (CODE-core) extended its Earth–Moon space algorithm stack, which depends on this library, to aarch64 Linux and LP64 platforms, upstream defects surfaced:

1. The invalid `&[u8] -> *const [i8]` cast in `cspice/src/string.rs` fails to compile outright on aarch64 (E0606);
2. Three places in `spk.rs` and `time/julian_date.rs` hard-code `i32` where a `SpiceInt` is expected (i64 on LP64 platforms, E0308), so Linux/macOS cannot compile.

We submitted PR #12 (aarch64 cast fix) and PR #13 (`cspice::ffi` safe wrappers) upstream and set a waiting deadline based on the upstream maintainer's historical response cadence. The deadline passed without a response, making long-term independent maintenance of the fork inevitable.

## Decision

Establish a brand-new standalone repository `cislunarspace/cspice-rs` (not a GitHub fork; the full git history is preserved) and maintain it independently as a hard fork:

- Baseline = upstream master (736cc37) + the equivalent content of PR #12 + PR #13;
- Crates renamed `cspice-rs` / `cspice-rs-sys` (the original names are taken on crates.io by upstream, see ADR 0002); the version line restarts from 0.1.0, inherits no upstream version numbers, and promises no compatibility with them;
- The three LP64 fixes and the aarch64 cross-check (ADR 0003) land with the baseline;
- LGPL-3.0 and the original author's attribution are preserved as-is.

## Consequences

- Positive: LP64/aarch64 defects become immediately controllable; release cadence, CI, and maintenance strategy are under our own control.
- Negative: We compete with upstream (should it revive) and with the crates.io crates under the original names, so provenance and acknowledgements must be stated clearly in the README; consumers (including CODE-core) need a one-time dependency switch.
- Once the new repository contains the equivalent of upstream PR #12/#13, those PRs lose urgency; if upstream responds later, preferentially push the new repository's incremental fixes back upstream — the two directions do not conflict.

## Revision History

- 2026-10-24: First recorded.
- 2026-09: Translated to English for public release.
