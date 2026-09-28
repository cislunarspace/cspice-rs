name: 缺陷报告
description: 报告 cspice-rs 的编译失败、绑定错误或行为异常
labels: ["bug"]
body:
  - type: textarea
    id: what-happened
    attributes:
      label: 缺陷描述
      description: 发生了什么、期望是什么
    validations:
      required: true
  - type: textarea
    id: repro
    attributes:
      label: 复现步骤
      description: 最小复现代码 / 命令序列
    validations:
      required: true
  - type: input
    id: version
    attributes:
      label: 版本
      description: cspice-rs / cspice-rs-sys 版本或 commit
    validations:
      required: true
  - type: textarea
    id: env
    attributes:
      label: 环境
      description: 操作系统与架构、rustc 版本、CSPICE 版本（如 N0067）
    validations:
      required: true
  - type: textarea
    id: logs
    attributes:
      label: 相关日志
      description: 编译器输出 / SPICE 错误栈（erract 相关信息一并附上）
