## 1)What is meant by an IPC Mechanism?
```c
IPC (Inter-Process Communication) is a mechanism that allows processes to communicate and share data with each other. 
It helps in data exchange, synchronization, and coordination between processes.
```
## 2)Why we use IPC Mechanism?
```c
We use IPC to:
*Share data between processes.
*Coordinate actions among processes.
*Improve system efficiency and modularity.
*Enable client-server communication in multitasking systems.
```
## 3)What are the types of IPC Mechanisms?
```c
Common IPC mechanisms include:
*Pipes
*Named Pipes (FIFOs)
*Message Queues
*Shared Memory
*Semaphores
*Sockets
*Signals
```
## 4)What is meant by “unicast” and “multicast” IPC?
```c
Unicast IPC: Communication between one sender and one receiver 
(e.g., a normal pipe).
Multicast IPC: Communication between one sender and multiple receivers
(e.g., message queues or sockets in multicast mode).
```
## 5)What is meant by PIPES?
```c
A pipe is a unidirectional communication channel used between related processes(parent-child).
Data written to one end of the pipe can be read from the other end.
```

## 6)What is meant by Blocking Calls?
```c
A blocking call is a system call that waits (blocks) until an operation completes.
Example: read() will block until data becomes available.
```

## 7)What are the types of Blocking Calls?
```c
Blocking I/O: Process waits until operation finishes.
Non-blocking I/O: Process continues without waiting.
Asynchronous I/O: Process is notified when operation completes.
```

## 8)What are the different types of I/O Calls?
```c
read() – reads data from a file descriptor
write() – writes data to a file descriptor
open() – opens a file or device
close() – closes a file descriptor
select(), poll(), epoll() – monitor multiple file descriptors
```
## 9)What are the I/O calls we use in IPC Mechanisms?
```c
Common I/O calls used in IPC:
open()
read()
write()
close()
mkfifo() (for named pipes)
msgsnd(), msgrcv() (for message queues)
shmget(), shmat() (for shared memory)
```
## 10)What are the Blocking Calls used in IPC?
```c
read() – blocks until data is available
write() – blocks if the buffer is full
msgrcv() – waits for a message
accept() – waits for a connection (in sockets)
```
 
## 11)What is meant by Named Pipes?
```c
Named Pipes (FIFOs) are special files that allow unrelated processes to communicate using a name in the filesystem (unlike anonymous pipes which need a relationship).
```
## 12)Where is the FIFO Object created?
```c
The FIFO object is created in the filesystem (directory structure) — usually as a special file visible in the directory.
```
```c
13. What is the call used to create a FIFO Object?
The system call:
int mkfifo(const char *pathname, mode_t mode);
Example: mkfifo("/tmp/myfifo", 0666);
```
## 14)What are the Blocking Calls used in Named Pipes?
```c
open() – blocks until both reader and writer are ready.
read() – blocks until data is available.
write() – blocks if no reader or buffer is full.
```
 
## 15)Why read system call acts as a blocking call?
```c
Because read() waits until:Data is available in the pipe or queue, or the writer closes the pipe (end of file).
It ensures the process doesn’t proceed with incomplete data.
```
 
## 16)Difference between Named Pipes and Pipes?
```c
Feature	            			Pipe						Named Pipe (FIFO)
Relationship	Between related processes (parent-child)	Between any processes
Identification	No name (anonymous)							Has a name in the filesystem
Persistence		Exists until process ends					Exists until deleted
Creation		pipe() system call							mkfifo() system call
```
 
## 17)What is return value of read system call?
```c
Returns number of bytes read if successful.
Returns 0 if end-of-file (EOF).
Returns -1 on error.
```
 
## 18)What is meant by message queue?
```c
A message queue is an IPC mechanism that allows processes to send and receive messages (structured data) using a queue.
Messages are stored in kernel memory until received.
```
 
## 19)Why we use message queues?
```c
We use message queues to:
Exchange structured data between processes.
Support communication between multiple senders and receivers.
Provide asynchronous and ordered message delivery.
```
 
## 20)What is difference between Named Pipe and Message Queue?
```c
Feature				Named Pipe				Message Queue
Data type		Byte stream					Message-based (structured)
Order			FIFO (first-in-first-out)	Priority + FIFO
Persistence		Until closed				Until explicitly removed
Communication	One-to-one					One-to-many
System calls	mkfifo(), read(), write()	msgget(), msgsnd(), msgrcv()
```
 
## 21)What is the system call used to create the message queue?
```c
The system call used is: int msgget(key_t key, int msgflg);
key → unique identifier for the message queue
msgflg → permissions and creation flags (e.g., IPC_CREAT)
Example: int msgid = msgget(1234, IPC_CREAT | 0666);
```
 
## 22)Where was the message queue created?
```c
The message queue is created in the kernel memory (not in user space).
It remains there until it is explicitly deleted or the system is rebooted.
```
 
## 23)What is meant by Shared Memory?
```c
Shared Memory is an IPC mechanism where multiple processes share a common memory region.
This allows them to directly read and write data without copying through the kernel, making it very fast.
```
## 24)Why we use Shared Memory?
```c
We use shared memory because:
It provides fast communication (no message copying).
It allows large data exchange efficiently.
It is ideal when multiple processes need to access and modify common data.
```

## 25)Difference between Shared Memory and Message Queues
```c
Feature				Shared Memory					Message Queue
Communication Type	Shared region of memory			Message-based communication
Speed				Very fast (direct access)		Slower (involves kernel copying)
Synchronization		Needs semaphores for sync		Automatically synchronized
Data Format			Any (raw data)					Structured messages
Use Case			Large data, frequent access		Small, structured data exchange
```
 
## 26)What is use of stat command?
```c
The stat command is used to display detailed information about files and IPC objects (like inodes, permissions, ownership, timestamps, etc.).
Example:stat /tmp/myfifo
→ shows info about the FIFO (named pipe) object.
```
 
## 27)What is the use of semctl command?
```c
semctl is a system call used to control semaphore operations, such as:
Getting information about a semaphore
Setting values
Removing semaphore sets
Syntax:int semctl(int semid, int semnum, int cmd, ...);
```
## 28) How do we destroy the shared memory object?
```c
We use the system call:
shmctl(shmid, IPC_RMID, NULL);
shmid → ID of the shared memory segment
IPC_RMID → flag to remove (delete) the shared memory from the system
```
## 29) What is meant by Semaphores?
```c
A Semaphore is a synchronization tool used to control access to a shared resource by multiple processes.
It helps avoid race conditions and coordinate process execution.
There are two types:
Binary Semaphore → acts like a lock (0 or 1).
Counting Semaphore → allows multiple accesses up to a limit.
```
## 30) Why we use semaphores?
```c
Semaphores coordinate access to shared resources and provide synchronization between processes/threads. They prevent race conditions by controlling how many actors can enter a critical section (counting semaphore) or by acting as a binary lock (binary semaphore).
```
## 31)What is meant by Synchronization?
```c
Synchronization is ensuring correct ordering and timing of operations between concurrent processes/threads so they access shared resources safely and produce correct results (e.g., using mutexes, semaphores).
```
## 32)What is meant by Asynchronization?
```c
Asynchronization means operations proceed independently: a caller does not wait for an operation to complete and continues execution (e.g., non-blocking I/O, async callbacks). Completion is handled later via notification or polling.
```
## 33)Why we use mutex locks?
```c
Mutexes (mutual exclusion locks) protect critical sections by allowing only one thread at a time to execute code that accesses shared data. They are lightweight and appropriate for mutual exclusion within a single process (threads).
```
 
## 34)Difference between mutex locks and semaphores
 ```c
Mutex:
Binary (locked/unlocked).
Owned by a thread (owner can release).
Used for mutual exclusion within a process (threads).

Semaphore:
Counting (0..N).
Not owned by a specific thread (any can signal).
Used for resource counting and process-to-process sync (or threads).
```

## 35)What is meant by Race Condition?
```c
A race condition occurs when the outcome depends on the unpredictable timing/order of concurrent accesses to shared data. Without proper sync, interleavings can produce incorrect results.
```
## 36)What is meant by Deadlock?
```c
Deadlock is a situation where two or more processes/threads wait indefinitely for each other to release resources, so none can proceed. (Four Coffman conditions: mutual exclusion, hold and wait, no preemption, circular wait.)
```
## 37)What is meant by Critical Section?
```c
A critical section is a code region that accesses shared resources (data, devices) and must not be concurrently executed by more than one thread/process to avoid race conditions.
```
## 38)Difference between System V and POSIX IPC
```c
*System V IPC
Older APIs: msgget/msgsnd/msgrcv, semget/semop/semctl, shmget/shmat/shmctl.
Uses integer keys (ftok) and kernel objects identified by IDs.
*POSIX IPC
Newer, standardized interfaces: mq_open (message queues), sem_open (named semaphores), shm_open (shared memory).
Uses name strings (e.g., "/mysem"), more portable across POSIX systems.
*POSIX tends to be simpler & more consistent with file-like semantics; System V remains widely used.
```
## 39)Steps to create & use a Named Pipe (FIFO) for IPC
```c
1.Create FIFO in filesystem: mkfifo("/tmp/myfifo", 0666) (or shell: mkfifo /tmp/myfifo).
2.Process A opens FIFO for writing: open("/tmp/myfifo", O_WRONLY).
3.Process B opens FIFO for reading: open("/tmp/myfifo", O_RDONLY).
4.Writer writes: write(fd_w, buf, n).
5.Reader reads: read(fd_r, buf, n).
6.Close descriptors: close(fd_w); close(fd_r);.
7.Optionally remove FIFO: unlink("/tmp/myfifo").
```
## 40)Steps to create & use a (anonymous) pipe for IPC
```c
Create pipe: int fds[2]; pipe(fds); — fds[0] read end, fds[1] write end.
fork() a child.
Parent closes unused end (e.g., close read end in parent if parent writes).
Child closes unused end (e.g., close write end in child if child reads).
Use write(fds[1], buf, n) and read(fds[0], buf, n).
Close when done; parent can wait() for child if needed.
```
