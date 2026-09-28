# ADR 0003：CI 的 CSPICE 获取策略与 aarch64 验证边界

- 状态：已接受
- 日期：2026-10-24
- 决策人：cislunarspace 维护者

## 背景

`cspice-rs-sys` 构建需要 CSPICE 头文件（bindgen）与静态库（链接测试）。本地开发者以 `CSPICE_DIR` 手动提供；CI 需要自动获取。约束：

1. NAIF 不提供 aarch64 Linux 预编译包；
2. NAIF 服务器对 CI runner 的可达性不受我们控制；
3. 绑定的历史缺陷恰是平台差异类（E0606 aarch64、LP64 `SpiceInt`），必须有多平台门禁。

## 决策

- **常规获取**：CI 从 NAIF 下载固定版本的三平台预编译包（`PC_Linux_GCC_64bit`、`MacM1_OSX_clang_64bit`、`PC_Windows_VisualC_64bit`），版本号写入 workflow env，`actions/cache` 以 "包名+版本" 为 key 缓存；Unix 包解压后将 `lib/cspice.a` 重命名为 `lib/libcspice.a`。
- **`downloadcspice` feature 不用于 CI**：其每次干净构建都联网下载且版本不固定，不满足可复现门禁；仅保留给本地一次性试验。
- **aarch64-linux 永远只做交叉 `cargo check`**：bindgen 只需头文件不需链接（x64 包的 `include/` 即可），配合 `CSPICE_CLANG_TARGET=aarch64-unknown-linux-gnu` 生成 LP64 视角的绑定，足以在编译期拦截 E0606/E0308 类缺陷；链接级与运行时行为因 NAIF 无该平台预编译包而无法覆盖，此边界永久记录于此。
- **回退**：NAIF 不可达时改拉取 CODE-core `cspice-v1` release 资产（URL 同样写入 workflow env 并注明仅回退用）。回退资产覆盖 Linux x64/aarch64 与 Windows x64；无 macOS 包，macOS 失败只能重试等待 NAIF 恢复。

## 后果

- 正面：平台差异类缺陷（本仓库的建立起因）被 CI 持续拦截；NAIF 故障不再阻塞 CI。
- 负面：aarch64-linux 的链接与运行时正确性无门禁，依赖 LP64 与 aarch64 共享的整型宽度论证；NAIF 包升级需手动改版本号。
- 若 NAIF 未来提供 aarch64 Linux 包，应升级为完整 test job 并修订本 ADR。

## 修订记录

- 2026-10-24：首次记录。
