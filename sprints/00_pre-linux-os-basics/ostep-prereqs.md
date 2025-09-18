# [Pre-Linux OS Basics Sprint / OSTEP Prereq Knowledge]
## Day 18 (1/9/25 - 18/9/25)
**Task:**

The Pre-Linux OS Basics Sprint requires me to read the book [*"Operating Systems: Three Easy Pieces"*](https://pages.cs.wisc.edu/~remzi/OSTEP/ "Operating Systems: Three Easy Pieces"). However, the book starts with the following warning:

> If you are taking an undergraduate operating systems course, you should already have some idea of what a computer program does when it runs. If not, this book (and the corresponding course) is going to be difficult — so you should probably stop reading this book, or run to the nearest bookstore and quickly consume the necessary background material before continuing (both Patt & Patel [[PP03]](https://icourse.club/uploads/files/96a2b94d4be48285f2605d843a1e6db37da9a944.pdf "Introduction to Computing Systems: From Bits and Gates to C and Beyond") and Bryant & O’Hallaron
[BOH10](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/books/CSAPP_2016.pdf "Computer Systems: A Programmer’s Perspective") are pretty great books).

Since I don't have the time to read these books, this file contains all prereq topics I need knowledge of before OSTEP (with notes).

## 1) Data and representation
### Integers (two's complement)
Two's complement (as opposed to Sign-Magnitude) is a way to reprsent both positive and negative integers in binary so that the computer's arithmetic logic unit (ALU) can work with them.

Example:
- `+5` would be `0000 0101`
- `-5` would be `1111 1011`

The **Most Significant Bit (MSB)** is the sign bit. (`0` for postive, `1` for negative.)

For an `n`-bit number, the range of values is from `-2^(n-1)` to `2^(n-1) - 1`.
- **8-bit Example:** The range is `-128` (`-2^7`) to `+127` (`-2^7 - 1`)

To represent the negative version of a positive number `X` (e.g., `-1`):
1. Start with the positive binary representation (`+1`): `0000 0001`
2. Invert all the bits: `0000 0101` &rarr; `1111 1110`
3. Add `1` to the results: `1111 1110 + 1 = 1111 1111`

<br>

### Floating point (IEEE-754)

IEEE-754 is a technical standard that defines how computers represent and handle floating-point numbers. A float is split into three parts:

- **Sign bit (1 bit):** `0` if positive, `1` if negative.
- **Exponent (8 bits for 32-bit float):** Represents the "power" (like `23` in `10²³`). It's stored with a bias to handle negative exponents, meaning `Actual Exponent = Stored Exponent - Bias`)<br>
For 32-bit floats, this bias is `127`. Stored exponent range: `1 – 254`; corresponding to actual exponents: `-126 – 127`.
- **Fraction/Mantissa (23 bits for 32-bit float):** The significant digits of the number (like the `6.022` from `6.022 x 10²³`).

The formula for the value of a IEEE-754 float is: <br>
`Value = (-1)^Sign x (1 + Fraction) x 2^(Exponent - Bias)`

#### **Example 1: Storing 0.15625 in 32-bit float**

**Step 1) Determine the Sign Bit**
<br>
Since `0.15625` is positive, the sign bit is **`0`**.

**Step 2) Convert `0.15625` to binary:**
<br>
To convert a decimal fraction to binary, multiply by 2 repeatedly and take the integer parts (`0` or `1`) until the fraction becomes zero or until you have enough bits. For `0.15625`:
- 0.15625 x 2 = 0.3125 &rarr; **integer part `0`**
- 0.3125 x 2 = 0.625 &rarr; **integer part `0`**
- 0.625 x 2 = 1.25 &rarr; **integer part `1`**
- 0.25 x 2 = 0.5 &rarr; **integer part `0`**
- 0.5 x 2 = 1 &rarr; **integer part `1`**

Reading the integer parts from top to bottom, we get `0.00101₂`. Thus, `0.15625 = 0.00101₂`.

**Step 3) Normalize to Scientific Notation**
<br>
In binary scientific notation, we express the number as 1.xxxx x 2<sup>exponent</sup>. For `0.00101₂`, we move the binary point three places to the right to get `1.01₂`. This is equivalent to multiplying by `2⁻³`. So:
- Value: `1.01₂ x 2⁻³`
- The leading `1.` is implicit in the representation (not stored), so we focus on the fractional part "`01`".

**Step 4) Calculate the Stored Exponent**
<br>
The actual exponent is `-3`. We add the bias (`127`) to get the stored exponent: `-3 + 127 = 124`
<br>
Now, convert 124 to binary (8 bits): `01111100`

**Step 5) Extract the Fraction (Mantissa)**
<br>
The normalizaed value is `1.01₂`. The fraction part is the bits after the decimal point, which is "`01₂`". However, since IEEE-754 requires a 23-bit fraction, we pad with zeros to the right: `01000000000000000000000₂`.

> <!-- --- -->
> \*\*NOTE** <br>
> Converting this from binary to decimal: `1.01₂` = 1 x 2<sup>0</sup> + 0 x 2<sup>-1</sup> + 1 x 2<sup>-2</sup> = `1.25`
> <!-- --- -->

**Step 6) Full binary representation:**
<br>
The 32-bit float consists of:
- Sign bit: 1 bit (`0`)
- Exponent: 8 bits (`01111100₂`)
- Fraction: 23 bits (`01000000000000000000000₂`)

Thus, the full binary representation is: <br>
**`0011 1110 0010 0000 0000 0000 0000 0000`**<br>
The hexadecimal representation is: `0x3E200000`.

#### **How the Float is Read from Memory and Interpreted**

**Step 1: Read and Reconstruct the 32-Bit Value**
<br>
The CPU fetches **4 bytes** from memory addresses `0x1000` to `0x1003`. Assuming the system is **litte-endian**, the CPU then reverses the order of the bytes to get the correct 32-bit value: `0x3E200000`.

**Step 2: Interpret the Bits**
<br>
The CPU parses the 32-bit string according to IEEE-754, splitting it into the three defined fields:
- **Bit 31 (The Sign Bit):** `0`, meaning the number is **positive**.
- **Bits 30-23 (The Exponent Bits:** `01111100` &rarr; `124`.
- **Bits 22&ndash;0 (The Fraction Bits):** `01000000000000000000000`

**Step 3: Apply the IEEE-754 Formula**
<br>
`Value = (-1)^Sign x (1 + Fraction) x 2^(Exponent - Bias)`
1. **Calculate the Sign:**
    `(-1)^Sign` = (-1)^0 = `1`
2. **Calculate the true Exponent:**
    `Exponent - Bias` = 124 - 127 = `-3`
3. **Calculate the Significand (1+ Fraction):**<br>
    `1.01000000000000000000000₂` = 1 x 2<sup>0</sup> + 0 x 2<sup>-1</sup> + 1 x 2<sup>-2</sup> + 0 x 2<sup>-3</sup> + . . . + 0 x 2<sup>-23</sup> = 1 + 0 + 0.25 + 0 + . . . + 0 = `1.25`.
4. **Final Value:**<br>
    `1 x 1.25 x 2⁻³ = 0.15625`

<br>

<!-- 
#### **Example 2: Storing 35.7911 in 32-bit float**

**Step 1) Sign Bit**:
<br>
Since `35.7911` is positive, the sign bit is **`0`**.

**Step 2) Convert `35.7911` to binary:**

- **Integer part (`35`):**<br>
    35 ÷ 2 = 17 remainder `1`<br>
    17 ÷ 2 = 8 remainder `1`<br>
    8 ÷ 2 = 4 remainder `0`<br>
    4 ÷ 2 = 2 remainder `0`<br>
    2 ÷ 2 = 1 remainder `0`<br>
    1 ÷ 2 = 0 remainder `1`

    So, `35` in binary is `100011`.
- **Fraction part (0.7911):**<br>
    0.7911 x 2 = 1.5822 &rarr; `1`<br>
    0.5822 x 2 = 1.1644 &rarr; `1`<br>
    0.1644 x 2 = 0.3288 &rarr; `0`<br>
    0.3288 x 2 = 0.6576 &rarr; `0`<br>
    0.6576 x 2 = 1.3152 &rarr; `1`<br>
    0.3152 x 2 = 0.6304 &rarr; `0`<br>
    0.6304 x 2 = 1.2608 &rarr; `1`<br>
    0.2608 x 2 = 0.5216 &rarr; `0`<br>
    0.5216 x 2 = 1.0432 &rarr; `1`<br>
    0.0432 x 2 = 0.0864 &rarr; `0`<br>
    0.0864 x 2 = 0.1728 &rarr; `0`<br>
    0.1728 x 2 = 0.3456 &rarr; `0`<br>
    0.3456 x 2 = 0.6912 &rarr; `0`<br>
    0.6912 x 2 = 1.3824 &rarr; `1`<br>
    0.3824 x 2 = 0.7648 &rarr; `0`<br>
    0.7648 x 2 = 1.5296 &rarr; `1`<br>
    0.5296 x 2 = 1.0592 &rarr; `1`<br>
    0.0592 x 2 = 0.1184 &rarr; `0`<br>
    0.1184 x 2 = 0.2368 &rarr; `0`<br>
    . . .
    
    So, the fractional part in binary is approximately `1100101010000101100...`
- **Combine integer and fractional parts:**<br>
    `100011.1100101010000101100...`

**Step 3) Normalize to Scientific Notation**
<br>
Move the binary point 5 places to the left:<br>
`1.000111100101010000101100...` x `2^5`.

**Step 4) Calculate the Stored Exponent**
<br>
The actual exponent is `5`. To get the stored exponent: `5 + 127 = 132`.
<br>
Now, convert 132 to binary: (8 bits): `1000 0100`

**Step 5) Extract the Fraction (Mantissa)**
<br>
The significand is `1.000111100101010000101100...`, so the fraction bits are `00011110010101000010110` (23 bits).

**Step 6) Full binary representation:**
<br>
- **Sign:** `0`
- **Exponent:** `10000100`
- **Fraction:** `00011110010101000010110`

Thus, the full binary representation is: <br>
**`0100 0010 0000 1111 0010 1010 0001 0110`**<br>
The hexadecimal representation is: `0x420F2A16`.
-->


### Endianness

**Endianness** defines whether the most significant byte (MSB) or least significant byte (LSB) is placed at the lowest memory address when storing multi-byte data types in computer memory.

**Big-Endian**
<br>
`0x12` &rarr; `0x32` &rarr; `0x56` &rarr; `0x78` = `0x12345678`

**Little-Endian**
<br>
`0x78` &rarr; `0x56` &rarr; `0x32` &rarr; `0x12` = `0x12345678`

#### How to Determine Endianness
``` c
#include <studio.h>
int main()
{
    int num = 0x12345678
    char *byte = (char *)&num;

    if (*byte == 0x78)
    {
        printf("Little-Endian\n");
    }
    else if (*byte == 0x12)
    {
        printf("Big-Endian\n");
    }
    return 0;
}
```


### Alignment and Padding

#### **Alignment**
The requirement that data types in memory must start at specific memory addresses that are multiples of their size. Because CPUs read memory in chunks (called **`memory words`** or **`cache lines`**), and we want to avoid bit-shifting or crashes. For example,
- A 4-byte `int` should start at an address divisible by 4.
- A 8-byte `double` should start at an address divisible by 8.

> <!-- --- -->
> \*\*NOTE** <br>
> A **`Memory Word`** is the basic unit of data a CPU can work with in a single operation. On a modern 64-bit system, typically 8 bytes.
>
> A **`Cache Line`** is a larger block of memory (e.g. 64 bytes) that the CPU's cache loads from the main RAM all at once to be ready for super-fast access.
> <!-- --- -->

#### **Padding**
Extra bytes inserted by the compiler between fields in a `struct`/`class` to ensure each field is properly aligned.<br>
It can also be added at the end of a `struct` to ensure alignment in arrays.

#### **Example:** `struct X { char c; int i; };`
1. **Member Sizes:**
   - `char c`: 1 byte
   - `int i`: 4 bytes
2. **Without Padding:**
   - Let the struct started at address `0x1000`:
     - `c` would be at `0x1000` (aligned)
     - `i` would start at `0x1001` (misaligned)
3. **Compiler Adds Padding:**
   - To align `i`, the compiler inserts **3 bytes of padding after** `c`:
     - `c` at `0x1000`
     - Padding at `0x1001`,`0x1002`,`0x1003`.
     - `i` starts at `0x1004` (aligned).
4. **Total Size:**
    - `char c`: 1 byte
    - Padding: 3 bytes
    - `int i`: 4 bytes
    - **Total: 8 bytes**


<hr>
<br>


## 2) From source to a running program (the whole pipeline)

### The toolchain: preprocess &rarr; compile &rarr; assemble &rarr; link

####  **1\. Preprocessing (`cpp`)** 

The preprocessor handles all lines beginning with a `#` directive.
<br>
**Key Operations:**
- **`#include` Expansion** <br>
Finds the specified header files (like `stdio.h`) and literally copy-pastes their contents in source file.
- **Macro Expansion** <br>
Replaces all instances of defined macros (like `MESSAGE`) with their actual values.
- **Conditional Compilation** <br>
Processes `#if`, `#ifdef`, `#endif` directives, including or excluding blocks of code based on defined conditions.

**Input**: `hello.c`, **Output**: `hello.i`

####  **2\. Compilation (`cc1`)** 

The compiler takes the preprocessed code and translates it into **assembly language**.
<br>
**Key Operations:**
- **Parsing & Syntax Analysis** (checking for syntax errors)
- **Semantic Analysis** (type checking)
- **Optimization** (high-level optimizations)
- **Code Generation** (outputs a human-readable assembly language `.s` file)

**Input**: `hello.i`, **Output**: `hello.s`

#### **3\. Assembly (`as`)**

Takes the assembly code and translates it into machine code (pure binary)
<br>
**Key Operations:**
- **Mnemonic Translation**<br>
  Converts human-readable assembly mnemonics into their binary opcodes.
- **Resolving Addresses**<br>
  Assigns preliminary addresses to instructions and data.
- **Creating the Object File**<br>
  Outputs a structured file containing:
  1. **Machine Code**<br>(binary)
  2. **A Symbol Table**<br>(directory of names (`_main`, `_printf`, etc.) in the file, noting which ones are defined vs undefined)
  3. **Relocation Information**<br>(instructions for the linker on how to adjust addresses once final locations are known)


**Input**: `hello.s`, **Output**: `hello.o`

#### **4\. Linking (`ld`)**

The linker combines one or more `.o` object files and libraries into a single, executable file.
<br>
**Key Operations:**
- **Symbol Resolution**<br>
  The linker's **main job**. It looks at the symbol tables from the `.o` files, and when it sees an undefined symbol, it searches through all the linked libraries and other `.o` files to find its definition.
- **Relocation**<br>
  Assigns final memory addresses to all the code and data sections. It then goes back and updates all the preliminary addresses that the assembler created with these final addresses (using the Relocation Information from the **Symbol Table**).
- **Library Handling**<br>
  It can link two types of libraries:
  - **Static Libraries**<br>
  `.a` on Linux, `.lib` on Windows.
  The library code is physically copied into final executable.
  - **Dynamic/Shared Libraries**<br>
  `.so` on Linux, `.dll` on Windows.
  The library code is only referenced in the final executable, so the library is loaded into memory at runtime.

**Input**: `hello.o` + `libc.a`, **Output**: `hello` (executable)

#### **Summary:**
```
hello.c (Source)
      |
      v Preprocessing (cpp)
hello.i (Preprocessed)
      |
      v Compilation (cc1)
hello.s (Assembly)
      |
      v Assembly (as)
hello.o (Object File) + libc.a (Library)
      |               /
      v Linking (ld) /
   hello (Executable)
```

> <!-- --- -->
> \*\*NOTE** <br>The Python "Toolchain", by contrast, is a two-stage process.
> 1. Compilation to ByteCode
> 2. Interpretation by the Python Virtual Machine
> 
> <img src="img/0.svg" alt="SVG Image" width="40%"/>
> <!-- --- -->

<br>

### Executable layout

An executable file is a highly structured toolbox.
This structure is divided into named **sections** (or **segments** when loaded into memory), each with a specific purpose and set of permissions (read, write, execute).

#### **The Common Sections/Segments**
##### **1. `.text` (Code Segment)**
- **Contents**<br>
  All the executable machine code (i.e. the actual instruction for the CPU).
- **Permissions: Read-Only and Execute**
  - **Read-only** to prevent a program from accidentally modifying its own instructions.
  - **Executable** so the CPU can run the instructions.

##### **2. `.rodata` (Read-Only Data)**
- **Contents**<br>
  Constants and literal values that should never change during execuation.
- **Permissions: Read-Only and Execute**<br>

##### **3. `.data` (Initialized Data)**
- **Contents**<br>
  Global and static variables that have an explicit initial value set in the source code.
  > <!-- --- -->
  > \*\*NOTE** <br>Physically stored in the executable file on disk.
  > <!-- --- -->
- **Permissions: Read-Write**<br>
  Because these variables are supposed to be changed during runtime.

##### **4. `.bss` (Block started by Symbol)**
- **Contents**<br>
  Global and static variables that are initialized to zero or have no explicit initializer.
  > <!-- --- -->
  > \*\*NOTE** <br>Occupies no physical space on the disk in the executable file. This section in the executable is more like a note saying "When you load this program, please allocate 10,000 bytes of memory and fill it all with zeros.
  > <!-- --- -->
- **Permissions: Read-Write**<br>

##### **5. "Metadata" and Instructions for the OS**
Beyond the raw data and code, the executable file contains **headers** full of crucial metadata that tells the OS how to load and run it.
1. **The Entry Point (`_start`)**<br>
   The file specifies exactly where the OS should start executing the program. The entry point is usuallly a symbol named `_start`.
2. **Dynamic Linking Information**<br>
   Contains a "shopping list" of required shared libraries and a list of symbols needed from them. When the OS loads the program, the **dynamic linker** reads this list to perform dynamic linking.

#### **Visual Summary: From Disk to Memory**
<img src="img/1.svg" alt="SVG Image">


<hr>
<br>


## 3) Program Start: From Command to Running Process
Imagine we type `./my_program` into shell and hit enter. Here's what happens, step-by-step.

### **Step 1: The `execve` System Call**

The shell calls the the `execve` system call to execute program.
- **`execve`'s Job:** To replace the current process's memory with the contents of a new program file and then start running that new program.

The shell passes the path to the executable file (`./my_program`) and the arguments (`argv`) and environment variables 
(`envp`) to the kernel. The kernel now takes over.

### **Step 2: The Kernel's Loader**

The kernel's "**loader**" is responsible or setting up a new process in memory. Here's what it does:

**1\. Creates a New Process with a Fresh Virtual Address Space**<br>
Creates (or reuses) a process structure. A key part of this is creating a brand new, empty **virtual address space**.

> <!-- --- -->
> \*\*NOTE** <br>
> The **process structure** or **Process Control Block (PCB)** is a data structure inside the kernel's memory that contains all the information the OS needs to manage and track a specific process.<br>
> It consists of:
> - **Process ID (PID)**
> - **Process State:** Is it running, waiting, or ready to run? 
> - **Program Counter:** The address of the next instruction to execute.
> - **CPU Registers:** A saved copy of all the CPU's working memory for this process (so it can be paused and resumed)
> - **Memory Management Info:** Pointers to the virtual address space.
> - **Accounting Info:** How much CPU time has this process used?
> - **I/O Status Info:** What files, network sockets, or devices is this process using?
> <!-- --- -->

**2\. Maps Code and Data into Memory**
- Parses the headers of the executable file. It looks for the "program headers" that describe the **segments** (`.text`,`.data`, etc.) and where they need to be loaded.
- Performs **memory mapping**: setting up the process's page tables so that a specific **virtual address** points to the corresponding **physical page**. The actual code is only loaded from disk into physical RAM by the kernel when the CPU tries to access it (a "**page fault**").

> <!-- --- -->
> \*\*NOTE** <br>
> A **page fault** is when a program tries to access a portion of memory (a page) that is not currently in physical RAM.
> <!-- --- -->

**3\. Creates a Stack and an Empty Heap**
- **The Stack:** 
  Kernel allocates physical memory for the stack and maps it to a virtual address at the very top of the process's address space. The stack grows downwards. This memory is read-write.
- **The Heap:** Kernel sets up a single page of memory for the "program break" and maps it. This is the start of the heap. Initially, it's mostly empty, but it will grow upwards later (when `malloc()` or `brk()` are called).

> <!-- --- -->
> \*\*NOTE**<br>
> The "**program break**" is a kernel-maintained pointer that defines the current end of the process's data segment—the boundary between the heap's allocated memory and the unmapped memory beyond it.
> <!-- --- -->

> <!-- --- -->
> \*\*NOTE**<br>
> ```
> High Addresses  +----------------------+
>                 |      Kernel Space    | (Memory reserved for the OS)
>                 +----------------------+
>                 |                      |
>                 |         STACK        | <- Grows DOWNWARD
>                 |           |          |
>                 |           v          |
>                 |          ...         |
>                 |                      |
>                 |           ^          |
>                 |           |          |
>                 |          HEAP        | <- Grows UPWARD
>                 |                      |
>                 |          BSS         | (Uninitialized data)
>                 +----------------------+
>                 |          Data        | (Initialized data)
>                 +----------------------+
>                 |         rodata       | (Constants)
>                 +----------------------+
>                 |          Text        | (Program's code)
> Low Addresses   +----------------------+
> ```
> <!-- --- -->

**4\. Maps Needed Shared Libraries**
- Reads the list of shared libraries from the executable's headers.
- It finds these libraries on the filesystem and recursively maps their code (`.text`) and read-only date (`.rodata`) segments into the process's address space, just like it did for the main program in `2.`.

**5\. Places Initial Data on the Stack**<br>
Prepares the initial environment for the program by pushing information onto the new stack it just created: `argc`, `argv[]`, `envp[]`, `auxv`.


### **Step 3: The Handoff to `_start`**
Once the kernel has successfully built the entire virtual environment, the final step is to set the CPU's instruction pointer (`%rip`) to the address of the entry point specified in the executable header (`_start`) and let the CPU run.

#### **The Job of `_start`**
1. **Set up the C runtime environment**
2. **Extract arguments for `main`.** It reads `argc`, `argv`, and `envp` from the stack.
3. **Initialize the standard library**
4. **Call our `main` function.** (`main(argc, argv, envp)`)
5. **Handle the return.** When `main()` returns, `_start` tarkes its return value, passes it to the `exit()` system call, and terminates the process.

<br>

### User vs kernel mode

The CPU is designed with multiple levels of privilege, often called **rings**.

#### **Kernel Mode (Ring 0)**

Where the OS kernel runs.<br>
Code here has the **highest level of privilege**: full, unrestricted access to the CPU and all hardware. It can do anything.

#### **User Mode (Ring 3)**

Where all our applications (processes) run.<br>
Code running in User Mode is "sandboxed and restricted to the **lowest level of privilege**. It is physcially prevented by the CPU from:
- Direct Hardware Access
- Modifying Page Tables
- Disabling Interrupts
- Halting the CPU
- Changing Privilege Levels

> <!-- --- -->
> \*\*NOTE**<br>
> An **Interrupt** is a signal sent to the CPU that immediately grabs its attention, forcing it to temporarily stop what it's doing and execute a special piece of code called an **interrupt handler**.
> <br>
> Two types: **Hardware Interrupts** and **Software Interrupts**
> 
> Interrupts are what allow the CPU to respond to event in real-time (`e.g.` keyboard key pressed).
> 
> The computer's timer chip is advised to generate a hardware interrrupt **many times per second**. Every time the timer interrupt fires, the kernel has to decide:
> - Has the current program used up its fair share of CPU time?
> If yes, it saves the program's state and switches to running a different program.
>
> This happens extremely fast for milliseconds at a time, allowing the CPU to "multi-task".
> <!-- --- -->


<hr>
<br>


## 4) The process and its memory (while running)

### Virtual address space

**Virtual Address Space** is an abstraction provided by the OS and Memory Management Unit (MMU) that presents each running process with a private, linear range of memory addresses. It is "virtual" because it's independent of physical memory, creating the illusion that the process has exclusive access to the entire system's memory.

Virtual memory is **mapped** to physical memory by the kernel loader after `execve`, with physical RAM only being allocated **lazily** at first access.

### Heap and Stack

#### **Heap**

- A region for **dynamic memory allocation**. This is memory you explicitly request at runtime using functions like `malloc()`, `calloc()`, or `new`. It is used for data that must persist beyond the lifetime of a single function call or whose size is unknown at compile time.
- **Managed by a memory allocator library.**
- The heap **grows upward** toward higher memory. Size is managed explicitly by the programmer (with `malloc()` and `free()`).
- The top of the heap, the **program break**, is moved using `brk()`, `sbrk()`, and `mmap()`  system calls. `malloc()` uses these implicitly.

> <!-- --- -->
> \*\*NOTE**<br>
> `mmap()` for very large allocations.
> `mmap()` creates an independent mapping that is not connected to the main heap.
> <!-- --- -->

#### **Stack**

- A region for **automatic memory allocation**. It handles function calls and storage of local variables, function arguments, return addresses, and saved register values.
- **Managed by compiler**.
- Starts at a high memory address and **grows downward** toward lower addresses. Each function call pushes a "**stack frame**" onto the stack. When a function returns, its entire frame is popped off the stack. This allocation and freeing is automatic and handled by the compiler.
- A typical **stack frame** contains, in order:
  1. Function Arguments
  2. Return Address
  3. Saved Frame Pointer (a pointer to the previous stack frame)
  4. Local Variables
  5. Temporary Data

Once the stack and heap meet, the program is out of memory.

### Memory bugs and why they crash

Memory bugs crash because they violate the contracts and assumptions made by the OS and the **memory allocator**.

> <!-- -- -->
> \*\*NOTE**<br>
> The **Memory Allocator** is a software library (e.g., part of the C standard library `glibc`) that managaes a pool of memory within a process's virtual address space.
> 
> It acts as an intermediary between a program and the OS.
> 1. It asks the OS for large chunks of memory (using system calls like `brk`, `sbrk`, `mmap`).
> 2. It then takes that large chunk and splits it up into smaller pieces to fulfill your many small `malloc()` requests.
> 3. It keeps complex data structures (like linked lists) to track which parts of that large chunk are free and which are allocated.
> 4. When you call `free()`, it doesn't usually return the memory to the OS immediately. Instead, it just marks that small piece as "free" in its internal list so it can be reused by a future `malloc()` call.
> <!-- -- -->

#### 1. Out-of-Bounds Access

Reading or writing to an array or buffer outside of its allocated boundaries.

##### **Why it crashes**
A program's virtual address space is a map of **valid** (mapped to something) and **invalid** (unmapped) addresses. The allocator or the compiler gives you a pointer to a block of valid addresses. When you access memory outside that block, one of two things happens:
1. **You touch an unmapped address:**<br>
   The MMU triggers a **page fault**. The OS responds by sending the `SIGSEGV` (Segmentation Violation) signal to the process, which typically terminates it immediately.
2. **You touch a mapped address that belonds to something else:**<br>
   Might overwrite the contents of a different variable, a part of the heap's internal data structures, or even your own function's return address on the stack. This **corrupts data** but doesn't cause an immediate crash.

#### 2. Use-After-Free (Dangling Pointer)

Continuing to use a pointer after the memory it points to has been `free()`'d or has gone out of scope.

##### **Why it crashes**
Once `free` is called on a block of memory, the allocator now owns that block and can reassign it to a future `malloc()` call. If you use the old (dangling) pointer, two things can happen:
1. **The memory is reallocated:**<br>
   The allocator may have given the freed block to another part of the program. A write operation on this block corrupts the new, completely unrelated data.
2. **The memory is unmapped:**<br>
   To improve performance, the allocator may return large blocks of memory to the OS using a system call like `munmap`. Any operation on this block will result in **out-of-bounds access** (**page fault** &rarr; `SIGSEGV` system call).

#### 3. Double Free

Calling `free()` on the same pointer twice without an intervening `malloc()`.

##### **Why it crashes**
This corrupts the allocator's internal metadata.

> <!-- --- -->
> \*\*NOTE** <br>
> The allocator stores metadata in the heap, consisting of (at least) two lists:
> 1. A list of **allocated** blocks.
> 2. A list of **free** blocks
> 
> When you call `malloc`, it searches the **free list** for a block that's big enough.
> <br>
> When you call `free`, it takes the block and adds it to the **free list**.
> 
> Right before every single piece of memory that `malloc` gives you, it secretly stores a small header&mdash;a piece of metadata that describes the block that follows.
>
> ```
> +-----------------------------------------------------------------------+
> |  |              |  |              |  |                               | |
> |H | Your 100-byte|H | Your 40-byte |H |       Free 60-byte block      | |
> |E |   block      |E |   block      |E |    (on the free list)         | |
> |A |   (allocated)|A |   (allocated)|A |                               | |
> |D |              |D |              |D |                               | |
> |E |              |E |              |E |                               | |
> |R |              |R |              |R |                               | |
> +-----------------------------------------------------------------------+
> ^                  ^                  ^
> Pointer returned   Pointer returned   This block's metadata is part
> by malloc(100)     by malloc(40)      of the free list, pointing to
>                                      the next free block.
> ```
> What's in this header:
> - **Size of the block**
> - **Flags** (bits indicating if allocated or free)
> - **Pointers** (to link to the next block in the free list)
> <!-- --- -->

**The first `free(ptr)`** correctly puts the block on the "free list.":
1. It take the pointer `ptr` and goes backwards in memory to find the **metadata header** for that memory block.
2. It changes the flag in the metadata from `allocated` to `free`.
3. It splices the header to this now-free block into the **free list** linked list.

**The second `free(ptr)`**
1. The allocator takes `ptr` and goes to the same metadata.
2. It unexpectedly finds the metadata's flag status is already `free`.
3. It attempts to add the the block to the **free list** a second time, corrupting the linked list.

The crash often doesn't happen at the moment of the double-free. It's triggered later by a subsequent `malloc()` or `free()` call.

#### 4. Memory Leak

Allocating memory with `malloc()` but never `free()`ing it.

##### **Why it crashes**
It doesn't cause an immediate crash, it slowly **wastes memory**.

The heap grows and grows until the process runs out of available virtual address space or the system runs out of physical RAM. At this point, any `malloc()` call will fail and return `NULL`. Trying to use this `NULL` pointer will cause the program to crash.


<hr>
<br>


## 5) Calling conventions and the stack (how functions really call)

### The Need for a Calling Convention

When one function calls another, they need a strict agreement on:
1. **Where to put the arguments** so the callee can find them.
2. **Where to put the return value** so the caller can find it.
3. **Which registers can be freely used** and which must be saved and restored.

> <!-- --- -->
> \*\*NOTE** <br>
> **Registers** are a very small number of ultra-fast, super-efficient storage locations built directly into the CPU chip itself; they are the fastest form of storage in a computer.
> <br>
> Accessing a register is much faster than accessing RAM, because they're on the CPU, which uses registers for all its immediate tasks:
> - Holding values it's working on right now
> - Performing calculations
> - Keeping track of addresses
> - etc.
>
> Registers are **fixed-size**. On a `64-bit` CPU, most general-purpose registers are `64 bits` (8 bytes) wide.
> <!-- --- -->

This agreement or **calling convention** is called the **Application Binary Interface (ABI)**. Without it, compilers for different languages/version could never work together.

The most common one for 64-bit Linux/macOS is the **`System V AMD64 ABI`**:

#### **1. Passing Arguments and Return Values**

- **Integer/Pointer Arguments:**<br>
  The first 6 arguments are passed through CPU registers (in the following order):
  - `rdi` (1st arg)
  - `rsi` (2nd arg)
  - `rdx` (3rd arg)
  - `rcx` (4th arg)
  - `r8` (5th arg)
  - `r9` (6th arg)
  - Any additional arguments are pushed to stack.
- **Return Value:**<br>
  The result of a function is passed back to the caller in the `rax` register (or `rdx:rax` for very large values).

#### **2. Caller-Saved vs. Callee-Saved Registers**

- **Caller-Saved (Volatile) Registers:**<br>
  (`rax`, `rcx`, `rdx`, `rsi`, `rdi`, `r8`, `r9`, `r10`, `r11`)
  - **Rule:** The callee is free to **clobber** (overwrite) these registers without saving them.
  - **Responsibility:** If the caller has a value in one of these registers that it needs to survive the function call, the caller must save it *before* setting up the arguments and calling the function. 

- **Callee-Saved (Non-Volatile) Registers:**<br>
  (`rbx`, `rbp`, `r12`, `r13`, `r14`, `r15`)
  - **Rule:** If the callee wants to use these registers, it must restore them to their original values before returning to the caller.
  - **Responsibility:** The callee typically saves them onto the stack at the very start (in the prologue) and restores them at the very end (in the epilogue).

#### **3. The Function Frames: Prologue, Body, Epilogue**

The life cycle of a function can be broken down into three distinct phases: the **Prologue**, the **Body**, and the **Epilogue**.

To illustrate, suppose the simple function:
``` c
int my_calc(int a, int b)
{
  int result = a + b;
  return result;
}
```
**Assembly (Simplified):**
##### **Prologue: Setting Up the Function's Workspace**

This is the function's "setup" ritual&mdash;once the function is called&mdash; to create the **stack frame**:
``` ass
push rbp
move rbp, rsp
sub rsp, N
```

Let the stack is set up for a function that has just been called. The `call` instruction has done two things: **1)** pushed the return address onto the stack, and **2)** jumped to the new function:
  
| Memory Address | Content (Value) | Description | Register Pointing Here |
| --- | --- | --- | --- |
| `0x7fffffffe110` | ... | Caller's local variables | `rbp` (Caller's frame) |
| `...` | ... | ... | |
| `0x7fffffffe108` | `0x400567` | **Return Address** (saved by `call`) | `rsp` |

- **Step 1 (`push rbp`):** Save the Caller's Frame Pointer.

  The `push` instruction does two things:
  1. It decrements the stack pointer (`rsp`).
  2. It then copies the value from the specified register (`rbp`) to the new memory location that `rsp` now points to.

  By pushing `rbp` onto the stack, we are saving the caller's frame pointer so we can restore the caller's stack when we're done.
  <br>
  This single action creates a linked list of stack frames, allowing each function to cleanly return to its caller's perfectly restored state.

  Now the stack looks like this:

| Memory Address | Content (Value) | Description | Register Pointing Here |
| --- | --- | --- | --- |
| `0x7fffffffe110` | ... | Caller's local variables | `rbp` (Caller's frame) |
| `...` | ... | ... | |
| `0x7fffffffe108` | `0x400567` | **Return Address** | |
| `0x7fffffffe100` | `0x7fffffffe110` | **Saved `rbp`** (Caller's frame pointer) | `rsp` |

- **Step 2 (`move rbp, rsp`):** Establish Our Own Frame Pointer.

  This copies the current stack pointer into the base pointer register (`rbp = rsp = 0x7fffffffe100`). This is the moment our new stack frame is officially created, with `rbp` acting as the anchor point for the *current* function's frame:

| Memory Address | Content (Value) | Description | Register Pointing Here |
| --- | --- | --- | --- |
| `0x7fffffffe110` | ... | Caller's local variables | |
| `...` | ... | ... | |
| `0x7fffffffe108` | `0x400567` | **Return Address** | |
| `0x7fffffffe100` | `0x7fffffffe110` | **Saved `rbp`** | `rsp`, `rbp` |

- **Step 3 (`sub rsp, N`):** Allocate Space for Local Variables.

  Moves the stack pointer down by `N` bytes to reserve space for local variables.
  The compiler calculates the total size needed for all local variables and adjusts `rsp` by that amount in one go.
  
  Let `N=16`. The stack's final form for the function's execution:

| Memory Address | Content (Value) | Description | Register Pointing Here |
| --- | --- | --- | --- |
| `0x7fffffffe110` | ... | Caller's local variables | |
| `...` | ... | ... | |
| `0x7fffffffe108` | `0x400567` | **Return Address** | |
| `0x7fffffffe100` | `0x7fffffffe110` | **Saved `rbp`** | `rbp` |
| `0x7fffffffe0f8` | ? | **Local Variable 1** | |
| `0x7fffffffe0f0` | ? | **Local Variable 2** | `rsp` |

> <!-- --- -->
> \*\*NOTE** <br>
> The compiler automatically **spills** the register-based arguments (`a` in `edi`, `b` in `esi`) onto the stack between the **Prologue** and **Body steps**.
> <!-- --- -->

##### **Body: Doing the Actual Work**

The function body uses the registers and the stack space that was just set up:
``` ass
mov    eax, DWORD PTR [rbp-4]   ; Load local var 'a'
add    eax, DWORD PTR [rbp-8]   ; Add local var 'b'
mov    DWORD PTR [rbp-12], eax  ; Store the result in 'result'
```
1. `mov eax, DWORD PTR [rbp-4]`<br>
   Loads the 4-byte value from `[rbp-4]` in memory into the `eax` register.
2. `add eax, DWORD PTR [rbp-8]`<br>
   Adds the 4-byte value from `[rbp-8]` in memory to the value in `eax`.
3. `mov DWORD PTR [rbp-12], eax`<br>
   Stores the 4-byte value from `eax` into memory at `[rbp-12]`.

The final state of the stack:

| Memory Address | Content (Value) | Description | Register Pointing Here |
| --- | --- | --- | --- |
| `0x7fffffffe110` | ... | Caller's local variables | |
| `...` | ... | ... | |
| `0x7fffffffe108` | `0x400567` | **Return Address** | |
| `0x7fffffffe100` | `0x7fffffffe110` | **Saved `rbp`** | `rbp` |
| `0x7fffffffe0fc` | `10` | **Local Var: `a`** | |
| `0x7fffffffe0f8` | `20` | **Local Var: `b`** | |
| `0x7fffffffe0f4` | `30` | **Local Var: `result`** | |
| `...` | ... | ... | |
| `0x7fffffffe0f0` | ? | (Unused space) | `rsp` |

> <!-- --- -->
> \*\*NOTE** <br>
> `DWORD PTR` is a **size directive** for the assembler.
> 
> **`PTR`**: Signifies the following expression is a memory address to be dereferenced.<br>
> **`DWORD`**: Stands for "Double Word", which represents 32 bits.
> <!-- --- -->

##### **Epilogue: Cleaning Up and Returning**

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

NEVER KILL YOURSELF