# x86 assembly



| Command             | Description                                                                                                                                                                         | Links |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- |
| LAHF                | <p>Load Status Flags into AH register.<br>Bit 7 = Sign (SF)<br>Bit 6 = Zero (ZF)<br>Bit 4 = Auxiliary (AF)<br>Bit 2 = Parity (PF)<br>Bit 0 =Carry (CF)<br>Bit 1,3,5 = unchanged</p> |       |
| TEST AX,AX          | Add two variables, store calculation result in flag register                                                                                                                        |       |
| CMP AX,1            | Subtract two variables, store calculation result in flag register                                                                                                                   |       |
| LEA eax, \[rdi - 1] | Load effective address to register. It is used when the address is not known at compile time (for example on stack)                                                                 |       |
|                     |                                                                                                                                                                                     |       |
