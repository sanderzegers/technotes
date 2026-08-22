# GDB

#### Execution

| Command    | Description                                   |
| ---------- | --------------------------------------------- |
| `start`    | Start program and stop at `main`              |
| `starti`   | Start program and stop at first instruction   |
| `continue` | Continue execution until next breakpoint      |
| `until`    | Continue until current loop/function advances |
| `return`   | Force selected function to return immediately |

#### Breakpoints

<table data-header-hidden data-search="false"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><code>break *0x8049bd7</code></td><td>break on execution</td></tr><tr><td><code>awatch *(int *)0x08049340</code></td><td>break on read/write access (value must change)</td></tr><tr><td><code>watch *(char *)0x80123454</code></td><td>break on write access (value must change)</td></tr><tr><td><code>rwatch *(long *)0xfeedface</code></td><td>break on read access</td></tr><tr><td><code>catch syscall write</code></td><td>Catch a linux syscall</td></tr><tr><td><code>info breakpoints</code></td><td>list breakpoints</td></tr><tr><td><code>del 2</code></td><td>delete breakpoint 2</td></tr><tr><td><code>del breakpoints</code></td><td>delete all breakpoints</td></tr><tr><td><code>disable 1</code></td><td>disable breakpoint 1</td></tr><tr><td><code>tbreak *ADDR</code></td><td>Set temporary breakpoint at address</td></tr><tr><td><code>condition 3 $rax == 0</code></td><td>Break at breakpoint 3 only when condition is true</td></tr><tr><td><code>ignore 3 100</code></td><td>Ignore breakpoint 3 for next 100 hits</td></tr><tr><td><code>enable 3</code></td><td>Enable breakpoint 3</td></tr><tr><td><code>clear function</code></td><td>Delete breakpoints set at function</td></tr></tbody></table>

#### Hook stop

Run command after every breakpoint:

```
define hook-stop
  info registers
  x/4i $pc
end
```

Run specific command for a specific breakpoint is hit:

```
commands 2
  info registers
  x/4i $pc
end
```

#### Info

| `info proc mappings`  | show mapped memory addresses (stack, heap, libc location, etc) |
| --------------------- | -------------------------------------------------------------- |
| `info functions test` | show all functions with regex: test                            |
| `info registers`      | show all registers at current state                            |

#### Convenience variables

| `set $dat_84 = 0x303042` | create convenienc variable dat\_84. Has no impact on program.  |
| ------------------------ | -------------------------------------------------------------- |
| `x/bx $dat_84+4`         | examine one byte, formatted as hex at 0x303046                 |

#### Missing Entry Point

Launch program in GDB. Set breakpoint on \_\_libc\_start\_main. Relaunch the program in GDB and retrieve main from RDI.

#### Retrieve Basis Address for Ghidra

<pre><code>info proc mappings

Start addr at offset 0x0 that belongs to the executable.
In this example 
0x555555554000

(gdb) info proc mappings
process 3224
Mapped address spaces:

          Start Addr           End Addr       Size     Offset  Perms  objfile
      <a data-footnote-ref href="#user-content-fn-1">0x555555554000</a>     0x55555566d000   0x119000        <a data-footnote-ref href="#user-content-fn-1">0x0</a>  r--p   /tmp/example.bin

</code></pre>

#### Control flow

<table data-header-hidden data-search="false"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><code>si</code></td><td>Step Into one instruction</td></tr><tr><td><code>ni</code></td><td>Next instruction (one step)</td></tr><tr><td><code>next</code></td><td>Next source code line</td></tr><tr><td><code>finish</code></td><td>Execute until selected stack frame returns</td></tr><tr><td><code>thread</code></td><td>switch threads</td></tr><tr><td><code>jump *decrypt</code></td><td>Jumps directly to address. (modifies $IP)</td></tr><tr><td><code>jump ch12.c:32</code></td><td>Jump to line of code (when binary compiled with -g)</td></tr></tbody></table>

#### Stack Frame

<table data-header-hidden data-search="false"><thead><tr><th>Text</th><th>Description</th></tr></thead><tbody><tr><td><code>bt</code></td><td>Show function call stack</td></tr><tr><td><code>frame 2</code></td><td>Select stack frame 2</td></tr><tr><td><code>up</code></td><td>Move to caller stack frame</td></tr><tr><td><code>down</code></td><td>Move to called stack frame</td></tr><tr><td><code>info frame</code></td><td>Show information about selected stack frame</td></tr><tr><td><code>info args</code></td><td>Show function arguments</td></tr><tr><td><code>info locals</code></td><td>Show local variables</td></tr></tbody></table>

#### Fork / exec debugging

| `catch fork`                 | Stop when program calls `fork()`             |
| ---------------------------- | -------------------------------------------- |
| `catch vfork`                | Stop when program calls `vfork()`            |
| `catch exec`                 | Stop when program executes a new program     |
| `set follow-fork-mode child` | Follow child process after fork              |
| `set detach-on-fork off`     | Keep both parent and child under GDB control |

#### Modify data

| `set $eax=0`                    |                 |
| ------------------------------- | --------------- |
| `set ($eflags)\|=0x40`          | set zero flag   |
| `set $eflags &= ~(1 << 6)`      | unset zero flag |
| `set *(char*)0x080480d9 = 0x90` | Modify Code     |

#### Find Data

| find 0x8048000,0x804b000,"accept()" | find string between starting and end address |
| ----------------------------------- | -------------------------------------------- |
| list main.c:34                      | list source code at line 34                  |

#### GDB run

| `run <<< $(python -c "print('B'*300)")`                                                        | standard input                                                                |
| ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `run $(python3 -c 'print("A" + "B"*227)')`                                                     | command line argument                                                         |
| <p><code>#printf '5\n2\n\x41' > input.txt</code><br><code>gdb: run &#x3C; input.txt</code></p> | Run multiple commands. Eg. replacement for: printf('5\n2\n\x41') \| ./app.bin |

#### Signals

| Text                        | Description                                  |
| --------------------------- | -------------------------------------------- |
| `info signals`              | Show how GDB handles signals                 |
| `handle SIGSEGV stop print` | Stop and display message when SIGSEGV occurs |

## GDB Scripting

{% embed url="https://gist.github.com/sanderzegers/e8076c3a5e954c13a480899349817af5" %}

## PWNDBG



|                                |                                                       |
| ------------------------------ | ----------------------------------------------------- |
| pwndbg                         | List all pwndbg commands                              |
| starti                         | Set breakpoint at first instruction                   |
| entry                          | entry starts the program at the ELF entrypoint.       |
| set context-sections           | Set context to display                                |
| ctx-watch BUF                  | Add Watch expression to context view                  |
| ctx-watch execute "x/20x $rsp" | Add watch expression (gdb command) to context view    |
| ctx-unwatch 2                  | Remove watch expression 2                             |
| nextcall                       | Jump to next call                                     |
| asm ADD EBP,EBP                | Assemble shellcode into bytes                         |
| piebase                        | Retrieve relocated binary base address                |
| distance 0x001043a0 0x10449f   | calculate distance between two addresses              |
| xuntil 0x0123123               | Continue untill address                               |
| context                        | Display context window                                |
| env                            | Show all environment variables and addresses          |
| set environment variable value | Set a environment variable from within GDB            |
| search "Enter"                 | Search for string in memory                           |
| search 0xffff80cd -t dword     | Search for dword in memory                            |
| checksec                       | Show security features (stack canaries, nx, pie, etc) |



[^1]: 
