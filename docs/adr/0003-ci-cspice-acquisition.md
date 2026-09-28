# ADR 0003: CSPICE acquisition strategy for CI and the aarch64 verification boundary

- Status: Accepted
- Date: 2026-10-24
- Decision makers: cislunarspace maintainers

## Context

Building `cspice-rs-sys` requires the CSPICE headers (bindgen) and the static library (link tests). Local developers provide them manually via `CSPICE_DIR`; CI must fetch them automatically. Constraints:

1. NAIF does not provide a precompiled aarch64 Linux package;
2. The reachability of NAIF servers from CI runners is outside our control;
3. The historical defects of the bindings are exactly of the platform-difference kind (E0606 on aarch64, LP64 `SpiceInt`), so multi-platform gates are mandatory.

## Decision

- **Regular acquisition**: CI downloads version-pinned precompiled packages for three platforms from NAIF (`PC_Linux_GCC_64bit`, `MacM1_OSX_clang_64bit`, `PC_Windows_VisualC_64bit`); the version number is written into the workflow env, and `actions/cache` caches with a "package name + version" key; after unpacking, Unix packages have `lib/cspice.a` renamed to `lib/libcspice.a`.
- **The `downloadcspice` feature is not used in CI**: it downloads over the network on every clean build with an unpinned version, which does not satisfy reproducible gating; it is kept only for one-off local experiments.
- **aarch64-linux is forever cross-`cargo check` only**: bindgen needs the headers but not linking (the x64 package's `include/` suffices); combined with `CSPICE_CLANG_TARGET=aarch64-unknown-linux-gnu` it generates bindings from an LP64 perspective, which is enough to catch E0606/E0308-class defects at compile time; link-level and runtime behavior cannot be covered because NAIF publishes no precompiled package for that platform — this boundary is permanently recorded here.
- **Fallback**: when NAIF is unreachable, CI pulls the CODE-core `cspice-v1` release assets instead (that URL is likewise written into the workflow env and marked as fallback-only). The fallback assets cover Linux x64/aarch64 and Windows x64; there is no macOS package, so a macOS failure can only be retried while waiting for NAIF to recover.

## Consequences

- Positive: platform-difference defects (the reason this repository exists) are continuously intercepted by CI; NAIF outages no longer block CI.
- Negative: aarch64-linux link-level and runtime correctness have no gate, relying on the argument that LP64 and aarch64 share integer widths; upgrading the NAIF package requires manually changing the version number.
- If NAIF ships an aarch64 Linux package in the future, this should be upgraded to a full test job and this ADR revised.

## Revision History

- 2026-10-24: First recorded.
- 2026-09: Translated to English for public release.
