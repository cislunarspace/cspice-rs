name: 功能建议
description: 建议新增 CSPICE 函数包装或改进现有接口
labels: ["enhancement"]
body:
  - type: textarea
    id: problem
    attributes:
      label: 想解决的问题
      description: 该功能应对的场景
    validations:
      required: true
  - type: textarea
    id: solution
    attributes:
      label: 期望的方案
      description: 期望的 API 形态；若对应 CSPICE 函数请附 NAIF 文档链接
    validations:
      required: true
  - type: textarea
    id: alternatives
    attributes:
      label: 备选方案
      description: 已考虑过的其他做法
