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