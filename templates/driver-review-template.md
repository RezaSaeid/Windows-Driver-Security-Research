# Windows Driver Security Review

## Target
- Driver name/version:
- SHA-256:
- Signer:
- Windows build:
- Review date:
- Authorization / scope:

## Architecture
Describe driver model (WDM/KMDF), entry points, device objects, interfaces, and major dispatch paths.

## Attack surface
List user-accessible interfaces, IOCTLs, callbacks, services, and privileged operations.

## Review areas
- Access control and privilege checks
- Input validation and buffer handling
- Memory allocation, bounds, and lifetime
- IRP completion and cancellation
- Concurrency, synchronization, and reentrancy
- Error handling and cleanup
- Secure configuration and deployment

## Findings
For each finding include evidence, affected code path, preconditions, impact, confidence, and remediation.

## Limitations
State unavailable symbols, untested paths, assumptions, and environmental constraints.

## Summary
Only report conclusions supported by reproducible evidence.
