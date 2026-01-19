# PalisadeOS – Boot Milestone (BIOS, Real Hardware Path)

## Status: BOOTS (BIOS, Raw Disk Path)

This repository has reached a critical milestone:  
**PalisadeOS now boots successfully through a real BIOS-compatible boot path**, without GRUB, Limine, or any external bootloader framework.

This is not a multiboot or `-kernel` shortcut.  
This is a real, sector-based, BIOS-driven boot flow.

---

## What Is Running

The following components are confirmed working together:

### 1. BIOS MBR Boot Sector
- Valid 512-byte boot sector
- Correct `0x55AA` signature
- Executed directly by BIOS
- Loaded at `0x7C00`
- Uses real-mode x86 instructions
- Performs INT 13h disk reads
- Handles failure paths cleanly

### 2. Custom Bootloader (BootL_POS)
- Raw disk image–based loader
- Reads subsequent sectors from disk
- Sets up execution environment explicitly
- No dependency on GRUB, Limine, or Multiboot
- Designed for legacy BIOS systems

### 3. Freestanding Kernel (NeoKern)
- Built as a freestanding x86_64 ELF
- No libc
- No dynamic linker
- No host OS assumptions
- Explicit entry path
- Clean halt behavior (`hlt`) when idle
- No triple faults or CPU resets observed

### 4. Real Disk Image Boot
- Kernel and bootloader placed into a real disk image
- QEMU launched using `-drive file=...,format=raw`
- BIOS boot sequence executed normally
- CPU remains stable after kernel entry

---

## What Was Explicitly Avoided

This milestone was achieved **without**:

- GRUB
- Limine
- PXE
- Multiboot handoff
- `qemu -kernel` shortcuts
- UEFI firmware
- Linux build environment (entirely built in Termux)

Every stage is explicit and under project control.

---

## Why This Matters

Reaching a stable BIOS boot means:

- The project is no longer theoretical
- The kernel is executable on real legacy hardware
- The firmware-to-kernel path is understood and owned
- All future work builds on a known-good base

This is one of the hardest steps in operating system development.  
Everything beyond this point is incremental.

---

## Verified Conditions

The current boot path works under these conditions:

- Legacy BIOS or CSM-enabled firmware
- BIOS-readable disk layout
- Valid CHS/LBA translation
- CPU starts in real mode (guaranteed by BIOS)

These conditions are satisfied by:
- QEMU (legacy BIOS)
- Real legacy BIOS motherboards
- Modern boards with CSM enabled

---

## Next Steps (Intentionally Deferred)

The following are **deliberately postponed** to preserve stability:

- UEFI support
- Limine integration
- Filesystem drivers
- Memory allocators
- SMP
- Interrupt controllers beyond basic validation

The immediate priority is **stability and observability**, not features.

---

## Summary

PalisadeOS has crossed the line from “trying to boot” to **“maintaining a booting system.”**

The system:
- Executes from BIOS
- Loads from disk
- Transfers control to a custom kernel
- Runs predictably and safely halts

This repository now represents a real operating system foundation.
