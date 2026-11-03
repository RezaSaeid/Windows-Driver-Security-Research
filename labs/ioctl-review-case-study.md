# IOCTL Review Case Study: Validate Before Use

## Learning objective

Practice reviewing a dispatch routine for user-controlled lengths and offsets. This is a paper/code-review exercise; it is not a loadable driver and does not interact with a real device.

## Hypothetical interface

A device accepts a request containing:

- a 32-bit offset,
- a 32-bit length,
- a caller-provided input buffer.

The handler must ensure the requested range is fully contained in the input buffer before reading it.

## Review questions

1. Is the request structure large enough before any field is read?
2. Are integer conversions performed before or after bounds validation?
3. Can `offset + length` overflow?
4. Does the IOCTL transfer method match the way the handler accesses buffers?
5. Are input and output lengths validated independently?
6. Are all failure paths guaranteed to complete the IRP exactly once?
7. Is the device ACL restrictive enough for the intended caller set?

## Safer range predicate

Use subtraction after checking the offset:

```c
static bool range_is_valid(size_t offset, size_t length, size_t total)
{
    return offset <= total && length <= total - offset;
}
```

This avoids relying on an addition that could wrap. The predicate alone does not validate the request structure, pointer provenance, access policy, or lifetime of the buffer.

## Evidence to record

- Driver model and framework version
- IOCTL value and decoded method/access bits
- Dispatch callback and relevant call path
- Exact input/output buffer length checks
- Integer types and conversion points
- Error handling and IRP completion path
- Reproduction status (not attempted / static-only / safely reproduced)

## Completion criteria

A review is complete only when each question has a source location or is explicitly marked unknown. Do not label a suspected issue as a vulnerability without a reproducible security impact.
