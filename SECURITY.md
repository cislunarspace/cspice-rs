# 安全策略

## 报告漏洞

请勿通过公开 issue 报告安全漏洞。请使用 GitHub 私密安全公告（Security → Advisories → Report a vulnerability），或联系仓库维护者。

修复期间请勿公开披露细节；我们会在修复发布后致谢报告者（除非要求匿名）。

## 支持范围

- 本仓库的 Rust 绑定代码（`cspice/`、`cspice-sys/`、构建脚本、CI）。
- CSPICE 工具集本体（NAIF 发布的 C 库）的缺陷请报告给 [NAIF](https://naif.jpl.nasa.gov/naif/)；绑定层的临时规避措施可在本仓库评估。

## 处理时限

- P0（内存安全、可被利用的 UB）：3 天内响应。
- 其他：7 天内响应。
