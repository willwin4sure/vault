One of the defining features of Unix and its derivatives is that **everything is a file (descriptor)**. There are three kinds of files: ordinary disk files, directories, and special files.

> [!idea]
> Everything (documents, directories, hard-drives, modems, keyboards, printers) just interacts using series of bytes. Let's have them share an interface, called a ==file==!
## Ordinary Disk Files

> Your... files... in Windows Explorer. They're just bytes of data, potentially structured.

==Ordinary disk files== contain any data a user places on it, such as symbolic or binary programs.

No structuring is expected at all, but it could exist, e.g. a text file consists of strings of characters with lines demarcated by newline characters (in some encoding). ^b0a8c7

## Directories

> Your folders in Window Explorer. These form a rooted tree, and you can identify files by path names. You can also symlink things.

==Directories== provide the mapping from the names of files to the files themselves. ^d3e3f5

A directory is an ordinary file except it cannot be written on by unprivileged programs: this keeps the system in control. 

One special directory is the ==root directory==, from which all other files can be traced through some path down the tree. This path defines a ==path name== for each file, e.g. `/alpha/beta/gamma` refers to a file `gamma` in a directory `beta` in a directory `alpha` in the root `/`. Any path name not starting with `/` specifies a relative path from the current directory. ^69a0e7

The same non-directory file can actually exist in multiple locations, a feature called ==linking==. The Unix system is unique in that all links have equal status. Directory entries for files are simply the file name plus a pointer to the information describing a file. So there is no ground truth location for where a file is. Files exist independently of directory entries, though in practice files count the number of references to them and disappear together with the last link to it. ^4b0f02

## Special Files

> Like MMIO, but with files. 

Each supported I/O device is associated with at least one ==special file==.

These behave a lot like memory-mapped I/O: special files are read and written just like ordinary disk files, but such requests actually activate the associated device. ^16e0b3

An entry for each special file resides in the directory `/dev`, e.g. to write to a magnetic tape, one may write onto the file `/dev/mt`. Special files exist for each communication line, each disk, each tape drive, and for physical main memory.

Why do this? Well, now file and device I/O are as similar as possible, so you can use the same syntax and mechanisms for both. Special files also get the same protection mechanisms that regular files do.

## Removable File Systems

> USB drives. Or how Ubuntu mounts your Windows filesystem.

Not all files have to live on the same device. Through the process of ==mounting==, you can cause references to an ordinary file on your device to actually refer to the root directory of an external file system. Essentially, this replaces a leaf of the file system tree with a whole new subtree.

After the mount, there is virtually no distinction between files on the removable volume and those in the permanent file system. The only exception is that no link may exist between one file system hierarchy and another. This is enforced to avoid the bookkeeping necessary to assure removal of these links when the removable volume is dismounted. ^9937dc

## Protection

> The things that appear on the left when you run `ls -la`. Sometimes you have to manipulate these with `chmod`.

The access control scheme involves associating with each file ten ==protection bits==. ^1afade

Each user of the system has a unique ID. When a file is created, it is marked with the ID of the creator, as well as the ten protection bits. Nine of them specify read, write, and execute permissions of three groups of users: the owner, users in the same group as the owner, and all other users. ^91459c

If the tenth bit is on, the system will temporarily change the user ID of the current user to that of the creator of the file *when the file is executed as a program*. This is called the `setuid` bit, and allows privileged programs to access files that normal users cannot (maybe only the program should modify a certain file).

There is also a ==super-user== which can bypass all constraints. One example is that there is a command that creates an empty directory owned by the super-user, but with the `setuid` bit on, so you can execute the file (but not do arbitrary things with super-user access).

## I/O Calls

> `with open("foo.txt", "r") as f:` in Python

The operating system provides syscalls to do file I/O. We provide some examples in an anonymous language. There is an *open* syscall:

```
filep = open(name, flag)
```

can be used to read or write to a file with path name `name` that already exists. The `flag` argument can indicate whether the file is being read, written, or updated. The returned value `filep` is a ==file descriptor==, a short integer used to refer to the file later in the program. ^ae2dde

To make a new file or completely override an old one, the OS provides a *create* syscall that does exactly that, also opening the file for writing (it also returns a file descriptor).

> [!remark]
> The file system maintains no locks visible to the user. These are neither necessary (not often faced with large, single-file databases maintained by multiple, independent processes) nor sufficient (both users could still edit a file if their editor, say, created a copy of the file being edited) to prevent interference between two users of the same file.

After opening a file, we can read or write with the following calls:

```
n = read(filep, buffer, count)
n = write(filep, buffer, count)
```

^0d303d

This transmits up to `count` bytes between the file and the buffer. The returned value `n` is the actual number of bytes transferred (e.g. would be 0 if trying to read from the end of a file).

Calls are sequential: a pointer is stored by the system, indicating the next byte to read/write, and subsequent calls will start from the location of the pointer.

To do direct-access I/O, you can just move the read or write pointer to where you want by

```
location = lseek(filep, offset, base)
```

The pointer associated with `filep` is moved `offset` bytes away from `base`, which could be the current location or the beginning/end of the file. 

Some undiscussed syscalls are closing files, getting the status of a file, changing the protection mode or owner of a file, creating a directory, linking to an existing file, or deleting a file.

---

**Next:** [[Implementation of the Unix File System]]





