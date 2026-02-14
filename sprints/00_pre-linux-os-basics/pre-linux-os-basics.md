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


### Ch. 39 Interlude: Files and Directories<br>(15/02/26&ndash;)

Meow


<hr>
<br>


### Ch. 45 Data Integrity and Protection<br>(&ndash;)


## **Takeaway:**