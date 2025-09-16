## Explain the concept of process creation in operating systems
```c
A process is simply a program in execution (a running instance of a program)
It includes Program code (text section),Current activity (Program Counter, CPU registers),Process Stack,Data section,Process Control Block (PCB) → contains metadata like process ID, state, priority, resources, etc.
```
## Differentiate between the fork() and exec() system calls
```c
fork() :  Creates a new process(child) by duplicating the parent process.
After fork(),both parent and child continue executing the next instruction.
Child is a copy of the parent (same code,data stack, but different memory space and PID)

exec() : Replaces the current process image with a new program.
The calling process is completely replaced by thr new program ;
PID remains the same,It is used after fork() by the child to run a different program
```
## Write a c program to demonstrate the use of fork() system call
```c
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
int main()
{
pid_t pid;
pid = fork();
if(pid <0)
{
perror("fork failed");
return 1;
}
else if(pid ==0)
{
printf("This is child process");
printf("child pid: %d\n",getpid());
printf("parent pid: %d\n",get ppid());
}
else
{
printf("This is parent process\n");
printf("Parent pid: %d\n",get pid());
printf("child pid: %d\n",pid);
}
return 0;
}
```

## What is the purpose of the wait() system call in process management
```c
1.  It allows parent to wait for child process completion.
i) When a parent creates a child using fork(),both run concurrently.
ii) If the parent finishes first, the child becomes an Orphan
iii) Wait() makes the parent pause execution until its child finishes.

2. Collect Exit status of child(present zombie processes)
i) When a child terminates ,its entry remains in the process table(in a "zombie" state) until the parent collects its exit status.
ii) Wait() ensures the parent collects this status and the child is fully removed from the process table.

3. Synchronization: Useful in coordinating tasks between parent and child process(parent waits until child finishes before continuing)

#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>
int main()
{
pid_t pid = fork();
if(pid = 0)
{
printf("child process running....\n");
sleep(2);
printf("child process finished\n");
}
else if(pid >0)
{
int status;
printf("Parent waiting for child to finish...\n");
wait(&status);
printf("Parent: child has finished. Status =%d\n",WEXITSTATUS(status));
}
else
{
perror("fork failed");
}
return 0;
}
```











