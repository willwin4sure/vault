An ==image== is a computer execution environment. This includes the memory image, general register values, status of open files, current directory, etc. This is the state of a pseudo-computer.

A ==process== is the execution of an image. While the processor is executing on behalf of a process, the image must reside in main memory.

The user-memory part of an image has three logical sections:

* **Program text segment**, beginning at location 0 in the virtual address space. This is write-protected and shared among all processes executing the same program.
* **Non-shared, writeable data segment**, beginning at the first hardware protection byte boundary above the program text segment.
* **Non-shared stack segment**, starting at the highest address in the virtual space and growing downward.

## Processes

Each process has a unique ==process ID== (PID) at any point in time (though these PIDs can be recycled). Except when the system is bootstrapping itself into operation, a new process can only be spawned through the ==*fork*== system call, e.g.

```
child_pid = fork()
```

^13d5b7

When this happens, the process branches into two independently executing processes. They will have independent *copies* of the original memory image, and share all open files. They only differ in the return value of `fork()`:

* in the parent, the returned value is the (nonzero) process ID of the spawned child
* in the child, the returned value is always `0`

This allows you to distinguish if you are the parent or the child.

## Pipes

==Pipes== allow for communication between two processes without the use of temporary files. These use the same interface of *read* and *write* system calls, e.g.

```
filep = pipe()
```

^aa6a68

creates a [[The Unix File System#^ae2dde|file descriptor]]. This, like other open files, is passed from parent to child by the *fork* call. Then, a *read* using a pipe file descriptor will wait until another process performs a *write* using the same pipe file descriptor, allowing data to pass. 

This is not a completely general solution, since pipes need to be set up by a common ancestor of the two communicating processes.

## Execution of Programs

This system primitive

```
execute(file, arg1, arg2, ... , argn)
```

will request the system to read and execute the program named by *file*, passing it the following arguments.

The current process gets reused: the code and data in the invoking process is replaced from the *file*, though open files, the current directory, and inter-process relationships are unaltered. Only if the call fails (e.g. if *file* is not found or the execute [[The Unix File System#^91459c|permission bit]] is not set) will a return occur from *execute*.

## Process Synchronization

Processes can also synchronize back up by waiting for their children to complete via

```
processid = wait(status)
```

^ff3deb

which causes the caller to suspend execution until some child process completes execution. This returns the *processid* of the child process, and can write in some *status* into the provided location (see the next section on termination). An error return is taken if the calling process has no children.

You can also be more precise in which child to wait for by specifying a process ID.

## Termination

The system call

```
exit(status)
```

terminates a process, destroys its image, closes its open files, and generally obliterates it. The parent process is notified through the *wait* primitive, and *status* is made available to it. 

Processes may also terminate as a result of various illegal actions or user-generated signals.

---

**Next:** [[Unix Processes in Action]]



