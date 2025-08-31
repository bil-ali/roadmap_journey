# [Pre-Linux OS Basics Sprint / Day 1 / OSTEP Prereq Knowledge]

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

### Floating point (IEEE-754)
IEEE-754 is a technical standard that defines how computers represent and handle floating-point numbers.

#### How Floats Are Stored: Sign + Exponent + Fraction

Think of scientific notation (e.g. 6.022 x 10²³), but in binary. A float is split into three parts:
- **Sign bit (1 bit):** `0` is positive, `1` if negative.
- **Exponent (8 bits for 32-bit float):** Represents the "power" (like `23` in `10²³`). It's stored with a bias to handle negative exponents. For 32-bit floats, this bias is `127`, meaning `Actual Exponent = Stored Exponent - 127`). Stored exponent range: `1 – 254`; corresponding to actual exponents: `-126 – 127`.
- **Fraction/Mantissa (23 bits for 32-bit float):** The significant digits of the number (like the `6.022` from `6.022 x 10²³`).

**Example:** Storing 0.15625 in 32-bit float

**Step 1) Convert to 0.15625 binary:**

To convert a decimal fraction to binary, multiply by 2 repeatedly abd take the integer parts (`0` or `1`) until the fraction becomes zero or until you have enough bits. For `0.15625`:
- 0.15625 x 2 = 0.3125 &rarr; **integer part 0**
- 0.3125 x 2 = 0.625 &rarr; **integer part 0**
- 0.625 x 2 = 1.25 &rarr; **integer part 1**
- 0.25 x 2 = 0.5 &rarr; **integer part 0**
- 0.5 x 2 = 1 &rarr; **integer part 1**
Reading the integer parts from top to bottom, we get `0.00101₂`. Thus, `0.15625 = 0.00101₂`.

**Step 2) Normalize to Scientific Notation**

In binary scientific notation, we express the number as 1.xxxx x 2<sup>exponent</sup>. For `0.00101₂`, we move the binary point three places to the right to get `1.01₂`. The is equivalent to multiplying by `2⁻³`. So:
- Value: `1.01₂ x 2⁻³`
- The leading 1 is implicit in the representation (not stored), so we focus on the fractional part "01".

**Step 3) Determine the Sign Bit**

Since `0.15625` is positive, the sign bit is `0`.

**Step 4) Calculate the Stored Exponent**

The actual exponent is `-3`. We add the bias (127) to get the stored exponent: `-3 + 127 = 124`

Now, convert 124 to binary (8 bits): 