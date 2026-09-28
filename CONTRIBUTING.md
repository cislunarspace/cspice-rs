# 贡献指南

感谢关注 cspice-rs！本仓库是 [jacob-pro/cspice-rs](https://github.com/jacob-pro/cspice-rs) 的独立维护 hard fork，由 cislunarspace 维护。

## 开发环境

1. Rust 工具链：stable（MSRV 见各 crate 的 `rust-version` 字段）。
2. 安装 Clang/libclang（bindgen 依赖，Windows 可用 LLVM 官方安装器并设置 `LIBCLANG_PATH`）。
3. 从 [NAIF toolkit 页面](https://naif.jpl.nasa.gov/naif/toolkit_C.html) 下载对应平台的 CSPICE 编译包并解压，设置 `CSPICE_DIR` 指向含 `include/` 与 `lib/` 的 `cspice` 目录；Unix 平台需先把 `lib/cspice.a` 重命名为 `lib/libcspice.a`。

## 本地验证

```bash
make test          # fmt + clippy + 串行全量测试（CSPICE 全局状态要求 --test-threads=1）
make format        # 就地格式化
```

交叉验证 LP64 平台（无 aarch64 Linux 预编译包，只做 check 不链接）：

```bash
rustup target add aarch64-unknown-linux-gnu
CSPICE_CLANG_TARGET=aarch64-unknown-linux-gnu cargo check --workspace --target aarch64-unknown-linux-gnu
```

## 提交规范

commit message 使用 conventional commit type 前缀 + 中文正文：

```
fix: 修复 LP64 平台 SpiceInt 硬编码 i32
feat: 新增 pxform 安全包装
docs: 补充 aarch64 交叉验证说明
```

允许的 type：`feat`、`fix`、`docs`、`ci`、`chore`、`refactor`、`test`、`perf`。版本发布由 release-plz 依据 commit type 自动化。

## 代码约定

- rustdoc 与代码注释使用英文；README、issue、commit 正文使用中文。
- FFI 包装不得绕过线程锁直接调用 CSPICE；新增安全包装遵循 `ffi.rs` 的 "erract=RETURN + 显式错误检查" 模式。
- `SpiceInt`/`SpiceChar` 等 CSPICE 类型别名不得用固定宽度整型硬编码替代（LP64 平台 `SpiceInt` 为 i64，见 ADR 0001 与 CI 的 aarch64 check job）。
- 测试必须 `--test-threads=1` 运行；CSPICE 有进程级全局状态。

## PR 流程

主干分支 `main` 受保护：需 PR、CI 通过且会话解决后方可合并，禁止 force push。改动行为时在 PR 描述中给出本地验证输出。
