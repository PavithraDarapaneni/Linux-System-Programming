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
## Describe the role of exec() family of functions in process management.
```c
* The exec() family of functions in linux replaces the current process image with a new program
* After successful exec(), the old code,data,stack and heap are replaces with the new program.
* The PID remains the same meaning the process identity does not change,but its contents do.
* If exec() succeeds,it never returns,if it fails it returns -1.
* We can also say that the exec() family of functions plays a vital role in process management by replacing a process memory image with a new program, enabling execution of commands and ensuring cooperation between fork()[for creating a process] and exec()[for running a new program inside it].

Example using fork() + exec()
#include<stdio.h>
#include<unistd.h>
int main()
{
pid_t pid = fork();
if(pid == 0)
{
printf("child: replacing process with 'ls' command...\n");
execlp("ls","ls","-l",NULL);
perror("exec failed");
}
else if (pid > 0)
{
printf("Parent: I created child with PID = %d\n",pid);
}
else
{
perror("Fork failed");
}
return 0;
}
```

## Write a C Program to illustrate the use of execvp() function
```c
The function execvp() is useful because it
i) Accepts the program name and arguments as a vector(array of strings).
ii) Searches the program in the system's path environment variable.

Example:

#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
int main()
{
pid_t pid = fork();
if(pid<0)
{
perror("Fork failed");
return 1;
}
else if(pid == 0)
{
printf("child : Executing 'ls-l' using execvp()...\n");
char *args[] = {"ls","-l",NULL};
execvp("ls",args);
perror("execvp failed");
}
else
{
printf("Parentd: created child with PID = %d\n",pid);
}
return 0;
}
```

#










