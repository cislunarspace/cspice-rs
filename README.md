# cspice-rs

Rust 对 [NAIF CSPICE](https://naif.jpl.nasa.gov/naif/index.html) 工具集的绑定，由 cislunarspace 维护。

[![CI](https://github.com/cislunarspace/cspice-rs/actions/workflows/ci.yml/badge.svg)](https://github.com/cislunarspace/cspice-rs/actions/workflows/ci.yml)
![maintenance-status](https://img.shields.io/badge/maintenance-actively--developed-blue.svg)

- [cspice-rs-sys](./cspice-sys)：bindgen 生成的 CSPICE 原始 FFI 绑定（unsafe）。
- [cspice-rs](./cspice)：在 FFI 之上以 Rust 抽象封装的安全接口，含线程锁与错误翻译。

## 来源与致谢

本仓库 hard fork 自 [jacob-pro/cspice-rs](https://github.com/jacob-pro/cspice-rs)（LGPL-3.0，作者 Jacob Halsey），保留了完整 git 历史。上游自 2022 年后停止维护，我们提交的修复（aarch64 上的 E0606 无效 cast、`cspice::ffi` 安全包装）长期无法进入上游，因此建立独立维护的仓库继续演进，并向原作者的无偿开源工作致谢。

上游提交至 jacob-pro/cspice-rs 的 [PR #12](https://github.com/jacob-pro/cspice-rs/pull/12) 与 [PR #13](https://github.com/jacob-pro/cspice-rs/pull/13) 的等价内容已包含在本仓库基线中。

## 安装

`cspice-rs-sys` 构建时通过 bindgen 生成绑定，需要：

1. 安装 [Clang/libclang](https://releases.llvm.org/download.html)（bindgen 依赖）。
2. 获取 CSPICE 编译包（从 [NAIF toolkit 页面](https://naif.jpl.nasa.gov/naif/toolkit_C.html) 下载对应平台版本并解压），设置环境变量 `CSPICE_DIR` 指向解压出的 `cspice` 目录（须含 `include/` 与 `lib/`）。
3. Unix 平台需将 `lib/cspice.a` 重命名为 `lib/libcspice.a`（链接器命名约定）。

也可以启用 `downloadcspice` feature 在构建时自动从 NAIF 服务器下载 CSPICE。注意：这会显著增加构建时间，且每次干净构建都需要网络连接，CI 与生产构建不建议使用。

最小依赖示例（已发布到 crates.io 后）：

```toml
[dependencies]
cspice-rs = "0.1"
```

当前以 git 依赖使用：

```toml
[dependencies]
cspice-rs = { git = "https://github.com/cislunarspace/cspice-rs", tag = "v0.1.0" }
```

## 示例

```rust
use cspice_rs::data::furnish;
use cspice_rs::spk::{self, AberrationCorrection};
use cspice_rs::time::Et;

fn main() {
    furnish("meta_kernel.tm").unwrap();
    let et = Et::from_string("2024-01-01T00:00:00").unwrap();
    // 月球（301）相对地球（399）的状态，含光行时改正
    let (state, light_time) =
        spk::easy_reader(301, et, "J2000", AberrationCorrection::LT, 399).unwrap();
    println!("月球状态: {:?}, 光行时: {}", state.position, light_time);
}
```

## 线程安全

SPICE 库本身[不是线程安全的](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/req/problems.html)。`cspice-rs` 的所有安全接口内部经可重入锁串行化；跨线程自行调用 FFI 时请使用 `with_spice_lock`。

## 参与贡献

见 [CONTRIBUTING.md](./CONTRIBUTING.md)。安全漏洞报告见 [SECURITY.md](./SECURITY.md)。领域术语见 [CONTEXT.md](./CONTEXT.md)，架构决策见 [docs/adr/](./docs/adr/)。

## 许可证

[ LGPL-3.0](./LICENSE)，继承自上游。CSPICE 工具集本身的许可见 [NAIF](https://naif.jpl.nasa.gov/naif/index.html) 页面。
