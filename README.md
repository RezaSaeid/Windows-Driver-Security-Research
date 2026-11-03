# Windows Driver Security Research

A structured portfolio for Windows kernel-driver security, driver architecture, reverse engineering, and defensive code review.

> **Status:** Research and learning workspace. Completed experiments and findings are distinguished from planned exercises.

## Focus areas
- Windows driver architecture and lifecycle
- WDM and KMDF fundamentals
- Device objects, symbolic links, IRPs, and dispatch routines
- IOCTL handling and buffer validation
- Driver installation, signing, and service configuration
- Pool allocation, object lifetime, and concurrency
- Driver Verifier and crash-dump analysis
- Secure design and attack-surface reduction

## Structure
```text
fundamentals/
code-review/
labs/
crash-analysis/
secure-design/
reports/
```

## Suggested lab setup
Use a dedicated Windows VM with snapshots and a separate test VM for kernel debugging where possible. Begin with Microsoft sample drivers and intentionally vulnerable training drivers. Enable Driver Verifier only in disposable test environments and keep recovery access available.

## Research workflow
1. Identify the driver and supported OS/build.
2. Map device interfaces, dispatch paths, and privileged operations.
3. Review input validation, access checks, memory handling, synchronization, and cleanup.
4. Validate hypotheses with controlled tests and debugging.
5. Document root cause, impact, evidence, and mitigations.

## Responsible research
Analyze and test only software within an authorized scope. Do not publish sensitive third-party code, secrets, private symbols, or unnecessary exploit details.
