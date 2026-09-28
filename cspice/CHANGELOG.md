# Changelog

## [0.1.0](https://github.com/cislunarspace/cspice-rs/releases/tag/cspice-rs-v0.1.0) - 2026-09-28

### 其他

- feat/ffi-safe-wrappers (PR #13 内容并入独立仓库基线)
- Add safe FFI wrappers (pxform/sxform/bodvrd/et2utc/ktotal) with explicit error handling
- Release cspice-sys 1.0.4, allow cspice usage from different threads ([#8](https://github.com/cislunarspace/cspice-rs/pull/8))
- Prepare for 0.0.1 release ([#6](https://github.com/cislunarspace/cspice-rs/pull/6))
- Adjust SPK position and velocity return types ([#5](https://github.com/cislunarspace/cspice-rs/pull/5))
- Added AzEl coordinate system. ([#1](https://github.com/cislunarspace/cspice-rs/pull/1))
- Added spkez_c, spkezp_c, and spkezr_c implementations ([#3](https://github.com/cislunarspace/cspice-rs/pull/3))
- Add Eq derives where available ([#2](https://github.com/cislunarspace/cspice-rs/pull/2))
- pub use error
- improve thread error message
- update README.md
- add gfsep_c
- improve docs
- add vsep_c
- SpiceFrom
- add spkpos_c and recrad_c
- use static strings
- window functions
- move some functions
- add cell
- inline
- support spice utc zone
- improve modules and docs
- add zone support
- str2et_c
- StringParam
- use strings on stack
- more time
- time WIP
- faster conversion
- use more efficient strings
- set/get error device
- string & error handling

### 持续集成

- 三平台测试、aarch64 交叉检查、MSRV 与 release-plz 工作流

### 新增功能

- 独立仓库基线——crate 更名、依赖升级与 LP64 修复

### 缺陷修复

- fix time conversion

### 重构

- refactor pt2
- refactor
