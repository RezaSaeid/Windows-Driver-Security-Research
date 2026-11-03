# Windows Driver Review Checklist

## Interface and access
- [ ] Device object and symbolic link identified
- [ ] Device ACL and access rights reviewed
- [ ] IOCTL access bits and transfer method documented
- [ ] Untrusted caller assumptions explicit

## Input and memory
- [ ] Input/output lengths validated before access
- [ ] Integer overflow and truncation considered
- [ ] Structure size, alignment, and version validated
- [ ] User buffers handled according to API contract
- [ ] Caller pointers are not retained unsafely
- [ ] Allocation size matches actual use
- [ ] Object lifetime and reference counting reviewed

## Execution and concurrency
- [ ] IRQL requirements respected
- [ ] Shared state synchronization reviewed
- [ ] Cancellation and completion paths reviewed
- [ ] Error paths release resources
- [ ] Request ordering assumptions are enforced

## Validation
- [ ] Isolated authorized test environment used
- [ ] Driver Verifier settings and selected drivers recorded
- [ ] Dumps sanitized before publication
- [ ] Root cause separated from symptom
- [ ] Impact and exploitability assessed independently
- [ ] Reproduction and cleanup documented

A checked box means evidence was reviewed, not merely assumed.
