# [Pre-Linux OS Basics Sprint]
## (27/12/25&ndash;28/12/25)
## **Task:**

The Pre-Linux OS Basics Sprint requires me to read the book [*"Operating Systems: Three Easy Pieces"*](https://pages.cs.wisc.edu/~remzi/OSTEP/ "Operating Systems: Three Easy Pieces"). Specifically, the following chapters in particular:
- **Ch. 2 Introduction to Operating systems**
- **Ch. 4 The Abstraction: The Process**
- **Ch. 5 Interlude: Process API**
- **Ch. 6 Mechanism: Limited Direct Execution**
- **Ch. 39 Interlude: Files and Directories**
- **Ch. 45 Data Integrity and Protection**

## **Proof:**
### Ch. 2 Intro to Operating Systems<br>(27/12/25&ndash;29/12/25)

OSTEP's three major themes:
- **Virtualization**
- **Concurrency**
- **Persistence**

The OS's key technique: **virtualization**.<br>
The OS's API: **system calls**.

Each process has its own private virtual address space.

The OS's software that usually manages the disk (HDD, SSD): **file system**.<br>
It is responsible for storing any files the user creates in a reliable and efficient manner on the disks of the system.

#### **What OS Does**
> It takes physical resources, such as a CPU, memory, or disk, and **virtualizes** them. It handles tough and tricky issues related to **concurrency**. And it stores files **persistently**, thus making them safe over the long-term.

#### **OS Design Goals:**
- **Abstraction**
- **Concurrency**
- **Protection**/**Isolation**
- **Security**
- **Reliability**
- **Energy-Efficiency**
- **Mobility**

**Procedure calls**, as opposed to **system calls**, occur within the same user program. **System calls** are between the application and the OS, and require a privilege-level switch from `User Mode` to `Kernel Mode`.


<hr>
<br>


### Ch. 4 The Abstraction: The Process<br>(30/12/25&ndash;31/12/25)

The abstraction provided by the OS of a running program is called a **Process**.

**Time Sharing:** Running one process, then stopping it and running another, and so forth. This is how the OS provides the illusion of nearly-endless CPUs, allowing users to run many concurrent processes.

**Space Sharing:** Where a resource is divided (in space) among those who wish to use it. For example, with disk space.

Virtualization of the CPU is implemented using both some low-level machinery (**mechanisms**) and some high-level intelligence (**policies**).

**Mechanisms:** The low-level methods or protocols that implement a needed piece of functionality (system calls, context switches, memory allocation).
> <!-- --- -->
> **\*\*NOTE**** <br>
**Context Switch:** Gives the OS ability to stop running one program and start running another on a given CPU.
> <!-- --- -->

**Policies:** Algorithms for making some kind of decision within the OS (**e.g.**, scheduling policy).
> <!-- --- -->
> **\*\*NOTE**** <br>
**Scheduling Policy:** Given a number of programs to run on a CPU, this decides the order and duration for which competing processes or threads get access to the CPU.
> <!-- --- -->

**Machine State:** What a program can read or update when it is running. At any given time, what parts of the machine are important to the execution of this program?<br>
A process's machine state consists of:
- Memory (Address space)
- Registers
  - Program Counter (PC)/Instruction Pointer (IP)
  - Stack Pointer
  - Frame Pointer
- I/O Information

#### **Process API:**
- **Create**<br>
  OS must have some method to create new processes.
- **Destroy**<br>
  OS must have some method to destroy processes forcefully.
- **Wait**<br>
  Wait for a process to stop running.
- **Miscellaneous Control**<br>
  For example, suspend process, resume process, etc.
- **Status**<br>
  Interface to get status information about process.

#### **Process Creation (Program&rarr;Process)**
- Programs initially reside on **disk** in some kind of executable format.
- OS must **load** the program's code and static data (initialized variables) into the process's address space in memory.
- This is done **lazily**, as needed during execution.
- Some memory must be allocated for the program's **stack**<br>
  *(Used for local variables, function parameters, and return addresses)*.
- OS will also likely initilize the stack with arguments to `main()`
- OS may also allocate some memory for the program's **heap**<br>
  *(used for explicitly requested dynamically-allocated data)*.
- OS will also do other initialization tasks like **I/O setup** (by default, 3 open **file descriptors**: for standard input, standard output, and standard error).

#### **Process States**
- **Running**<br>
  A process is running on a processor. It is executing instructions.
- **Ready**<br>
  Process is ready to run but OS has chosen not to at this given moment.
- **Blocked**<br>
  Process has performed some kind of operation that makes it not ready to run until some other event takes place. **e.g.,** an I/O request to a disk.

Being moved from ready to running means the process has been **scheduled**.<br>
Being moved from running to ready means the process has been **descheduled**.<br>
![Process State Transitions](img/2.PNG)

**Zombie state:** Process has exited, but not yet been cleaned up.

> A **Process List** contains information about all processes in the system. Each entry is found in what is sometimes called a **process control block (PCB)**, which is really just a structure that contains information about a specific process.


<hr>
<br>


### Ch. 5 Interlude: Process API<br>(09/02/26&ndash;)

The UNIX process-creation API: `fork()`, `exec()`, `wait()`.

#### **The `fork()` System Call**
The **`fork()`** system call is used to create a new process. The **child** process is a nearly identical copy of the **parent**.
<br>
`fork()` duplicates the process at that point, and both copies (**parent** and **child**) keep running the same code afterward.
<br>
The CPU **scheduler** determines which process runs first.

#### **The `wait()` System Call**
The **`wait()`** system call delays the calling **parent process's** execution until (one of) its child process(es) finishes executing.


#### **The `exec()` System Call**
The **`exec()`** family of system calls replace the current process image with a new, different program. (PID is preserved)
<br>
This allows child to break from its similarity to its parent and execute new programs.


The **`kill()`** system call is used to send **signals** to a process, including directives to pause, die, etc.

A **superuser** (administrator) can controll all processes.


<hr>
<br>


### Ch. 6 Mechanism: Limited Direct Execution<br>(11/02/26&ndash;14/02/26)

The CPU is  virtualized via **time sharing**.<br>
The OS must virtualize the CPU in an **efficient** manner while retaining **control** over the system.

This is done through **Limited Direct Execution (LDE):** program is run directly on CPU while control is maintained through *restricted privilege modes* and *timer interrupts*.

**User Mode:** Execution mode where process can't perform restricted operations (e.g., issuing I/O requests).

**Kernel Mode:** Mode that OS kernel runs in. Code can do whatever it likes, including restricted operations.

How OS allows user processes to perform privileged operations: **system call**.

To execute a system call, a program must execute a special **Trap** instruction (***Trap:** special instruction to jump into kernel and execute the privileged system call operation*).

When finished, OS calls a special **return-from-trap** instruction (***Return-from-trap:** returns into calling user program and reduces privilege back to user mode*).

How does trap know what code to run inside the OS? It looks up relevant **trap handler** in the **trap table**, set up at boot time.

Each system call has a an assigned **system-call number**.<br>
The trap handler checks registers for this number, and if valid, executes the system call code.

**Timer Interrupt:** An interrupt that is raised every few miliseconds. The currently running process is halted, and an **interrupt handler** in the OS runs.<br>
This ensures OS can take control of the CPU from processes, whether they cooperate or not.

> <!-- --- -->
> Every time **timer interrupt** occurs, the user registers of the running process are implicitly saved by the hardware.
>
> This is different from the kernel registers saved by OS upon **context switch**.
> <!-- --- -->

Every time OS regains control, the **Scheduler** determines whether to execute a **Context Switch**.

**Context Switch:** OS saves a few register values for the currently-executing process (*onto kernel stack*) and restores a few for the soon-to-be-executing process (*from kernel stack*). Now, after return-from-trap, instead of returning to the running process, system switches to executing another process.


<hr>
<br>


### Ch. 39 Interlude: Files and Directories<br>(15/02/26&ndash;18/02/26)

**Process** is a virtualization of the CPU.<br>
**Address space** is a virtualization of memory.<br>
**Files** and **Directories** virtualize persistent storage.

Each file has some kind of low-level name: **inode number (i-number)**. (This is separate from file descriptors)

A directory is a collection of tuples, each of which maps a human-readable name to a low-level name.<br>
Each entry in a directory refers to either files or other directories, making a **directory tree**/**directory hierarchy**.

The directory hierarchy starts at a **root directory** and uses a **separator** (/) to name subsequent **sub-directories**.

<br>

**File Descriptor:** An integer, private per process, used in UNIX to access files.

**Open File Table:** System-level data structure, tracking files, the current offset, whether the file is readable or writable, and other metadeta.<br>
Each process maintains an FD array, each entry of which is a pointer to an entry in the system-wide open file table.

For each file a process opens, the OS tracks a "current" **offset**, which determines where the next read or write will begin reading from/writing to.
<br>
System call `lseek()` can be used to move this offset. The offset is also incremented after regular `read()` and `write()` system calls.

Different processes have separate, independent entries on the **open file table**, even for the same file.<br>
Entries are only ***shared*** in two cases: **`fork()`** (*child process*) and **`dup()`** (*new FD, same underlying open file*).

**Reference Count:** Counter tracking how many processes are sharing a given open file table entry.

`write()` doesn't immediately write to persistent storage; it saves to buffer first for some time.
<br>
**`fsync(int fd)`:** Forces dirty data to disk immediately.

`rename(char *old, char *new)` is **atomic**.

To view file metadata: `stat()` or `fstat()`.<br>
Metadata info is kept as a data structure called an **inode**.<br> Inodes reside on disk, active ones are cached in memory.

**To delete a file**: `rm` on terminal, which uses `unlink()` system call in the background.


**`mkdir()`:** System call to **create a directory**.<br>
Just `mkdir` on terminal.

An empty directory has two entries: one entry that refers to itself (`./`), and one entry that refers to its parent (`../`). 

**To read a directory**: `ls` on the terminal, which uses `opendir()`, `readdir()`, and `closedir()` system calls.

**To delete a directory**: `rmdir` on terminal, which uses `rmdir()` system call.<br>
Will only delete empty directories.

> <!-- --- -->
> **\*\*NOTE**** <br>
> Each directory&mdash;even an empty&mdash;contains two special entries: `.`, which refers to itself, and `..`, which refers to its parent.
> <!-- --- -->

**`link()`:** System call to create a new directory entry (a "**hard link**") for an existing file (*same inode number, so not a copy*). Takes two arguments: old pathname and new pathname.<br>
`ln` on terminal.

**Reference Count/Link Count:** Allows file system to track how many different names have been linked to a given inode number.<br>
When link count reaches zero, the file system frees the inode and related data blocks (i.e., actually deletes the file).

You can't hard link to a directory or to files in other disk partitions.

> <!-- --- -->
> **\*\*NOTE**** <br>
> inode numbers are only unique within a particular file system, and a file system is self-contained within each partition.
> <!-- --- -->

**Symbolic Link / Soft Link:** Same as hard link in function except a symbolic link is actually a file itself, of type 'symbolic link'.<br>
The link file holds the pathname of the linked-to file (*like a pointer*).
Possibility of **dangling reference** if linked-to file is deleted.
`ln -s` on terminal.

> <!-- --- -->
> **\*\*NOTE**** <br>
> A 'symbolic link' is a separate third type of file, like 'regular file' or 'directory'.<br>
> On the ouput of `ls -al`, the first character in the left-most column is `-` for regular files, `d` gor directories, and `l` for soft links:<br>
> ```
> prompt> ls -al
> drwxr-x---  2 remzi remzi   29 May  3 19:10 ./
> drwxr-x--- 27 remzi remzi 4096 May  3 15:14 ../
> -rw-r-----  1 remzi remzi    6 May  3 19:10 file
> lrwxrwxrwx  1 remzi remzi    4 May  3 19:10 file2 -> file
> ```
> <!-- --- -->

Virtualizations of CPU and memory provide the illusion of a *private* CPU and *private* meomry for each process/user. Virtualized disk does not (and should not) have this as files are often *shared* between processes.<br>
Mechanisms to facilitate this: **Permission Bits** and **Access Control Bits (ACL)**.

**Permission bits:** Determine for each regular file, directory, or other, exactly who can access it and how.<br>
The permissions consist of three groupings: what the **owner** of the file can do to it, what someone in a **group** can do, and what anyone (or "**other**") can do.<br>
The abilities these groups can have are to *read*, *write*, or *execute*.

```
prompt> ls -l foo.txt
-rw-r--r--  1 remzi wheel  0 Aug 24 16:29 foo.txt
```
The first `-` just shows that `foo.txt` is a regular file. The permission bits are the next nine characters (`rw-r--r--`), showing that the file is both readable and writable by owner (`rw-`), only readable by members of the group `wheel` (`r--`), and only readable by anyone else (`r--`).

Owner of the file can change permission bits using **`chmod`** command.

Execute bit for regular files determine whether you can run a file.<br>
For directories, execute bit determines whether you can `cd` into the directory.

Some file systems like AFS (Andrew File System) have more sophisticated controls like **access control list (ACL)**.

**Access Control List:** More general and powerful way to represent exactly who can access a given resource.<br>
Rather than owner/group/other model, ACL enables user to create a specific list of who can and can't read a set of files.

**`mount()`:** Takes an existing directory as a target mount point and essentially pastes a new file system onto the directory tree at that point.<br>
`mount` on terminal.


<hr>
<br>


### Ch. 45 Data Integrity and Protection<br>(18/02/26&ndash;)


## **Takeaway:**