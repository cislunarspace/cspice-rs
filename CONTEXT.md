# 术语表

本文件是 cspice-rs 的唯一领域术语表。术语在此定名后，代码注释、文档、issue 与 ADR 用词保持一致；新增术语先入此表再使用。中文文本统一使用弯引号 ""，不用直角引号；英文文档使用英文标点。

## SPICE

NASA JPL 的行星际任务几何计算工具集（Spacecraft, Planet, Instrument, C-matrix, Event）。本仓库绑定其 C 语言发行版 CSPICE。

## NAIF

Navigation and Ancillary Information Facility，SPICE 的开发与发布机构。CSPICE 预编译包从其 [toolkit 页面](https://naif.jpl.nasa.gov/naif/toolkit_C.html) 下载。

## kernel（内核文件）

SPICE 的数据文件（星历 `.bsp`、 leap seconds `.tls`、帧定义 `.tf` 等）。加载用 `furnsh_c`，卸载用 `unload_c`；测试内核在 `cspice/test_data/`。

## ET / TDB

Ephemeris Time，即 Barycentric Dynamical Time（TDB），单位为 J2000 历元起的秒。SPICE 时间接口的统一中间表示，对应 `cspice_rs::time::Et`。

## safe wrapper（安全包装）

在 `cspice_rs_sys` 的 FFI 之上封装的 Rust 函数：负责缓冲区分配、字符串转换、线程锁与错误翻译，调用方不接触裸指针。集中模式见 `cspice/src/ffi.rs`。

## ffi module（ffi 模块）

`cspice_rs::ffi`：后加入的安全包装集合（pxform/sxform/bodvrd/et2utc/ktotal 等），以 "erract=RETURN + errdev=NULL + 显式错误检查" 模式处理错误，错误类型为 `SpiceFfiError`。

## SpiceInt / LP64

CSPICE 的整数类型别名。在 LP64 平台（Linux/macOS）上 `SpiceInt` 是 `long`（64 位），在 Windows（LLP64）上是 32 位。绑定代码必须使用类型别名或显式 cast，不得硬编码 `i32`；aarch64-linux 交叉 check 用于在 CI 中持续拦截此类缺陷。

## erract=RETURN / errdev=NULL

CSPICE 错误处理模式：出错时不打印、不中止，而是记录并立即返回，由包装层经 `failed_c`/`getmsg_c` 显式检查并翻译为 Rust 错误。在首次 FFI 调用前以 `Once` 初始化。

## downloadcspice feature

`cspice-rs-sys` 的可选 feature：构建时从 NAIF 服务器自动下载 CSPICE。每次干净构建都要联网且耗时，仅建议一次性本地试验使用。
