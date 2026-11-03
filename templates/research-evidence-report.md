# Windows Driver Security Review Report

> Distinguish static review observations from runtime evidence. Do not report unperformed validation as successful.

## Target identity
- Driver path, filename and SHA-256:
- Version / signer / architecture:
- OS build:
- Framework: WDM / KMDF / UMDF / unknown
- Symbols available:

## Scope and safety
- Authorization and target boundary:
- Static / dynamic / debugger methods:
- VM snapshot and recovery:
- Explicit exclusions:

## Architecture and entry points
- Initialization:
- Device objects and symbolic links:
- Dispatch routines / framework callbacks:
- IOCTL interfaces:
- PnP, power and unload paths:

## Ownership and lifecycle
| Resource | Acquisition | Owner | Async users | Release | Evidence |
|---|---|---|---|---|---|
| | | | | | |

## Execution context
| Routine | IRQL contract | Process context | Pageable? | Blocking calls | Evidence |
|---|---|---|---|---|---|
| | | | | | |

## Concurrency and teardown
- Locks and lock order:
- Cancellation path:
- Work items / DPCs / timers:
- Stop/remove ordering:
- In-flight operation draining:
- Race hypotheses and evidence:

## Findings
For each finding record ID/title, affected routine, attacker-controlled input/state, preconditions, reachability, violated invariant, root cause, demonstrated impact, confidence and evidence IDs.

## Limitations and conclusion
- Untested paths / missing symbols:
- Static-only assumptions:
- Reproduction status:
- Remediation:
- Reviewer sign-off:
