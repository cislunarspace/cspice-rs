# ADR 0002：双 crate 命名 cspice-rs / cspice-rs-sys

- 状态：已接受
- 日期：2026-10-24
- 决策人：cislunarspace 维护者

## 背景

上游在 crates.io 占用 `cspice`（0.1.0）与 `cspice-sys`（1.0.4）。独立仓库无法用原名发布；硬造无关新名（如 `naif-bindings`）会割裂搜索关联。同时 e2m2e 同时使用两层：数值 crate 直接用 `-sys` 层的自定义 FFI 符号，编排层用安全包装层。

## 决策

沿用 Rust 生态 "-sys 惯例" 的双 crate 结构，统一加 `-rs` 后缀：

|目录|package 名|lib 名（`use` 路径）|职责|
|---|---|---|---|
|`cspice-sys/`|`cspice-rs-sys`|`cspice_rs_sys`|bindgen 生成的裸 FFI 绑定|
|`cspice/`|`cspice-rs`|`cspice_rs`|安全包装层|

- 目录名保持上游原样（`cspice/`、`cspice-sys/`），减少历史 diff 噪音；
- 两个 crate 均从 0.1.0 起版：名字不同，与上游 `cspice` 0.1 / `cspice-sys` 1.0.4 无兼容承诺；
- `cspice-rs-sys` 先发布（`cspice-rs` 对其是 path + version 双依赖）。

## 后果

- 正面： crates.io 搜索 "cspice" 可发现；目录布局与上游 diff 最小；双栖用法（仅 sys / 全栈）都成立。
- 负面：`use cspice_rs::` 与 crate 名 `cspice-rs` 存在连字符-下划线映射的心智负担；与上游 crate 名不完全一致的表述需在 README 讲清。
- 使用方从上游迁移需机械替换 `use` 路径（CODE-core 的切换见其仓库 ADR）。

## 修订记录

- 2026-10-24：首次记录。
