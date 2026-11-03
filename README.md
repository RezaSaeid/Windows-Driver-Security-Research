# Windows Driver Security Research

A technical research workspace for Windows kernel-driver architecture, secure code review, debugging concepts, and vulnerability-research foundations.

> **Portfolio status:** Research and learning workspace. Exercises and review hypotheses are not represented as completed findings. A code smell, unsafe-looking pattern, or crash is not by itself proof of a vulnerability.

## What this repository is intended to demonstrate

- Ability to map a driver interface from device creation through request dispatch and completion.
- Understanding of WDM/KMDF concepts and user/kernel interaction.
- Structured review of access control, buffer validation, memory safety, object lifetime, and concurrency.
- Root-cause-oriented analysis that distinguishes a triggering symptom from the underlying defect.

## Technical focus

WDM and KMDF, device objects, symbolic links, IRPs, dispatch routines and queue callbacks, IOCTL handling, device ACLs, buffer validation, pool allocation, object lifetime, IRQL constraints, cancellation, synchronization, Driver Verifier, and crash analysis.

## Start here

- [Driver research roadmap](docs/driver-research-roadmap.md)
- [Driver review checklist](docs/review-checklist.md)
- [Driver interface review exercise](labs/driver-interface-review.md)
- [Kernel debugging safety](docs/debugging-safety.md)
- [Driver review template](templates/driver-review-template.md)

## Review methodology

For each interface, document:

1. How the device interface is created and exposed.
2. Which access rights and caller assumptions apply.
3. How requests reach their handlers.
4. How input and output buffers are described, validated, and accessed.
5. Whether integer conversions, allocation sizes, and structure versions are safe.
6. How references, locks, cancellation, completion, and cleanup are handled.
7. Which observations support a defect hypothesis and which do not.

## Expected research deliverables

A strong review should include a sanitized interface map or call-flow diagram, a completed review table, relevant code or disassembly references, an explanation of the violated invariant (if any), impact analysis, confidence level, and remediation guidance.

## Lab environment

Use a dedicated Windows VM with snapshots and recovery access. Begin with Microsoft sample drivers or intentionally vulnerable training drivers. Record exact source revision, binary hash, OS build, architecture, and tool versions.

## Current maturity and safety

The repository provides review guidance and exercises; do not interpret their presence as evidence that a driver has been dynamically tested. Driver Verifier and kernel debugging can crash the system. Never run risky driver experiments on a daily-use host or against a third-party driver without authorization.
