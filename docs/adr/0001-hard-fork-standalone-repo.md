# ADR 0001：以独立仓库 hard fork jacob-pro/cspice-rs

- 状态：已接受
- 日期：2026-10-24
- 决策人：cislunarspace 维护者

## 背景

[jacob-pro/cspice-rs](https://github.com/jacob-pro/cspice-rs) 是 Rust 社区对 NAIF CSPICE 的主要绑定之一（crate `cspice` 0.1.0 / `cspice-sys` 1.0.4），但自 2022 年起无提交、无 issue 处理。e2m2e（CODE-core）依赖该库的地月空间算法栈在向 aarch64 Linux 与 LP64 平台扩展时暴露了上游缺陷：

1. `cspice/src/string.rs` 的 `&[u8] -> *const [i8]` 无效 cast 在 aarch64 上直接编译失败（E0606）；
2. `spk.rs` 与 `time/julian_date.rs` 三处硬编码 `i32` 传入 `SpiceInt`（LP64 平台为 i64，E0308），Linux/macOS 无法编译。

我们向上游提交了 PR #12（aarch64 cast 修复）与 PR #13（`cspice::ffi` 安全包装），并按上游维护者历史响应节奏设定了等待期限。期限届满无响应，fork 转为长期独立维护已成必然。

## 决策

建立全新独立仓库 `cislunarspace/cspice-rs`（非 GitHub fork，保留完整 git 历史），以 hard fork 方式独立维护：

- 基线 = 上游 master（736cc37）+ PR #12 + PR #13 等价内容；
- crate 更名 `cspice-rs` / `cspice-rs-sys`（原名在 crates.io 被上游占用，见 ADR 0002），版本线从 0.1.0 重新起步，不继承上游版本号，也不承诺与其兼容；
- LP64 三处修复与 aarch64 交叉检查（ADR 0003）随基线入库；
- LGPL-3.0 与原作者署名原样保留。

## 后果

- 正面：LP64/aarch64 缺陷立即可控；发布节奏、CI 与维护策略自主。
- 负面：与上游（若复活）及 crates.io 原名 crate 形成竞争关系，需在 README 明确来源与致谢；使用方（含 CODE-core）需一次依赖切换。
- 上游 PR #12/#13 在新仓库有等价内容后即失去紧迫性；若上游日后响应，优先把新仓库的增量修复回传上游，双向不冲突。

## 修订记录

- 2026-10-24：首次记录。
