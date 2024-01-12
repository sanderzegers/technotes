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



#### Modify data

| set $eax=0                      |                 |
| ------------------------------- | --------------- |
| set ($eflags)\|=0x42            | set zero flag   |
| set $eflags &= \~(1 << 6)       | unset zero flag |
| set \*(char\*)0x080480d9 = 0x90 | Modify Code     |



### GDB Scripting

{% embed url="https://gist.github.com/sanderzegers/e8076c3a5e954c13a480899349817af5" %}
