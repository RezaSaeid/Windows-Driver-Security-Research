# Kernel Debugging and Driver Verifier Safety

## Before enabling verification
- Use a disposable VM, never a daily-use workstation.
- Create and test a recovery snapshot.
- Confirm console access and recovery/safe-mode path.
- Record the exact driver and verifier options selected.
- Verify only the target test driver where possible.

## Preserve privately
- OS build and architecture
- Driver version and SHA-256
- Verifier configuration
- Reproduction steps and timestamps
- Bugcheck code and parameters
- WinDbg version and symbol status
- !analyze -v output, stack, and relevant module data

## Interpretation
A verifier bugcheck identifies a detected rule violation; it does not automatically prove attacker reachability or exploitable impact. A crash may arise from setup, unsupported configuration, or an unrelated driver.

## Cleanup
Follow the documented verifier reset procedure, reboot, confirm normal boot, and restore the known-good snapshot if needed. Never leave verification enabled unintentionally.
