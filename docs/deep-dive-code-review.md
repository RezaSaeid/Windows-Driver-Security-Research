# Driver Security Deep Dive — Ownership, IRQL and Teardown

> Conceptual WDM/KMDF review material. No runnable kernel implementation or real product finding is claimed.

## 1. Ownership map

For every resource, identify allocation, owner, reference/transfer, asynchronous use, completion and release. A non-NULL pointer does not prove its target remains alive.

| Resource | Questions |
|---|---|
| IRP / WDFREQUEST | Who owns completion? Can cancel and worker paths race? |
| Device extension / context | What keeps it alive while callbacks execute? |
| MDL | Who allocates, locks, maps, unmaps and frees it? |
| Work item / DPC | Can queued work outlive referenced state? |
| Timer / callback | Is it stopped and drained before context release? |
| Lock | What is lock order? Can a wait occur while held? |

## 2. Asynchronous request state

~~~mermaid
stateDiagram-v2
 [*] --> Received
 Received --> Validated
 Validated --> Queued
 Queued --> Running
 Queued --> Cancelled
 Running --> Completing
 Cancelled --> Completing
 Completing --> Completed
 Completed --> [*]
~~~

This is an abstract state model. The implementation must synchronize cancellation and worker completion so they cannot both complete a request or release shared state prematurely.

## 3. Synthetic teardown race

A request queues work referencing device context C. Device stop begins before the worker runs. Establish:
1. what reference/framework guarantee keeps C alive;
2. how stop prevents new requests;
3. how queued work is cancelled or drained;
4. how cancellation races with worker completion;
5. who owns final request completion;
6. how timers, DPCs and callbacks are quiesced.

A delay or best-effort cancellation is not a lifetime argument.

## 4. IRQL and execution context

| Context | High-level constraint to verify |
|---|---|
| PASSIVE_LEVEL | Blocking may be possible where API contract permits; pageable code may be allowed |
| APC_LEVEL | Restrictions differ from PASSIVE; verify every called API |
| DISPATCH_LEVEL | Must not perform operations that block; pageable code/data generally cannot be accessed |

These are reminders, not a replacement for the exact API contract. Record each callback's documented maximum IRQL and the IRQL requirements of helpers it calls.

## 5. WDM versus KMDF

**WDM:** driver code directly handles IRPs and implements dispatch, cancellation, completion and many lifetime details.

**KMDF:** framework abstractions provide request/object models, queues, parent-child lifetime relationships and callback contracts. They do not automatically solve application-specific validation, authorization, concurrency or buffer correctness.

Do not transfer WDM ownership assumptions to KMDF without documenting the framework contract.

## 6. Teardown model

~~~mermaid
flowchart TD
 A[Mark device stopping] --> B[Reject new work]
 B --> C[Stop timers and callback producers]
 C --> D[Cancel or drain queued work]
 D --> E[Safely wait for in-flight operations]
 E --> F[Release shared resources]
 F --> G[Delete device/context objects]
~~~

Exact ordering depends on WDM/KMDF callbacks and PnP/power state. Never wait while holding a lock required by the work being drained.

## Review annotation format

For each sensitive routine document entry contract (IRQL, process context, buffer trust), preconditions, ownership, synchronization, exit contract and partial-failure cleanup.

## Quality gates

- [ ] Resource ownership and lifetime explicit.
- [ ] Request completion has one coordinated owner.
- [ ] IRQL and pageable-code assumptions verified.
- [ ] Cancellation and worker paths synchronized.
- [ ] Stop/removal prevents new work before draining.
- [ ] MDL and mapped-buffer lifetime correct.
- [ ] Partial initialization and rollback covered.
- [ ] WDM/KMDF semantics not conflated.

## References

- [IRP](https://learn.microsoft.com/windows-hardware/drivers/kernel/irp)
- [Managing Hardware Priorities](https://learn.microsoft.com/windows-hardware/drivers/kernel/managing-hardware-priorities)
- [KMDF Object Life Cycle](https://learn.microsoft.com/windows-hardware/drivers/wdf/framework-object-life-cycle)
- [Cancel-Safe IRP Queues](https://learn.microsoft.com/windows-hardware/drivers/kernel/cancel-safe-irp-queues)
