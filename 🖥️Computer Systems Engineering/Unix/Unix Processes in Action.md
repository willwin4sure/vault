Here is some Python code demonstrating some of the system calls from [[Unix Processes and Images|the last page]]. These use the Python `os` module.

> [!warning]
> Some of the commands (such as `os.fork()`) will only work on Unix-based systems like Linux or Mac, but *not* Windows. I couldn't find an easy drop-in replacement, so use Ubuntu/WSL if you are on a Windows machine.

Here is a basic script demonstrating the [[Unix Processes and Images#^13d5b7|forking]] of processes:

```python
import os

child_pid = os.fork()

if child_pid == 0:
	pid = os.getpid()
	print(f"I am Child process with PID {pid}.")

elif child_pid > 0:
	pid = os.getpid()
	print(f"I am Parent process with PID {pid}, and PID {child_pid} is my child.")

else:
	print("Child process creation failed...")
```

The parent knows the child's PID, but the child doesn't know the parent's PID. This is potentially for various safety concerns (probably don't want children randomly deleting/interfering with parents).

Parents can also [[Unix Processes and Images#^ff3deb|wait]] for the child process to terminate before resuming execution using the following line of code:

```python
os.waitpid(child_pid, 0)
```

Recall that processes can communicate to each other through [[Unix Processes and Images#^aa6a68|pipes]]. We can do this in Python using the following command:

```python
pipe_fd_read, pipe_fd_write = os.pipe()
```

This actually returns *two* file descriptors, one for reading and the other for writing. We can use them by calling `os.read()` and `os.write()`, which perform [[The Unix File System#^0d303d|reading and writing]] syscalls.

```python
import os

pipe_fd_read, pipe_fd_write = os.pipe()

child_pid = os.fork()

if child_pid == 0:
	# Child process, don't need to write
	os.close(pipe_fd_write)

	message = os.read(pipe_fd_read, 100)
	print(f"Received the message {message.decode()}")
	os.close(pipe_fd_read)

else:
	# Parent process, don't need to read
	os.close(pipe_fd_read)

	message = b"Hello, Pipe!"
	print(f"Sending the message {message.decode()}")
	os.write(pipe_fd_write, message)
	os.close(pipe_fd_write)
```

---

**Next:** [[The Unix Shell]]