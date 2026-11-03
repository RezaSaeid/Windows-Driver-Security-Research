# Driver Interface Review Exercise

## Purpose
Practice tracing a user-mode request into a driver without loading an unknown driver or sending malformed requests to a production system.

## Inputs
Choose a Microsoft sample driver or a deliberately vulnerable training driver in an isolated VM. Record exact release/source commit, binary hash, Windows build, and architecture.

## Review workflow
1. Locate device creation and symbolic-link/interface registration.
2. Identify dispatch registration (WDM major functions or KMDF queue callbacks).
3. Enumerate control codes and map each to its handler.
4. Record access rights, transfer method, input/output lengths, and caller assumptions.
5. Trace each input field to its first use and any privileged operation.
6. Inspect error paths, allocation/free pairs, cancellation, synchronization, and completion.
7. Form hypotheses only where code and reachability support them.
8. Validate only in a disposable VM with recovery access.

## Review table
| Field | Notes |
|---|---|
| Device/interface | |
| Device ACL/access | |
| IOCTL/function | |
| Transfer method | |
| Handler | |
| Input/output size checks | |
| User-buffer handling | |
| Privileged operation | |
| Error/cleanup path | |
| Evidence and confidence | |

## Deliverable
A sanitized call-flow diagram, completed table, and concise list of questions or confirmed defects. A code smell is not a vulnerability without demonstrated security impact.
