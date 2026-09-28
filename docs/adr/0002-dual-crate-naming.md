# ADR 0002: Dual-crate naming: cspice-rs / cspice-rs-sys

- Status: Accepted
- Date: 2026-10-24
- Decision makers: cislunarspace maintainers

## Context

Upstream occupies `cspice` (0.1.0) and `cspice-sys` (1.0.4) on crates.io. A standalone repository cannot publish under those names, while inventing an unrelated new name (e.g. `naif-bindings`) would break search discoverability. Meanwhile, e2m2e uses both layers simultaneously: the numerics crate uses the `-sys` layer's custom FFI symbols directly, while the orchestration layer uses the safe wrapper layer.

## Decision

Keep the dual-crate structure of the Rust ecosystem's "-sys convention", with a uniform `-rs` suffix added:

| Directory | Package name | Lib name (`use` path) | Responsibility |
|---|---|---|---|
| `cspice-sys/` | `cspice-rs-sys` | `cspice_rs_sys` | Raw bindgen-generated FFI bindings |
| `cspice/` | `cspice-rs` | `cspice_rs` | Safe wrapper layer |

- Directory names stay as upstream had them (`cspice/`, `cspice-sys/`), minimizing historical diff noise;
- Both crates start from version 0.1.0: the names differ, and there is no compatibility promise with upstream `cspice` 0.1 / `cspice-sys` 1.0.4;
- `cspice-rs-sys` is published first (`cspice-rs` depends on it via path + version).

## Consequences

- Positive: discoverable by searching "cspice" on crates.io; the directory layout minimizes diff against upstream; both usage modes (sys only / full stack) work.
- Negative: `use cspice_rs::` versus the crate name `cspice-rs` carries the mental burden of the hyphen-to-underscore mapping; the fact that the crate names do not exactly match upstream must be made clear in the README.
- Consumers migrating from upstream need a mechanical replacement of `use` paths (CODE-core's switch is recorded in that repository's ADR).

## Revision History

- 2026-10-24: First recorded.
- 2026-09: Translated to English for public release.
