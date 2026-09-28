name: Bug report
description: Report a compile failure, binding error, or unexpected behavior in cspice-rs
labels: ["bug"]
body:
  - type: textarea
    id: what-happened
    attributes:
      label: Bug description
      description: What happened, and what you expected
    validations:
      required: true
  - type: textarea
    id: repro
    attributes:
      label: Steps to reproduce
      description: Minimal reproducing code / command sequence
    validations:
      required: true
  - type: input
    id: version
    attributes:
      label: Version
      description: cspice-rs / cspice-rs-sys version or commit
    validations:
      required: true
  - type: textarea
    id: env
    attributes:
      label: Environment
      description: OS and architecture, rustc version, CSPICE version (e.g. N0067)
    validations:
      required: true
  - type: textarea
    id: logs
    attributes:
      label: Relevant logs
      description: Compiler output / SPICE error stack (include erract-related information as well)
