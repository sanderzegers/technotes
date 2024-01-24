# GDB

#### Breakpoints

| break \*0x8049bd7   | break on execution         |
| ------------------- | -------------------------- |
| awatch \*0xfeedface | break on read/write access |
| watch \*0x08049340  | break on write access      |
| rwatch \*0x80123454 | break on read access       |
| info breakpoints    | list breakpoints           |
| del 2               | delete breakpoint 2        |
| del breakpoints     | delete all breakpoints     |

#### Info

| info proc mappings | show mapped memory addresses (stack, heap, libc location, etc) |
| ------------------ | -------------------------------------------------------------- |



Convenience variables

|                         |                                                                        |
| ----------------------- | ---------------------------------------------------------------------- |
| set $dat\_84 = 0x303042 | set local variable dat\_84. Has no impact on program. Just convenience |
| x/bx $dat\_84+4         | Print out value at @0x303046                                           |
|                         |                                                                        |

#### Missing Entry Point

Launch program in GDB. Set breakpoint on \_\_libc\_start\_main. Relaunch the program in GDB and retrieve main from RDI.



#### Control flow

|        |                                            |
| ------ | ------------------------------------------ |
| si     | Step Into one instruction                  |
| ni     | Next instruction (one step)                |
| finish | Execute until selected stack frame returns |
| thread | switch threads                             |

#### Modify data

| set $eax=0                      |                 |
| ------------------------------- | --------------- |
| set ($eflags)\|=0x42            | set zero flag   |
| set $eflags &= \~(1 << 6)       | unset zero flag |
| set \*(char\*)0x080480d9 = 0x90 | Modify Code     |

GDB run



|                                        |                |
| -------------------------------------- | -------------- |
| run <<< (python -c "print('B'\*300)")  | standard input |
| run $(python -c "print('A','B'\*227)") |                |
|                                        |                |

## GDB Scripting

{% embed url="https://gist.github.com/sanderzegers/e8076c3a5e954c13a480899349817af5" %}

## PWNDBG



|                                |                                                    |   |
| ------------------------------ | -------------------------------------------------- | - |
| pwndbg                         | List all pwndbg commands                           |   |
| set context-sessions           | Set context to display                             |   |
| ctx-watch BUF                  | Add Watch expression to context view               |   |
| ctx-watch execute "x/20x $rsp" | Add watch expression (gdb command) to context view |   |
| nextcall                       | Jump to next call                                  |   |
| asm ADD EBP,EBP                | Assemble shellcode into bytes                      |   |
| piebase                        | Retrieve relocated binary base address             |   |
| nextcall                       | Jump to next call                                  |   |
| distance 0x001043a0 0x10449f   | calculate distance between two addresses           |   |
