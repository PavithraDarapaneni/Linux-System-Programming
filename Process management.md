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

## How does thevfork() system call differ from fork()?
```c
fork():
* Creates a new child process which is a duplicate of the parent.
* Both parent and child getseparate address spaces(memory).
* The child gets  copy of the parent's data,heap and stack(using copy-on-write in modern OS).
* Both processes run independently after fork(). parent and child can run concurrently.

vfork(): Also creates a new child process but:
* No new address space is created immediately
* Child shares the parents address space until it calls exec() or _exit()
* The parent is suspended until the child calls exec() or _exit().
* It is more efficient than fork() if the intention is to immediately call exec(), since it avoids copying the address space.
```
## Discuss the significance of get pid() and get ppid() system calls?
```c
get pid() system call: Returns the process ID(PID) of the calling process.
* Every process in the system is uniquely identified by a PID.
* Useful for debugging,logging or signaling(kill()) needs a PID).
* syntax: pid_t getpid(void);

get ppid() system call: Returns the parent process ID(PPID) of the calling process.
* If the parent terminates before the child, the PPID changes to that of init, which adopts orphaned process.
* Useful for synchronization or tracing process hierarchy.
* syntax: pid_t getppid(void);
Example:
#include<stdio.h>
#include<unistd.h>
int main()
{
pid_t pid = fork();
if(pid == 0)
{
printf("Child : My PID = %d\n",getpid());
printf("Child : My Parent's PID = %d\n",getppid());
}
else if (pid>0)
{
printf("Parent : My PID = %d\n",get pid());
printf("Parent : I created child with PID = %d\n",pid);
}
else
{
perror("fork failed");
}
return 0;
}
```

## Explain ther concept of process termination in UNIX-like operating systems.
```c
In UNIX-like systems, process termination can be voluntary(normal completion or error) or Involuntary(killed by OS or another process). The OS cleans up resource, sets exit status and uses wait()/wait pid() to let the parent collect the status and prevent zombies.
```
## Write a C program to create a child process using fork() and print it's PID.
```c
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
int main()
{
pid_t pid;
pid = fork();
if(pid < 0)
{
perror("fork failed");
return 1;
}
else if(pid == 0)
{
printf("child process: My PID is %d\n",getpid());
printf("child process: My parent PID is %d\n",getppid());
}
else
{
printf("Parent process: My PID is %d\n",getpid());
printf("Parent process: created child with PID %d\n",pid);
}
return 0;
}
```

## Describe the process hierarchy in UNIX-like operating systems
```c
1. Root of Hierarchy : init/systemd: This is the ancestor of all other process. All other process are created directly or indirectly by it.
2. Parent and Child Process: New Process are created using fork() system call
    * The parent process calls fork()
    * The child process is a duplicate of the parent.
   After creation, the child can:
    * Continue execution independently ot
    * Replace itself with a new program using exec()
   This creates a tree-like structure
    * Parent at the top
    * Child below it
    * Grand child and so on...
3. Process tree: All Processes in UNIX form a hierarchial trtee structure called the process ttree.
4. Orphan Process: If a parent process terminates before the child , the child becomes orphan
    * Orphan processes are automatically adopted by init/systemd(pid =1)
    * THis ensures that every process always has a parent.
5. Zombie process: When a child finishes execution but the parent hasn't called. wait() to collect its exit status, the child becomes a zombie.
   * It still appears in the process table until the parent reaps it.
```

## What is the purpose of exit() function in C Programming 
```c
exit() definition: exit() is a library function declared in <stdlib.h>. It terminates the calling process immediately.
The mainpurpose of exit() are :
1.Terminates a process gracefully : Ends the program execution
                                    Ensures all cleanup actions are performed (like flushing buffers).
2. Returns an exit status to the operating system: The integer status is returned to the parent process(via wait() or waitpid())
3. Resource cleanup: Closes all open file descriptors,Flushes output buffers.
                     Executes functions registered with atexit().
4. Difference from return in main(): Return in main() is equivalent to calling exit(status).
                                     But inside the whole program-only exit() can do that.
```
































