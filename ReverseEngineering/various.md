# Various

## Checksec

Check executables and kernel properties

```
checksec --file=ch37.bin
RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      Symbols         FORTIFY Fortified       Fortifiable     FILE
Partial RELRO   No canary found   NX enabled    PIE enabled     No RPATH   No RUNPATH   No Symbols        No    0               4               ch37.bin

```

### RELRO:

Relocation Read-Only

Partial RELRO: \
\- Default setting in GCC\
\- Only forces the GOT to come before the BSS in memory, eliminating the risk of a buffer overflow on a global variable overwriting GOT entries



Full RELRO:\
\- Makes GOT completely read-only.\
\- Longer load time of binary



### PIE:

Position Independent Executable\
\- PIE binary and all of its dependencies are loaded into randomized locations within virtual memory.\
\- Protect against ROP attacks\
\- GDB disabled address randomization by default. Will relocate to 0x555555550000.



