# ["Hands-on Introduction to Linux Commands and Shell Scripting" by IBM]

## **Module 1**

### Hands-on Lab: Getting Started with the Linux Terminal

`pwd` command prints the path name to the present working directory.<br>
`ls` command lists the contents of a directory.<br>
`cd` command changes the current working directory.

| Symbol | Represents path to |
| --- | --- |
| `~` | home directory |
| `/` | root directory |
| `.` | present working directory |
| `..` | parent of present working directory |

### Hands-on Lab: Installing and working with text editors

`sudo` command enables access to "super-user" system administration tools.<br>
`apt` system administration command to update and install packages.

`vim` commands starts vim.<br>
`:help` to get help in Vim.
`:q` to quit Vim.
`i` to enter **Insert** mode.
`Esc` to exit **Insert** mode into **Command** mode.
`:w` to save your work.

## **Module 2**

### Hands-on Lab: Informational Commands

`whoami` to return your username.<br>
`uname` to print the kernel name.<br>
`id` to display the user and group id.<br>
`df` to print available disk space.<br>
`ps` to print available disk space.<br>
`top` to view a real-time table of processes.<br>
`echo` to print given text.<br>
`date` to display the current time and date.<br>
| Specifier | Explanation |
| --- | --- |
| `$d` | Display the day of the month (01 to 31) |
| `%h` | Displays the abbreviated month name (Jan to Dec) |
| `%m` | Displays the month of year (01 to 12) |
| `%Y` | Displays the four-digit year |
| `%T` | Displays the time in 24 hour formate as HH:MM:SS |
| `%H` | Displays the hour |

`man` to get the user manual for a command.<br>

### Hands-on Lab: Navigating and Managing Files and Directories

`mkdir` to create a new directory.
`touch` to create a new file.
`date -r` to displat the last modification timestamp of a specified file.
`find` to search for and locate files.
`rm` to remove a file.
`rm -r` to remove a directory.
`mv` to rename or move a file.
`cp` to copy a file.

### Hands-on Lab: Access Control Commands

`chmod` to modify permissions for a file.
| Option | Description |
| --- | --- |
| `r`, `w`, `x` | **Permissions**: read, write, and execute |
| `u`, `g`, `o` | **User categories**: user, group, and all others |
| `+`, `-` | **Operations**: grant and revoke |

### Hands-on Lab: Wrangling Text Files at the Command Line

`cat` prints entire file contents to standard output.
`more` and `less` print file contents page-by-page.
`head` prints the first N lines of a file.
`tail` prints the last N lines of a file.
`wc` finds the number of lines, words, and characters in a file.
`sort` and `uniq` sort lines and drop duplicates.
`grep` and `cut` extract lines and fields from a file.
`paste` merges text files.