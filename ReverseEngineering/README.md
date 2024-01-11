# GDB

#### Breakpoints

| rwatch \*0xfeedface | break on read/write access |
| ------------------- | -------------------------- |
| watch \*0x08049340  | break on write access      |

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
