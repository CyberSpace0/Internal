🐚 Simple Shell

A UNIX command line interpreter written in C.

📌 Project Overview

This project is a minimalist implementation of a UNIX shell.
It replicates the core behavior of /bin/sh using low-level system calls and without relying on external libraries beyond the standard C library.

The shell supports:

Interactive and non-interactive mode
Execution of commands with arguments
PATH resolution
Built-in commands (exit, env)
Proper error handling
Correct exit status propagation
Memory management (Valgrind clean)
📚 Learning Objectives

Through this project we practiced:

Process creation (fork)
Program execution (execve)
Process synchronization (wait)
Environment handling (environ)
PATH parsing
Dynamic memory management
System call error handling
UNIX process lifecycle
🛠 Compilation
gcc -Wall -Werror -Wextra -pedantic -std=gnu89 *.c -o hsh
🚀 Usage
Interactive Mode
$ ./hsh
$ ls
$ ls -l /tmp
$ env
$ exit
Non-Interactive Mode
echo "ls -l" | ./hsh
⚙️ Features Implemented (Holberton Requirements)
✅ 1. Basic Shell
Displays a prompt in interactive mode
Reads user input using getline
Executes commands using fork and execve
Handles EOF (Ctrl + D)
✅ 2. Handle Arguments

Supports commands with arguments:

ls -l /var

Tokenization is implemented using strtok.

✅ 3. Handle PATH

If the command does not contain / or ., the shell searches for it in the PATH environment variable.

Example:

ls

Internally searches:

/bin/ls
/usr/bin/ls
...

If not found → prints error without calling fork.

✅ 4. fork Must Not Be Called If Command Doesn't Exist

The shell validates command existence before forking:

if (cmd_path == NULL)
{
    fprintf(stderr, "./hsh: %d: %s: not found\n", line_num, argv[0]);
    return (127);
}

This satisfies Holberton’s strict requirement.

✅ 5. Built-in: exit

Usage:

exit
Exits the shell
Returns the last command exit status
No arguments required (per project specification)
✅ 6. Built-in: env

Usage:

env

Prints all environment variables using:

extern char **environ;
🧠 Execution Flow
START
  ↓
Display prompt (if interactive)
  ↓
Read input (getline)
  ↓
Remove newline
  ↓
Tokenize input
  ↓
Check built-ins (exit / env)
  ↓
Search in PATH (if needed)
  ↓
If command not found → print error (NO fork)
  ↓
fork()
  ↓
Child → execve()
Parent → wait()
  ↓
Return exit status
  ↓
Repeat
📂 Main Components
🔹 main()
Shell loop
Input parsing
Built-in handling
Status tracking
🔹 execute_command()
Validates executable path
Forks process
Calls execve
Waits for child
Returns exit status
🔹 find_in_path()
Parses PATH
Iterates directories
Constructs full path
Validates using stat
Returns full path or NULL
🔹 print_env()
Iterates through environ
Prints environment variables
❌ Error Handling

Handles:

Command not found
Fork failure
Execve failure
Empty input
EOF
Memory allocation errors

Error format:

./hsh: line_number: command: not found

Exit status 127 is returned when command is not found.

🧵 Memory Management
All dynamically allocated memory is freed
cmd_path freed after use
line freed before exit
No memory leaks (Valgrind clean)
🧪 Example
Valid Command
$ ls
main.c hsh README.md
Invalid Command
$ fakecmd
./hsh: 3: fakecmd: not found
🔐 System Calls Used
fork
execve
wait
write
access
stat
getline
malloc
free
🧾 Return Status Behavior
Returns the exit status of the last executed command.
Returns 127 if command not found.
Built-in exit returns last status.
🧩 Project Constraints (Respected)

✔ No use of system()
✔ No advanced shell features (pipes, redirections, etc.)
✔ No unnecessary forks
✔ Proper PATH handling
✔ No memory leaks
✔ Only allowed functions used

👨‍💻 Authors
AZZAM AL DUYULI && ABDULMALIK BIN AQEEL
