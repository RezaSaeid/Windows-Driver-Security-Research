# Windows Driver Security — Technical Analysis Framework

## Review model
A kernel driver is part of the operating system trusted computing base. Review interface exposure, request lifecycle, memory ownership, synchronization, execution level, and teardown—not only IOCTL parsing. This is a methodology document, not a claim that a particular driver has been tested or found vulnerable.

## Identify architecture
Record WDM, KMDF, or mixed design; DriverEntry and initialization; device creation and interface registration; dispatch routines or framework queues; PnP/power responsibilities; device extension/context structures and lifetime; unload, cleanup, and removal paths; OS build, architecture, signing state, and source/binary identity.

For KMDF, identify framework object types, parent-child relationships, callbacks, queue dispatch mode, and cleanup callbacks. For WDM, identify major-function dispatch, IRP ownership, pending behavior, and completion responsibility.

## Trace request lifecycle
Model: user-mode open → I/O manager access validation and request construction → driver/framework dispatch → handler validation → buffer access and operation → synchronous/asynchronous completion → cancellation and cleanup. At every transition record ownership of request, buffer, object references, and completion responsibility. Ambiguous ownership is a high-value review question.

## Memory and buffer safety
Review allocation size versus read/write length; overflow and truncation; zero and boundary lengths; structure size/alignment/packing/version; user-pointer probing and locking; MDL mapping, direction, and lifetime; pool allocation and initialization; copy lengths; and IRQL legality. A successful probe is not a permanent guarantee that user memory remains valid.

## IRQL, synchronization, concurrency
Identify maximum IRQL and reentrancy/concurrency for each callback/helper. Review spin-lock scope, pageable operations while locks are held, wait behavior, shared-state synchronization, lock ordering, cancellation/completion races, timer/DPC/work-item teardown, reference counts during async work, and PnP removal/power transitions. A race hypothesis must name shared state, competing paths, missing invariant, and observable consequence.

## Object lifetime and teardown
Maintain a table of resource, creator, ownership transfer, release path, and asynchronous users for device/context, request/IRP, buffer/MDL, work item/timer, and referenced objects. Verify all success/failure paths. Teardown must prevent new work, drain/cancel outstanding work, and avoid freeing state still referenced by callbacks.

## Access control and privileged operations
For each externally reachable operation document interface ACL, requested access, IOCTL access bits, explicit authorization, caller-controlled inputs, privileged operation, and intended caller privilege. A privileged API call is not itself a vulnerability; establish that an untrusted caller can reach it with attacker-controlled parameters across an intended boundary.

## Static versus dynamic validation
Static review identifies paths and unsafe assumptions but may not establish runtime reachability, configuration, race timing, or impact. Dynamic validation is environment-dependent. Record driver/OS build, verifier settings, VM snapshot, debugger configuration, and reproduction reliability. A verifier report or bugcheck is evidence of a failure condition, not automatically a security vulnerability.

## Root-cause model
Distinguish trigger, fault site, data flow, lifetime/control-flow history, violated invariant, root cause, and security consequence. Use source, symbols, disassembly, call stacks, object state, and memory evidence where available. State uncertainty when source/private symbols are unavailable.

## Deliverables and safety
Deliver architecture/init overview, interface map, request lifecycle diagram, memory/ownership review, IRQL/concurrency review, lifetime/teardown table, evidence-backed findings, confidence, impact, and remediation. Use sample or intentionally vulnerable drivers in isolated VMs. Never publish unredacted kernel dumps, private symbols, or sensitive memory.