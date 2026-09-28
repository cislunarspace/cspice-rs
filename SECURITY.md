# Security Policy

## Reporting a Vulnerability

Please do not report security vulnerabilities through public issues. Use GitHub private security advisories (Security → Advisories → Report a vulnerability), or contact the repository maintainers.

Please do not disclose details publicly while a fix is in progress; we will credit reporters once the fix is released (unless anonymity is requested).

## Scope of Support

- The Rust binding code in this repository (`cspice/`, `cspice-sys/`, build scripts, CI).
- Defects in the CSPICE toolkit itself (the C library published by NAIF) should be reported to [NAIF](https://naif.jpl.nasa.gov/naif/); interim workarounds at the binding layer can be evaluated in this repository.

## Response Timeline

- P0 (memory safety, exploitable UB): response within 3 days.
- Others: response within 7 days.
