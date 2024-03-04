> Wraps around the kernel providing users with a way of communicating with the machine.

At its most basic level, the Unix shell is a command-line interpreter. Users type in lines of the form

```
command arg1 arg2 ... argn
```

Then, a file with the name `command` is sought; command may be a [[The Unix File System#^69a0e7|path name]] specifying any file in the system. If the file cannot be found, the system generally prefixes the string with `/usr/bin/`, which contains commands that are generally used. The sequence of directories to be searched for name resolution is specified by the `$PATH` variable.

Once a command is found, it is brought into memory and executed. When the command finishes, the shell resumes execution and indicates its readiness by outputting a prompt character. More details on how the shell actually works are in the next page on [[Implementation of the Unix Shell|its implementation]].

## Standard I/O

Generically, programs must [[The Unix File System#I/O Calls|open or create]] files to get file descriptors for. However, programs run by the shell start off with three open files with descriptors `0`, `1`, and `2`.

The file descriptor `0` is for `stdin`, `1` is for `stdout`, and `2` is for `stderr`.

A user, when running a command from the shell, can tell the program to use different files for these standard I/O streams. For example,

```bash
ls >there
```

creates a file named `there` and places the directory listing inside of it. `<here` is used for the input stream and `2> log` is used for the error stream.

## Filters

A sequence of commands separated by vertical bars causes the shell to execute all the commands simultaneously and [[Unix Processes and Images#Pipes|pipe]] the standard output of each command into the standard input of the next command in sequence. For example,

```bash
ls -l | grep .txt
```

will find all `.txt` files in the current directory. This procedure could be done more clumsily using multiple commands and reading/writing temporary files (followed by their removal).

A program which copies its standard input into its standard output (with processing) is called a ==filter==.

## Command Separators

You don't need to separate commands into separate lines; you can just separate them with semicolons:

```bash
ls ; vim hi.txt
```

will just execute them sequentially. 

## Multitasking

The shell doesn't actually need to wait for a command to finish executing before allowing to take in another one. You can tell the shell to do this by appending a `&` character at the end of your command, and it will be immediately ready to accept another one.

```bash
python3 fork_test.py
```

The *processid* of the command you just ran will be printed, allowing you to wait for its completion or terminate it. 

## Other Capabilities

Finally, the shell is itself a command, and may be called recursively by calling `sh`.

It also has other capabilities like substituting parameters or general conditional and looping constructs.

--- 

**Next:** [[Implementation of the Unix Shell]]