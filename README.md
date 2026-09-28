# cspice-rs

Rust bindings to the [NAIF CSPICE](https://naif.jpl.nasa.gov/naif/index.html) toolkit, maintained by cislunarspace.

**English** | [简体中文](README.zh-CN.md)

[![CI](https://github.com/cislunarspace/cspice-rs/actions/workflows/ci.yml/badge.svg)](https://github.com/cislunarspace/cspice-rs/actions/workflows/ci.yml)
![maintenance-status](https://img.shields.io/badge/maintenance-actively--developed-blue.svg)

## Contents

- [Provenance & Acknowledgements](#provenance--acknowledgements)
- [Installation](#installation)
- [Example](#example)
- [Thread Safety](#thread-safety)
- [Contributing](#contributing)
- [License](#license)

- [cspice-rs-sys](./cspice-sys): raw (unsafe) bindgen-generated FFI bindings to CSPICE.
- [cspice-rs](./cspice): safe interface wrapping the FFI in Rust abstractions, with thread locking and error translation.

## Provenance & Acknowledgements

This repository is a hard fork of [jacob-pro/cspice-rs](https://github.com/jacob-pro/cspice-rs) (LGPL-3.0, by Jacob Halsey) with the full git history preserved. Upstream has been unmaintained since 2022, and the fixes we submitted (an E0606 invalid cast on aarch64, and the `cspice::ffi` safe wrappers) could not land upstream for a long time. We therefore established an independently maintained repository to continue evolving the code, and we thank the original author for their unpaid open-source work.

The equivalents of [PR #12](https://github.com/jacob-pro/cspice-rs/pull/12) and [PR #13](https://github.com/jacob-pro/cspice-rs/pull/13) submitted to jacob-pro/cspice-rs are included in this repository's baseline.

## Installation

`cspice-rs-sys` generates bindings via bindgen at build time and requires:

1. Install [Clang/libclang](https://releases.llvm.org/download.html) (a bindgen dependency).
2. Obtain the CSPICE toolkit (download the version for your platform from the [NAIF toolkit page](https://naif.jpl.nasa.gov/naif/toolkit_C.html) and unpack it), and set the environment variable `CSPICE_DIR` to the unpacked `cspice` directory (it must contain `include/` and `lib/`).
3. On Unix platforms, rename `lib/cspice.a` to `lib/libcspice.a` (linker naming convention).

Alternatively, enable the `downloadcspice` feature to download CSPICE automatically from NAIF servers at build time. Note: this significantly increases build time and requires network access on every clean build; it is not recommended for CI or production builds.

Published on crates.io (0.1.0):

```toml
[dependencies]
cspice-rs = "0.1"
```

Or pinned to a git tag:

```toml
[dependencies]
cspice-rs = { git = "https://github.com/cislunarspace/cspice-rs", tag = "v0.1.1" }
```

## Example

```rust
use cspice_rs::data::furnish;
use cspice_rs::spk::{self, AberrationCorrection};
use cspice_rs::time::Et;

fn main() {
    furnish("meta_kernel.tm").unwrap();
    let et = Et::from_string("2024-01-01T00:00:00").unwrap();
    // State of the Moon (301) relative to Earth (399), with light-time correction
    let (state, light_time) =
        spk::easy_reader(301, et, "J2000", AberrationCorrection::LT, 399).unwrap();
    println!("Moon state: {:?}, light time: {}", state.position, light_time);
}
```

## Thread Safety

The SPICE library itself is [not thread-safe](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/req/problems.html). All safe interfaces in `cspice-rs` serialize internally through a reentrant lock; when calling the FFI yourself across threads, use `with_spice_lock`.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md). For reporting security vulnerabilities, see [SECURITY.md](./SECURITY.md). For domain terminology, see [CONTEXT.md](./CONTEXT.md); for architecture decisions, see [docs/adr/](./docs/adr/).

## License

[ LGPL-3.0](./LICENSE), inherited from upstream. For the license of the CSPICE toolkit itself, see the [NAIF](https://naif.jpl.nasa.gov/naif/index.html) page.
