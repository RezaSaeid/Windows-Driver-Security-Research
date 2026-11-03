# Windows Driver Security — Architecture & Lifecycle Case Study

> Conceptual WDM/KMDF reference and synthetic scenario, not a review of a named vendor driver.

## Request lifecycle

Secure review follows ownership and state transitions, not only individual functions.

~~~mermaid
flowchart TD
 A[User request] --> B[I/O Manager]
 B --> C[WDM dispatch or framework]
 C --> D[Validate request and authority]
 D --> E[Acquire referenced resources]
 E --> F[Perform bounded operation]
 F --> G[Release resources]
 G --> H[Complete exactly once]
 C -->|Cancel or failure| I[Cancellation/error path]
 I --> G
~~~

This is a logical model. KMDF manages many framework objects and callbacks; WDM places more lifecycle responsibility directly on the driver.

## Ownership matrix

| Resource | Questions |
|---|---|
| IRP / WDFREQUEST | Who owns completion? Can multiple paths complete it? |
| MDL / mapped buffer | Who allocates, locks, maps, unmaps and frees it? |
| Device/context object | Which references keep it alive during async work? |
| Work item / DPC | Can teardown race with queued execution? |
| Timer / callback | Is it stopped and synchronized before context destruction? |
| User pointer | Is it captured safely and used in a valid context? |

Trace allocation, reference, transfer, completion and release. Do not infer ownership from variable names.

## IRQL and execution context

For each callback/helper, document permitted IRQL, whether it may block, pageable-code constraints, originating process context, synchronization primitive, lock ordering, and possible re-entrancy. An operation valid at PASSIVE_LEVEL may be invalid at DISPATCH_LEVEL; verify the exact API contract.

## Synthetic teardown race

A request queues asynchronous work while device removal begins. Review the ordering of request acceptance, state reference, work queueing, cancellation, stop/removal, callback execution, and context release.

Required invariants:
- no callback dereferences freed context;
- cancellation and completion are coordinated;
- a request completes at most once;
- teardown prevents new work and drains existing work;
- locks are not held across unsafe waits or re-entrant calls.

## Security review dimensions

**Memory safety:** lengths, offsets, alignment, integer arithmetic, structure versions, allocation size versus later access.

**Access control:** device ACL, symbolic-link exposure, IOCTL access bits, operation-specific authorization.

**Lifetime/concurrency:** references across async boundaries, cancellation, unload, surprise removal, power transitions, callback draining.

**Information disclosure:** initialized output and correct status/information-length relationship.

## Report structure

Driver identity and hash/version; framework and classification evidence; device creation and dispatch/callback registration; request path; resource ownership map; concurrency map; findings with reachability/impact/confidence; limitations such as missing symbols or static-only review.

## Quality gates

- [ ] Framework and callback contracts identified.
- [ ] IRQL and process-context assumptions checked.
- [ ] IRP/WDFREQUEST completion ownership traced.
- [ ] MDL and buffer ownership documented.
- [ ] Cancellation and teardown reviewed.
- [ ] Device ACL and IOCTL authorization reviewed.
- [ ] Claims supported by a violated invariant and evidence.

## References

- [Introduction to Driver Objects](https://learn.microsoft.com/windows-hardware/drivers/kernel/introduction-to-driver-objects)
- [I/O Request Packets](https://learn.microsoft.com/windows-hardware/drivers/kernel/irp)
- [Windows Driver Frameworks](https://learn.microsoft.com/windows-hardware/drivers/wdf/)
- [Managing Hardware Priorities](https://learn.microsoft.com/windows-hardware/drivers/kernel/managing-hardware-priorities)
