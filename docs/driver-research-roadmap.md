# Windows Driver Security Research Roadmap

## Prerequisites
Build foundations in C, pointers, memory layout, x64 calling conventions, Windows processes/threads, handles, and debugging.

## Stages and outputs
1. Driver architecture: WDM/KMDF, entry points, device objects, IRPs. Output: architecture notes.
2. I/O path: dispatch, completion, cancellation, buffering. Output: request-flow diagram.
3. Packaging: INF, service configuration, signing concepts. Output: package review.
4. Memory safety: pool allocation, bounds, integer arithmetic, user-buffer capture, object lifetime. Output: code-review notes.
5. Synchronization: IRQL, locks, work items, cancellation, races. Output: concurrency worksheet.
6. Validation: static analysis and Driver Verifier in an isolated VM. Output: configuration and results.
7. Debugging: kernel debugging, symbols, bugcheck triage, stack and pool inspection. Output: sanitized dump analysis.
8. Security review: attack surface, access control, trust boundaries. Output: threat model and prioritized findings.

Kernel testing can crash or corrupt a system. Use a disposable VM, snapshots, test-only drivers, and recovery access. Never enable Driver Verifier on a daily-use host. Reading alone does not complete a stage.
