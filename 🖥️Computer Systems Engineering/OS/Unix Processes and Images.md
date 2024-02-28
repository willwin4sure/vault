An ==image== is a computer execution environment. This includes the memory image, general register values, status of open files, current directory, etc. This is the state of a pseudo-computer.

A ==process== is the execution of an image. Each process has a unique ==process ID== (PID) at any point in time (though these PIDs can be recycled).

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

The parent knows the child's PID, but the child doesn't know the parent's PID (perhaps for safety concerns).

```python
waitpid(child_pid, 0)
```

So how does the child communicate with the parent?

```python
pipe_fd_read, pipe_fd_write = os.pipe()
```

```python
def main():
	pipe_fd_read, pipe_fd_write = os.pipe()

	child_pid = os.fork()

	if child_pid == 0:
		message = os.read(pipe_fd_read, 100)
		print(f"Received the message {message.decode()}")
		os.close(pipe_fd_read)

	else:
		message = b"Hello, Pipe!"
		os.write(pipe_fd_write, message)
		os.close(pipe_fd_write)
```

Threads are created in processes, and all share the same virtual address space, unlike processes.