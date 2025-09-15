## Explain the concept of process creation in operating systems
```c
A process is simply a program in execution (a running instance of a program)
It includes:
Program code (text section)
Current activity (Program Counter, CPU registers)
Process Stack (function calls, parameters, local variables)
Data section (global variables, heap, etc.)
Process Control Block (PCB) → contains metadata like process ID, state, priority, resources, etc.
```
## Differentiate between the fork() and exec() system calls
```c
 
