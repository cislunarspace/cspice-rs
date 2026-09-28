name: Feature request
description: Suggest a new CSPICE function wrapper or an improvement to an existing interface
labels: ["enhancement"]
body:
  - type: textarea
    id: problem
    attributes:
      label: Problem to solve
      description: The scenario this feature should address
    validations:
      required: true
  - type: textarea
    id: solution
    attributes:
      label: Proposed solution
      description: The desired API shape; if it maps to a CSPICE function, link the NAIF documentation
    validations:
      required: true
  - type: textarea
    id: alternatives
    attributes:
      label: Alternatives considered
      description: Other approaches you have already considered
