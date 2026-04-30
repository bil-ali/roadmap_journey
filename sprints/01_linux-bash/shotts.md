# ["The Linux Command Line" by William Shotts]
## (24/2/26 &ndash; 30/04/26)
## **Task:**

The task is to read relevant chapters from [The Linux Command Line by William Shotts](https://www.kea.nu/files/textbooks/humblesec/thelinuxcommandline.pdf).

## **Proof:**
### Ch. 1 WHAT IS THE SHELL?<br>(24/02/26)

`date`<br>
Displays current time and date.

`cal`<br>
Displays calender for current month.

`df`<br>
Displays current amount of free space on disk drives.

`free`<br>
Displays amount of free memory.

`exit` or `CTRL+D`<br>
End terminal session.


<hr>
<br>


### Ch. 2 NAVIGATION<br>(24/02/26&ndash;25/02/26)

`pwd`<br>
Print name of current working directory.

`ls`<br>
List directory contents.

`cd`<br>
Change working directory.

`.` refers to the working directory.<br>
`..` refers to the working directory's parent directory.

On Linux, filenames that begin with . are hidden, i.e, not listed by `ls`. (`ls -a` does list them though).

Filenames and commands in Linux are case sensitive.

`cd` Changes working directory to home directory.<br>
`cd -` Changes working directory to previous working directory.<br>
`cd ~user_name` Changes working directory to home directory of *user_name*.


<hr>
<br>


### Ch. 3 EXPLORING THE SYSTEM<br>(25/02/26&ndash;27/02/26)

`ls -l`<br>
Lists directory contents, but in long format output.

**Basic Command Structure:**
``` bash
command -options arguments
```

**Options:** e.g., `-l`<br>
**Long Options:** e.g., `--reverse`

`ls -lt --reverse`<br>
Lists directory contents, in long format, sorted by modification date, with the order of the sort reversed.

**Common `ls` Options**
| **Option** | **Long option** | **Description** |
| --- | --- | --- |
| `-a` | `--all` | List all files, even hidden ones |
| `-A` | `--almost-all` | Like -a except doesn't list . and .. |
| `-d` | `--directory` | In conjunction with `-l`, shows details about the directory istelf rather than its contents |
| `-F` | `--classify` | Append an indicator character to the end of each listed name (like / for directories) |
| `-h` | `--human-readable` | In long format listings, displat file sizes in human-readable format (rather than in bytes) |
| `-l` | | Display in long format |
| `-r` | `--reverse` | Display in reverse order |
| `-S` | | Sort by file size, largest first |
| `-t` | | Sort by modification time, newest first |

`file`<br>
Determine file type.

In Linux a file with filename *picture.jpg* doesn't necessarily have to be a JPEG file.

`less`<br>
View text file contents.<br>

| **Command** | **Action** |
| --- | --- |
| `PAGE UP` or `b` | Scroll back one page |
| `PAGE DOWN` or `Space` | Scroll forward one page |
| `↑` | Scroll up one line |
| `↓` | Scroll down one line |
| `G` | Move to the end of the text file |
| `1G` or `g` | Move to the beginning of the text file |
| `/characters` | Search forward to the next occurence of `characters` |
| `n` | Search for the next occurence of the previous search |
| `h` | Display  help screen |
| `q` | Quit `less` |

`clear`<br>
Clears screen content.

`reset`<br>
Re-initializes terminal into default state.


<hr>
<br>


### Ch. 4 MANIPULATING FILES AND DIRECTORIES<br>(27/02/26)

`cp`<br>
Copy files and directories.

`mv`<br>
Move/rename files and directories.

`mkdir`<br>
Create directories.

`rm`<br>
Remove files and directories.

`ln`<br>
Create hard (`ln`) and symbolic (`ln -s`) links

#### **Wildcards and Character Classes**

| **Wildcard** | **Meaning** |
| --- | --- |
| * | Matches any character |
| ? | Matches any single character |
| [*characters*] | Matches any character that is a memeber of the set *characters* |
| [!*characters*] | Matches any character that is not a member of the set *characters* |
| [[:*class*:]] | Matches any character that is a member of the specified *class* |

| **Character class** | **Meaning** |
| --- | --- |
| [:alnum:] | Matches any alphanumeric character |
| [:alpha:] | Matches any alphabetical character |
| [:digit:] | Matches any numeral |
| [:lower:] | Matches any lowercase letter |
| [:upper:] | Matches any uppercase letter |

<br>

Hard links can't reference directories. Symbolic links can.

If you write to a symbolic link, the referenced file is written to.<br>
If you delete a symbolic link, only the link is deleted, not the file. If the file is deleted before the link, the link continues to exist but point to nothing (*broken link*).

Only way to identify a hard link is by checking the inode is identical using `ls -li`.<br>
Symbolic links identified by leading `l` in the first field in `ls -l`.


<hr>
<br>


### Ch. 5 WORKING WITH COMMANDS<br>(1/03/26)

A command can be one of four things:
- An executable program
- A command built into the shell itself (*shell builtins*)
- A shell function
- An alias

`type`<br>
Tells us what type the command is (of the above 4).

`which`<br>
Display exact location of executable program which will be executed.

`help`<br>
Get help for shell builtins.<br>
OR `<command> --help` (e.g., `mkdir --help`)

`man`<br>
Display an executable program command's manual page.

Manual pages generally contain:
- A title (the page's name)
- A synopsis of the command's syntax
- A description of the command's purpose
- A listing and description of each of the command's options

Man Page Organization:
| **Section** | **Contents** |
| --- | --- |
| 1 | User Commands |
| 2 | Programming interfaces for kernel system calls |
| 3 | Programming interfaces to the C library |
| 4 | Special files such as device nodes and drivers |
| 5 | File formats |
| 6 | Games and amusements such as screen savers |
| 7 | Miscellaneous |
| 8 | System administration commands |

`apropos`<br>
Display a list of appropriate commands by searching the list of man pages.

`whatis`<br>
Display one-line manual page descriptions.

`info`<br>
Display a command's info entry.

Info manual is basically man, but structured like a tree where nodes are accessed via hyperlinks.

`alias`<br>
Create an alias for a command.<br>
(`alias *name*='*string*'`)

Aliases vanish when your shell session ends.


<hr>
<br>


### Ch. 6 REDIRECTION<br>(1/03/26&ndash;2/03/26)

**Redirecting Standard Output**<br>
`ls -l /usr/bin > ls-output.txt`<br>
To append, not overwrite:
`ls -l /usr/bin >> ls-output.txt`

**Redirecting Standard Error**<br>
`ls -l /usr/bin 2> ls-output.txt`

**Redirecting Standard Output and Standard Error At Once**<br>
`ls -l /usr/bin > ls-output.txt 2>&1`<br>
`ls -l /usr/bin &> ls-output.txt`<br>
`ls -l /usr/bin &>> ls-output.txt`<br>

If we want to throw away output, we redirect (`>` or `2>`) to `/dev/null`.

`cat`<br>
Reads one or more files and copies them to standard output.

`cat` with no argument reads from standard input (ended with `CTRL+d`).

**Redirecting Standard Input**<br>
`cat < lazy_dog.txt`

<br>

**Pipelines:** A shell feature that allows the standard output of one command to be piped into the standard input of another.
<br>
Pipe operator: `|`.

**Filters:** Commands used with pipelines to perform complex operations, somehow changing input before outputing them.<br>
e.g., `ls /bin /usr/bin | sort | less`

`sort`<br>
Sort lines of text.

`uniq`<br>
Omit or report (`uniq -d) consecutive repeated lines.

`wc`<br>
Print newline, word, and byte counts for each file.<br>
`wc -l` for only line count.

`grep`<br>
Print lines matching a pattern:<br>`grep pattern filename`<br>
`grep -i` to make grep case insensitive.<br>
`grep -v` to print only those lines that don't match the pattern.

`head`<br>
Output the first part of a file.

`tail`<br>
Output the last part of a file.

`tee`<br>
Read from standard input and write to both standard output and file(s).


<hr>
<br>


### Ch. 7 SEEING THE WORLD AS THE SHELL SEES IT<br>(2/03/26)

`echo`<br>
Display a line of text.

**Pathname Expansion**<br>
`echo /usr/*/share`

**Tilde Expansion**<br>
`echo ~`

**Arithmetic Expansion**<br>
`echo $((2+2))`

**Brace Expansion**
`echo Number_{1..5}`<br>
`mkdir {2007..2009}-{01..12}`

**Parameter Expansion**<br>
Expansion of variables.<br>
`echo $USER`

More useful in shell scripts than command line.

**Command Substitution**<br>
Using the output of a command as an expansion.<br>
`echo $(ls)`

For text in **double quotes**, all special characters used by shell (`$`,`\`, etc.) are treated literally.<br>
Pathname/tilde/brace expansions and word splitting suppressed, but not parameter/arithmetic expansions and command substitution.

**Single quotes** suppress *all* expansions.

If we want to quote a single character, instead, escape it with `\`.

**Control Codes** are also notated with `\`:
| **Escape sequence** | **Meaning** |
| --- | --- |
| \a | Bell (alert that causes the computer to beep) |
| \b | Backspace |
| \n | Newline; line feed on Unix-like systems |
| \r | Carriage return |
| \t | Tab |

`echo -e "\a"`<br>
`echo $'\a'`


<hr>
<br>


### Ch. 9 REDIRECTION<br>(05/03/26&ndash;06/03/26)

Unix-like OS are multi-user systems.

Three possible states of ownership:
- User
- Group
- Other

`id`<br>
Display user identity.

User accounts are defined in `/etc/passwd`, groups are defined in `/etc/group`.

| **Attribute** | **File type** (in `ls -l`) |
| --- | --- |
| - | A regular file |
| d | A directory |
| l | A symbolic link |
| c | A character special file |
| b | A block special file |

`chmod`<br>
Change a file's mode (permissions).

`chmod` supports two distinct ways of specifying mode changes:
- octal (base-8) representation<br>
    `chmod 600 foo.txt`<br>
    where 6 *(octal)* = 110 *(binary)* = rw-<br>
    0 *(octal)* = 000 *(binary)* = ---
- symbolic representation.<br>
    `chmod u+x,go=rx foo.txt`

`umask`<br>
Set the default file permissions.<br>
*(Uses octal notation)*

`su`<br>
Start a shell as another user.<br>
`su [-[l]] [user]`<br>
`su -c 'command'` for a single command.

`sudo`<br>
Execute a command as another user.

Unlike `su`, `sudo` doesn't require other user's password. Nor does it load up a whole new shell or environment.

`sudo` "trusts" you for several minutes unitl its timer runs out.

`chown`<br>
Change the owner and group owner of a file or directory.<br>
`chown [owner][:[group]] file...`

`chgrp`<br>
Change a file's group ownership. **(Obsolete)**

`passwd`<br>
Change a user's password.


<hr>
<br>


### Ch. 10 PROCESSES<br>(06/03/26)

Modern OS are multitasking, which is achieved via **processes**.

Like files, processes also have owners and user IDs, effective user IDs, etc.

`ps`<br>
Report a snapshot of current processes.<br>
`ps x`: Shows all processes, with process state, regardless of controlling terminal.<ps>
`ps aux`: Shows all processes belonging to every user.

`pstree`<br>
Outputs a tree-like process list, showing parent-child relationships between processes.

`top`<br>
Display dynamic view of tasks.

`jobs`<br>
List active jobs.

`bg`<br>
Place a job in the background.

To automatically place program in background at launch, run it as `program &`

`fg`<br>
Place a job in the foreground.<br>

To terminate a foreground process: `CTRL+C`<br>
To stop a foreground process without terminating: `CTRL+Z`

`kill`<br>
Send a signal to a process.<br>
`kill -signal PID...`<br>
*(signal is TERM by default)*

**Common Signals:**
| **Number** | **Name** | **Meaning** |
| --- | --- | --- |
| 1 | `HUP` | Hang up. |
| 2 | `INT` | Interrupt.<br> Same as `Ctrl+C` on terminal. |
| 9 | `KILL` | Kill.<br> Sent to kernel immediately, not to target process. Last resort after `TERM`. |
| 15 | `TERM` | Terminate.<br> Default of `kill` command. |
| 18 | `CONT` | Continue.<br> Restore process after a `STOP` or `TSTP`.<br> Sent by `bg` and `fg` commands. |
| 19 | `STOP` | Stop.<br> Not sent to target process, thus can't be ignored. |
| 20 | `TSTP` | Terminal stop.<br> Sent by `CTRL+Z` on terminal.<br> Is sent to target program, unlike `STOP`, thus may be ignored. |
| 3 | `QUIT` | Quit. |
| 11 | `SEGV` | Segmentation violation. (page fault) |
| 28 | `WINCH` | Window change. |

`killall`<br>
Sending signals to multiple processes matching a specified program or username.<br>
`killall [-u user] [-signal] name...`

**Shutting down the system:**
- `halt` or `shutdown -h`
- `poweroff`
- `reboot` or `shutdown -r`
- `shutdown`

`vmstat`<br>
Outputs a snapshot of system resource usage (memory, swap, disk I/O)

`xload`<br>
Graphical program showing system load over time.

`tload`<br>


<hr>
<br>


### Ch. 11 THE ENVIRONMENT<br>(07/03/26)

**The Environment:** Body of information maintained by shell during the shell session.

Environment stores:
- Environment Variables
- Shell Variables
- Aliases
- Shell Functions

`printenv`<br>
Print part or all of the environment variables.

`set`<br>
Shows both shell and environment variables.

`alias`<br>
Create an alias for a command.<br>
`alias` without any arguments lists all aliases in shell session.

`export`<br>
Export environment to subsequently executed programs.


<hr>
<br>


### Ch. 12 A GENTLE INTRODUCTION TO VI<br>(07/03/26&ndash;09/03/26)

Assuming `vi` to be aliased `vim`.

`vi` to start.
`:q` to quit. `:wq` *(or `ZZ`)* to save and quit. `:q!` to force quit.

`vi` starts in **command mode**.<br>
`i` to enter **insert mode**.<br>
`ESC` to return from **insert mode** to **command mode**.

`:w` to save changes.

#### Cursor Movement Keys
| **Key** | **Moves the cursor** |
| --- | --- |
| `l` or `→` | Right one character |
| `h` or `←` | Left one character |
| `j` or `↓` | Down one line |
| `k` or `↑` | Up one line |
| `0` | To the beginning of the current line |
| `^` | To the first non-whitespace character on the current line |
| `$` | To the end of the current line |
| `w` | To the beginning of the next word or punctuation character |
| `W` | To the beginning of the next word, ignoring punctuation characters |
| `b` | To the beginning of the previous word or punctuation character |
| `B` | To the beginning of the previous word, ignoring punctuation characters |
| `CTRL+F` or `PAGE DOWN` | Down one page |
| `CTRL+B` or `PAGE UP` | Up one page |
| `numberG` | To line *number* |
| `G` | To the last line of the file |
| `a` | To the end of current word or punctuation character, then enter **insert mode** |
| `A` | To the end of current line, then enter **insert mode** |

Most of these commands can be prefixed with numbers.<br>
**e.g.**, `5j` moves cursor down five lines.

#### Line Opening Keys
| **Command** | **Opens** |
| --- | --- |
| `o` | **Open** the line below the current line |
| `O` | **Open** the line above the current line |

`u` to undo a change.

Two types of deletion commands:
- `x`: Delete a character at cursor location.
- `d`: Always followed by a movement command to control size of deletion.

| **Command** | **Deletes** |
| --- | --- |
| `x` | The current character |
| `3x` | The current character and the next two characters |
| `dd` | The current line |
| `5dd` | The current line and the next four lines |
| `dW` | From the current cursor to the beginning of the next word |
| `d$` | From the current cursor location to the end of the current line |
| `d0` | From the current cursor location to the beginning of the line |
| `d^` | From the current cursor location to the first non-whitespace character in the line |
| `dG` | From the current line to the end of the file  |
| `d20G` | From the current line to the twentieth line of the file |

`d` command doesn't just delete, it cuts. Every `d` deletion is copied into paste buffer.

`p` to paste (below the current line).<br>
`P` to paste above the current line.

`y` to "yank" (copy) text.

#### Yanking Commands
| **Command** | **Copies** |
| --- | --- |
| `yy` | The current line |
| `5yy` | The current line and the next four lines |
| `yW` | From the current cursor position to the beginning of the next word |
| `y$` | From the current cursor location to the end of the current line |
| `y0` | From the current cursor location to the beginning of the line |
| `y^` | From the current cursor location to the first non-whitespace character in the line |
| `yG` | From the current line to the end of the file |
| `y20G` | From the current line to the twentieth line of the file |

`J` to join the line below to current line.

`f` to search within a line and move cursor to the next instance of specified character (**e.g.**, `fa`).<br>Iterated by `;`.

`/` to search for a word or phrase or regular expression.<br>Iterated by `n`.

#### Global Search-and-Replace

To change the word *Line* to *line* for the entire file:<br>
`:%s/Line/line/g`

**(`%` defines range, meaning from first line to last line. We could also have specified an exact range like `1,5`)**

`:%s/Line/line/gc`: Substitution with user confirmation every time.

#### Editing Multiple Files

`vi file1 file2 file3...`

`:bn`<br>
Switch to next file.

`:bp`<br>
Switch to previous file.

Can't switch if current file has unsaved changes. *(Can force with `:bn!`, `:bp!`)*

`:buffers`<br>
List files being edited.

`:buffer number`
Switch to file *number* from `:buffers`.

`:e file4`<br>
Add *file4* to current editing session.

`:r filename` inserts *filename* into current file below the current cursor position.


<hr>
<br>


### Ch. 14 PACKAGE MANAGEMENT<br>(28/03/26&ndash;29/03/26)

Packages are not compatible across distros.

| Packaging system | Distributions (partial listing) |
| --- | --- |
| Debian-style (`.deb`) | Debian, Ubuntu, Linux Mint, Raspbian |
| Red Hat-style (`.rpm`) | Fedora, CentOS, Red Hat Enterprise Linux, OpenSUSE |

Package management systems consist of:
- **Low-level tools** that handle tasks such as installing and removing package files.
- **High-level tools** that perform metadata searching and dependency resolution.

| Distributions | Low-level tools | High-level tools |
| --- | --- | --- |
| Debian-style | dpkg | apt-get, apt, aptitude |
| Fedora, Red Hat enterprise Linux, CentOS | rpm | yum, dnf |

**Finding a Package in a Repository:**<br>
`apt-get update; apt-cache search search_string`

**Installing a Package from a Repository:**<br>
`apt-get update; apt-get install package_name`

**Installing a Package from a Package File:**<br>
`dpkg -i package_file`<br>
*(no dependency resolution since this is using low-level tool)*

**Removing a Package:**<br>
`apt-get remove package_name`

**Updating Packages from a Repository:**<br>
`apt-get update; apt-get upgrade`

**Upgrading a Package from a Package File:**<br>
`dpkg -i package_file`<br>
*(same as installation)*

**Listing Installed Packages:**<br>
`dpkg -l`

**Determining Whether a Package Is Installed:**<br>
`dpkg -s package_name`

**Displaying Information About an Installed Package:**<br>
`apt-cache show package_name`

**Finding Which Package Installed a File:**<br>
`dpkg -S file_name`


<hr>
<br>


### Ch. 16 NETWORKING<br>(04/04/26)

`ping`<br>
Send an ICMP ECHO_REQUEST to network hosts.

`traceroute`<br>
Print the route packets trace to a network host.

`ip`<br>
Show/manipulate routing, devices, policy routing, and tunnels.

`netstat`<br>
Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.

`ftp`<br>
Internet file transfer program.

`lftp`<br>
Basically `ftp` with more features.

`wget`<br>
Non-interactive network downloader.

`ssh`<br>
OpenSSH SSH client (remote login program).

`scp`<br>
Use an SSH-encrypted tunnel to (secure) copy files across the network.<br>
`scp remote-sys:document.txt`

`sftp`<br>
A secure replacement for `ftp`, that&mdash;instead of transmitting everything in cleartext&mdash;uses an SSH encrypted tunnel.


<hr>
<br>


### Ch. 17 SEARCHING FOR FILES<br>(25/04/26&ndash;27/04/26)

`locate`<br>
Find files by name.

`find`<br>
Search for files in a directory hierarchy.<br>
`find ~ -type f -name "*.JPG" -size +1M | wc -l`

**`-type` Test:**
| File type | Description |
| --- | --- |
| `b` | Block special device file |
| `c` | Character special device file |
| `d` | Directory |
| `f` | Regular file |
| `l` | Symbolic link |

**`-size` Test:**
| Character | Unit |
| --- | --- |
| `b` | 512-byte blocks<br>*(This is the default)* |
| `c` | Bytes |
| `w` | 2-byte words |
| `k` | Kilobytes |
| `M` | Megabytes |
| `G` | Gigabytes |

**`find` Tests:**
| Test | Description |
| --- | --- |
| `-cmin n` | Match files or directories whose content/attributes were last modified `n` minutes ago. |
| `-cnewer file` | Match files or directories whose contents/attributes were last modified more recently than those of `file` |
| `-ctime n` | Match files/directories whose contents were last modified m*24 hours ago. |
| `-empty` | Match empty files and directories. |
| `-group name` | Match files/directories belonging to group/group ID `name` |
| `-iname pattern` | Same as `-name` test, but case-insenetive |
| `-inum n` | Match files with inode number `n` |
| `-mmin n` | Match files/directories whose contents were last modified |
| `-mtime n` | Match files/directories with inode number `d.` |
| `-name pattern` | Match files/directories with the specified wildcard `pattern`. |
| `-newer file` | Match files/directories whose contents were modified more recently than `file`. |
| `-nouser` | Match files/directories that don't belong to a valid user. |
| `-nogroup` | Match files/directoriies that don't belong to a valid group.|
| `-perm mode` | Match files or directories that have permissions set to the specified `mode` |
| `-samefile name` | Match files that share the same inode number as file `name` |
| `-size n` | Match files of size `n` |
| `-type c` | Match files of type `c` |
| `-user name` | Match files/directories belonging to user `name` |

Along with **Tests**, `find` also supports **Operators**.<br>*e.g.*:<br>
`find ~ \( -type f -not -perm 0600 \) -or \( -type d -not -perm 0700\)`

**`find` Logical Operators:**
| Operator | Description |
| --- | --- |
| `-and` | Match if the test on both sides of the operator are true.<br>*(Can be shortened to `-a`)*<br>*(Can also be left out as `-and` is implied by default)* |
| `-or` | Match if a test on either side of the operator is true.<br>*(Can be shortened to `-o`)* |
| `-not` | Match of the test following the operator is false.<br>*(Can be abbreviated with `!`)* |
| `()` | Group tests and operators together to form larger expressions |

`find` also allows **Actions** to be performed based on the search results.

**Predefined `find` Actions:**
| Action | Description |
| --- | --- |
| `delete` | Delete the currently matching file |
| `-ls` | Perform the equivalent of `ls -dils` on the matching file |
| `-print` | Output the full pathname of the matching file to standard output<br>*(This is the default* |
| `-quit` | Quit once a match has been made |

`find` also supports **User-Defined Actions**. This can be done using `-exec` or `-ok` actions.<br>
`find ~ -type f -name 'foo*' -exec rm '{}' '+'`<br>
`find ~ -type f -name 'foo*' -ok ls -l '{}' ';'`<br>
*(`-ok` prompts before every action)*

**Common `find` Options:**
| Option | Description |
| --- | --- |
| `-depth` | Direct `find` to process a directory's files before the directory itself. |
| `-maxdepth levels` | Set the max number of `levels` that `find` will descend intp a directory tree when performing tests and actions |
| `-mindepth levels` | Set the minimum number of `levels` that `find` will descend intp a directory tree when performing tests and actions |
| `-mount` | Direct `find` not to traverse directories that are mounted on other file systems |
| `-noleaf` | Direct `find` not to optimize its search based on the assumption that it's searching a Unix-like file system |


<hr>
<br>


### Ch. 18 ARCHIVING AND BACKUP<br>(28/04/26&ndas;30/04/26)

**Run-length Encoding:**
- `AAAAA` &rarr; `5A`
- `WWWWWWWWWWWWBWWWWWWWWWWWWBBBWWWWWWWWWWWWWWWWWWWWWWWWBWWWWWWWWWWWWWW` &rarr; `12W1B12W3B24W1B14W`

Two types of Compression Algorithms:
- Lossless
- Lossy

`gzip`<br>
Replaces original file with a compressed version of the original.

`gunzip`<br>
Uncompresses file.

`gzip` Options:
| Option | Long option | Description |
| --- | --- | --- |
| `-c` | `--stdout`<br>`--to-stdout` | Write output to standard output and keep the original files |
| `-d` | `--decompress`<br>`--uncompress` | Decompress. Causese `gzip` to act like `gunzip` |
| `-f` | `--force` | Force compression even if a compressed version of the original file already exists |
| `-h` | `--help` | Display usage information |
| `-l` | `--list` | List compression stats for each file compressed |
| `-r` | `--recursive` | If one or more arguments is a directory, recursively compress files contained in them |
| `-t` | `--test` | Test the integrity of a compressed file |
| `-v` | `--verbose` | Display verbose messages while compressing |
| `-number` | | Set amount of compression. *`number`* is an integer from 1 (fastest, least compression) to 9 (slowest, most compression)<br>*(The default is 6)* |

`zcat`<br>
Equivalent to `gunzip -c`

`bzip2`<br>
`gzip` with a stronger *(but slower)* compression algorithm.<br>
Has all the same options as `gzip`, except `-r`.

`bunzip2`<br>
Uncompresses *.bz2* files.

**`bziprecover`**: Program to recover damaged *.bz2* files.

<br>

`tar`<br>
Archives files.<br>
`tar mode[options] pathname...`

`tar` Modes:
| Mode | Description |
| --- | --- |
| `c` | Create an archive |
| `x` | Extract an archive |
| `r` | Append specified pathnames to the end of an archive |
| `t` | List archive contents |

`tar cf playground.tar playground`<br>
`tar tf playground.tar`<br>
`cd foo && tar xf ../playground.tar`

Files/directories extracted from *.tar* take on the ownership of the user extracting, rather than the original owner.

`tar` often used in conjunction with `find`.<br>
`find playground -name 'file-A' -exec tar rf playground.tar '{}' +`<br>
`find playground -name 'file-A' | tar cf - --files-from=- | gzip > playground.tgz`<br>
*(First `-` is stdout, second `-` is stdin)*

<br>

`zip`<br>
Both compresses and archives files/directories.<br>
`zip options zipfile file...`

`unzip`<br>
Uncompresses and extracts files.

`zip` and `unzip` are mainly for Windows compatibility. On Linux, `tar` and `gzip` are preferred for archiving and compression.

`rsync`<br>
Remote file and directory synchronization.<br>
`rsync options source destination`
