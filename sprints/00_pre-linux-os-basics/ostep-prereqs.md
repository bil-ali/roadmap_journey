# [Pre-Linux OS Basics Sprint / Day 1 (1/9/2025) / OSTEP Prereq Knowledge]

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

> \*\*NOTE** <br>Converting this from binary to decimal: `1.01₂` = 1 x 2<sup>0</sup> + 0 x 2<sup>-1</sup> + 1 x 2<sup>-2</sup> = `1.25`

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
> \*\*NOTE** <br>
> A **`Memory Word`** is the basic unit of data a CPU can work with in a single operation. On a modern 64-bit system (typically 8 bytes).
>
> A **`Cache Line`** is a larger block of memoery (e.g., 64 bytes) that the CPU's cache loads from the main RAM all at once to be ready for super-fast access.

#### **Padding**
Extra bytes inserted by the compiler between fields in a `struct`/`class` to ensure each field is properly aligned.<br>
It can also be added at the end of a `struct` to ensure alignment in arrays.

#### **Example:** `struct X { char c; int i; };`
1. **Member Sizes:**
   - `char c`: 1 byte
   - `int i`: 4 bytes
2. **Without Padding:**
   - If the struct started at address `0x1000`:
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
  The linker's **main job**. It looks at the symbol tables from the `.o` files, and when is sees an undefined symbol, it searches through all the linked libraries and other `.o` files to find its definition.
- **Relocation**<br>
  Assigns final memory addresses to all the coe and data sections. It then goes back and updates all the preliminary addresses that the assembler created with these final addresses (using the Relocation Information from the **Symbol Table**).
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

<!-- --- -->
> \*\*NOTE** <br>The Python "Toolchain", by contrast, is a two-stage process.
> 1. Compilation to ByteCode
> 2. Interpretation by the Python Virtual Machine
> 
> <img src="img/0.svg" alt="SVG Image" width="40%"/>
<!-- --- -->

<br>

### Executable layout

An executable file is a highly structured toolbox.
This structure is divided into named **sections** (or **segments** when loaded into memory), each with a specific purpose and set of permissions (read, write, execute).

#### **The Common Sections/Segments**
1. `.text` (Code Segment)