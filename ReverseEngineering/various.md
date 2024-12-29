# Various

## neChecksec

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



Linux System Call Table: [https://faculty.nps.edu/cseagle/assembly/sys\_call.html](https://faculty.nps.edu/cseagle/assembly/sys_call.html)



Shellcode

{% embed url="https://shell-storm.org/shellcode/index.html" %}

Store in environment variable:

```
export shellcode=`python -c 'print("\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f \x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80")'`

```
