## Main Loop

Here is the sequence of steps the shell takes to process commands:

* Most of the time, the shell is waiting around.
* When the newline character is entered, the shell's *read* call returns.
* The shell analyzes the entered command and puts the arguments in a form ready for an *execute* call.
* *fork* is called, spinning off a child process.
* The child process, whose code is still that of the shell, runs *execute* and gets replaced by the program.
* The parent process *waits* for the child to die, and then prompts the user, *reading* the keyboard for another command.

## Other Details

[[The Unix Shell#Multitasking|Background processes]] can be implemented by simply not waiting for the child process to die.

This mechanism also works well with the [[The Unix Shell#Standard I/O|standard I/O streams]], since the processes created by the *fork* primitive inherit not only the memory image of the parent but also the files currently open, including those with the file descriptors 0, 1, and 2.

When an argument `<`, `>`, or `2>` is given to redirect there streams, however, the child process, just before performing *execute*, changes the standard I/O file descriptor to refer to the named file. 

[[The Unix Shell#Pipes and Filters|Filters]] are implemented with Unix [[Unix Processes and Images#Pipes|pipes]]. 

The main loop of the shell ordinarily never terminates, unless it receives and end-of-file on its input file. This allows you to run `sh <comfile` and return control to the child process to die, allowing the parent to stop *waiting*.

## Initialization

The shell itself is a child process of another process, spawned during system initialization. In fact, a program is invoked (via *execute*) called *init*, which creates one process per terminal channel. There is a user identification and password system to set the process's user ID before executing the shell.

The original process still *waits* for this process to die. If it does so, it prompts with a log-in sequence again. Therefore, logging out can be achieved by simply inputting an EOF.

---

**Next:** [[Unix Traps]]