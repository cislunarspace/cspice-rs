# AGENTS.md

cspice-rs 的 agent 协作约定:NAIF CSPICE 工具库的 Rust 绑定,双 crate 结构。

## Project Overview

对 NAIF CSPICE(C 语言的航天动力学内核库)提供 Rust 绑定:底层 unsafe FFI + 上层安全抽象(线程锁、错误翻译)。维护自 jacob-pro/cspice-rs 的 hard fork(上游 2022 年起无维护)。

## Architecture & Data Flow

两个 crate 单向依赖:`cspice-sys`(bindgen 生成的 raw FFI,build.rs 在构建期生成绑定,需 CSPICE_DIR 或 downloadcspice feature)← `cspice`(安全封装)。上层通过 FFI 调用 CSPICE 例程,把 C 错误状态翻译为 Rust `Result`。

## Key Directories

| 路径 | 用途 |
|---|---|
| `cspice-sys` | raw unsafe FFI 绑定(build.rs + bindgen) |
| `cspice` | 安全封装层 |
| `cspice/test_data` | 测试数据 |
| `docs` | 文档 |
| `.github/actions/setup-cspice` | CI 用的 CSPICE 安装 action |
| `release-plz.toml` | 发布自动化配置 |

## Development Commands

构建需先备好 CSPICE(CSPICE_DIR 指向含 include/ 与 lib/ 的目录)。测试/lint:`make test`(cargo fmt --check、cargo-sort --check、clippy -D warnings、test --test-threads=1),或看 CI(.github/workflows/ci.yml,CI 会自动安装 CSPICE)。

## 约定

改动需有测试;提交信息用中文;遵循既有命名与两 crate 的层次边界(sys 不依赖安全层)。
