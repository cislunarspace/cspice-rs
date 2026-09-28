# Contributing Guide

Thanks for your interest in cspice-rs! This repository is an independently maintained hard fork of [jacob-pro/cspice-rs](https://github.com/jacob-pro/cspice-rs), maintained by cislunarspace.

## Development Environment

1. Rust toolchain: stable (MSRV per crate, see the `rust-version` field).
2. Install Clang/libclang (a bindgen dependency; on Windows use the official LLVM installer and set `LIBCLANG_PATH`).
3. Download the CSPICE toolkit for your platform from the [NAIF toolkit page](https://naif.jpl.nasa.gov/naif/toolkit_C.html) and unpack it, then set `CSPICE_DIR` to the `cspice` directory containing `include/` and `lib/`; on Unix platforms, first rename `lib/cspice.a` to `lib/libcspice.a`.

## Local Verification

```bash
make test          # fmt + clippy + full test suite, serialized (CSPICE global state requires --test-threads=1)
make format        # format in place
```

Cross-check LP64 platforms (no precompiled aarch64 Linux package; check only, no linking):

```bash
rustup target add aarch64-unknown-linux-gnu
CSPICE_CLANG_TARGET=aarch64-unknown-linux-gnu cargo check --workspace --target aarch64-unknown-linux-gnu
```

## Commit Convention

Commit messages use a conventional-commit type prefix plus a Chinese body:

```
fix: 修复 LP64 平台 SpiceInt 硬编码 i32
feat: 新增 pxform 安全包装
docs: 补充 aarch64 交叉验证说明
```

Allowed types: `feat`, `fix`, `docs`, `ci`, `chore`, `refactor`, `test`, `perf`. Releases are automated by release-plz based on the commit type.

## Code Conventions

- rustdoc and code comments use English; commit bodies use Chinese.
- FFI wrappers must not call CSPICE bypassing the thread lock; new safe wrappers follow the "erract=RETURN + explicit error checking" pattern in `ffi.rs`.
- CSPICE type aliases such as `SpiceInt`/`SpiceChar` must not be replaced with hard-coded fixed-width integer types (`SpiceInt` is i64 on LP64 platforms; see ADR 0001 and the aarch64 check job in CI).
- Tests must run with `--test-threads=1`; CSPICE has process-global state.

## Pull Request Process

The `main` branch is protected: merging requires a PR, passing CI, and resolved conversations; force pushes are forbidden. When changing behavior, include your local verification output in the PR description.
