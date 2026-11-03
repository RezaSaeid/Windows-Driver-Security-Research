# Windows Driver Security Research

A structured portfolio for Windows kernel-driver architecture, secure code review, debugging, and vulnerability research foundations.

> **Status:** Research and learning workspace. Planned exercises are not represented as completed findings.

## Focus
WDM/KMDF, device objects, symbolic links, IRPs, dispatch routines, IOCTL handling, access control, buffer validation, pool allocation, object lifetime, concurrency, Driver Verifier, and crash analysis.

## Start here
- [Driver research roadmap](docs/driver-research-roadmap.md)
- [Driver review checklist](docs/review-checklist.md)
- [Interface review exercise](labs/driver-interface-review.md)
- [Kernel debugging safety](docs/debugging-safety.md)
- [Driver review template](templates/driver-review-template.md)

## Lab requirements
Use a dedicated Windows VM with snapshots and recovery access. Begin with Microsoft sample drivers or intentionally vulnerable training drivers. Record exact source revision, binary hash, OS build, architecture, and tool versions.

## Research workflow
Map the interface, trace requests to handlers, review validation/access/memory/lifetime/concurrency, validate hypotheses under controlled conditions, and document root cause, impact, evidence, and mitigations.

## Safety
Driver Verifier and kernel debugging can crash the system. Never run risky driver experiments on a daily-use host. Test only within an authorized scope.
