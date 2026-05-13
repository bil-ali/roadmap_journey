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
`:help` to get help in Vim.<br>
`:q` to quit Vim.<br>
`i` to enter **Insert** mode.<br>
`Esc` to exit **Insert** mode into **Command** mode.<br>
`:w` to save your work.<br>


<hr>
<br>


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

`mkdir` to create a new directory.<br>
`touch` to create a new file.<br>
`date -r` to displat the last modification timestamp of a specified file.<br>
`find` to search for and locate files.<br>
`rm` to remove a file.<br>
`rm -r` to remove a directory.<br>
`mv` to rename or move a file.<br>
`cp` to copy a file.

### Hands-on Lab: Access Control Commands

`chmod` to modify permissions for a file.
| Option | Description |
| --- | --- |
| `r`, `w`, `x` | **Permissions**: read, write, and execute |
| `u`, `g`, `o` | **User categories**: user, group, and all others |
| `+`, `-` | **Operations**: grant and revoke |

### Hands-on Lab: Wrangling Text Files at the Command Line

`cat` prints entire file contents to standard output.<br>
`more` and `less` print file contents page-by-page.<br>
`head` prints the first N lines of a file.<br>
`tail` prints the last N lines of a file.<br>
`wc` finds the number of lines, words, and characters in a file.<br>
`sort` sorts the lines of text in a file and displays the result.<br>
`uniq` prints a text file with any consecutive, repeated lines collapsed to a single line.<br>
`grep` and `cut` extract lines and fields from a file.<br>
`paste` merges text files.

### Reading (Optional): A Brief Introduction to Networking

`ping` command sends special 'echo request' packets to a host and waiting for a response from the host.

### Hands-on Lab: Working with Networking Commands

`hostname` command prints host name.<br>
`hostname -i` prints host IP address.<br>
`ip a` displays system network interfaces.
`ping` command tests network connection.<br>
`curl` and `wget` commands transfer data from URL(s).<br>
*(`wget` supports recursive file download)*

### Hands-on Lab: Archiving and Compressing Files

`tar` allows you to pack multiple files and directories into a single archive file.<br>
`zip` allows you to compress files.<br>
`unzip` allows you to extract files.<br>


<hr>
<br>


## **Module 3**

### Shell Scripting Basics

By convention, use `.sh` file extension for shell scripts.<br>
Shebang directive: `#!interpreter [optional-arg]

### A Brief Introduction to Shell Variables

`read` command takes input from standard input and assigns it to shell variable.

### Filters, Pipes, and Variables

**Filters**: Shell commands that take input from standard input, transform input data, and output to standard output.<br>
**Pipe (`command1 | command2`)** pipes the output of command1 to the input of command2.<br>
`set` command lists all shell variables.<br>
`unset var_name` deletes shell variable, *`var_name`*.<br>
`export var_name` extends shell variable *`var_name`* to an environment variable.<br>
`env` command lists all environment variables.<br>

###  Examples of Pipes

`tr` command replaces characters in input text.<br>
`tr [OPTIONS] [target characters] [replacement characters]`

### Useful Features of the Bash Shell

#### Metacharacters
`#`: precedes a comment.<br>
`;`: command separator.<br>
`*`: filename expansion wildcard.<br>
`?`: single character wildcard in filename expansion.

#### Quoting
`\`: escape metacharacter interpretation.<br>
`" "`: interpret literally, but evaluate metacharacter.<br>
`' '`: interpret literally.

#### I/O Redirection
`>`: Redirect output to file.<br>
`>>`: Append output to file.<br>
`2>`: Redirect standard error to file.<br>
`2>>`: Append standard error to file<br>
`<`: Redirect file contents to standard input.

#### Command Substitution
`$(command)`: replace the command with its output.<br>
``command``: replace the command with its output.

#### Batch Versus Concurrent Modes
`command1; command2`: Batch Mode &mdash; commands run sequentially.<br>
`command1 & command2`: Concurrent Mode &mdash; commands run in parallel.

#### Command Line Arguments
`./MyBashScript.sh arg1 arg2`

### Introduction to Advanced Bash Scripting

Basic `if-then-else` syntax:
```
if [ condition ]
then
    statement_block_1
else
    statement_block_2
fi
```

`$#` is a special variable that holds the number of command-line arguments read by a bash script.

Arithmetic is done using the notation, `$(()).<br>
`echo $((3+2))`

#### Logical Operators

`&&` is AND.<br>
`||` is OR.

`==` or `-eq`: is equal to.<br>
`!=` or `ne`: is not equal to.<br>
`>` or `-gt`: is greater than.<br>
`<` or `-lt`: is less than.<br>
`>=` or `-ge`: is greater than or equal to.<br>
`<=` or `-le`: is lesser than or equal to.


#### Arrays

Empty array creation:<br>
`declare -a empty_array`

Populated Array definition:<br>
`my_array=(1 2 "three" "four" 5)`

Appending an element to array:<br>
`my_array+=("six")`

Indexing array:<br>
`echo ${my_array[0]}`

Print all array elements:<br>
`echo ${my_array[@]}`

#### For Loops

```
for item in ${my_array[@]}; do
    echo $item
done
```
**OR**
```
for i in ${!my_array[@]}; do
    echo ${my_array[$i]}
done
```

```
for ((i=1; i<5; i++)); do
    echo $i
done
```
**OR**
```
for i in {1..4}; do
    echo $i
done
```

### Scheduling Jobs using Cron

**Cron** is a tool that runs scheduled jobs consisting of shell commands/scripts.<br>
Crontab files contain jobs and schedule data.<br>

`crontab -e` opens text editor to enable editing Crontab file.<br>
Job syntax: <br>`m h dom mon dow command`<br>
*(Example job:<br>
`30 15 * * 0 date >> sundays.txt`)*

`crontab -l` returns a list of all cron jobs and their schedules.

### Hands-on Lab: Scheduling Jobs using Crontab

`crontab -r` removes current crontab.