When discussing the Unix file system, we mentioned that [[The Unix File System#^d3e3f5|directory entries associate names with the files themselves]]. What exactly do we mean by that?

## The I-List

The pointer to the file is an integer called the ==i-number== (index number) for the file. This is used as an index into a system table called the ==i-list==, which is a known part of the device. This returns an entry, called the file's ==i-node==. This contains a description of the file:

1. The user and group IDs of the [[The Unix File System#^91459c|owner]].
2. The [[The Unix File System#^1afade|protection bits]].
3. The physical disk or tape *addresses* for the file contents.
4. Size of the file.
5. Time of creation, last use, and last modification.
6. Number of [[The Unix File System#^4b0f02|links]] to the file, i.e. number of times it appears in a directory.
7. A code indicating if it is an ordinary file, a directory, or a special file.

The [[The Unix File System#^499500|open and create syscalls]] convert the given path name into an i-number by searching the directories. Once the file is open, its device, i-number, and read/write pointer are stored in a system table indexed by the returned file descriptor. This allows the i-node to be easily retrieved.

Creating/linking/deleting files involves allocating/updating i-nodes. In particular, the link count must be updated. If the link count ever drops to 0, any disk blocks in the file are freed and the i-node is de-allocated.

## Hierarchical Blocking for Files

How are [[The Unix File System#^b0a8c7|ordinary files]] actually stored on the disk? Well, the disk is divided into a number of 512-byte blocks, each logically addressed from 0 to the number that can fit on the device.

In the last section, we said that the i-node contains the physical disk addresses for the file contents. The i-node actually has space for 13 device addresses. Does this mean that we can only store $13 \cdot 512$ bytes of information per file?

> [!idea]
> Use a hierarchical model to allow for both fast access to small files and slower access to large files, while making everything space-efficient.

The first 10 device addresses in the i-node point directly to the first 10 blocks of the file. If the file is larger than 10 blocks, the 11th device addresses points to an indirect block that contains up to 128 addresses of additional blocks. The 12th device address points to a double-indirect block, and the 13th device address points to a triple-indirect block.

In total, this allows files to grow to as large as
$$
512\cdot\left(10 + 128 + 128^{2} + 128^{3}\right)=1,082,201,088
$$
bytes. Quite large for the 1970s! Of course, access through the higher levels of indirection takes more disk accesses, and therefore takes longer.

For [[The Unix File System#^16e0b3|special files]], the above does not apply: the last 12 device addresses are useless, while the first specifies an internal ==device name==, interpreted as a pair of integers representing a device type and a subdevice number. The ==device type== indicates which system routine deals with I/O on the device.

## Implementing Syscalls

Recall [[The Unix File System#^9937dc|mounting]], which takes two arguments: the name of an existing ordinary file and the name of a special file whose associated storage volume has the structure of an independent file system with its own directory hierarchy.

This is implemented by maintaining a system table that takes in the i-number and device name of the specified ordinary file and returns the device name of the indicated special file. Then, while a path name is being scanned during a call to *open* or *create* (say), we search this table for each i-number/device pair that appears. If a match is found, we just replace the i-number with the new root directory and the device name by the value found in the table.

To the user, reading and writing of files appears to be synchronous and unbuffered: immediately after a *read*, the data is available. 

The system will also make several optimizations, such as using main memory as a cache for disk blocks, lazily writing back (this minimizes the number of I/O calls made). Further, the system can recognize when a program has made accesses to sequential blocks of a file and asynchronously pre-read the next block.

A program that reads/writes in blocks of 512 bytes will have better performance than a program that operates on single bytes, but the gain is not that big.

## Disk Layout

Here is a possible disk layout for a simple file system.

* The first block is the ==boot block==, which contains the code to bootstrap the OS.
* The second block is the ==super block==, which contains information about the entire disk, such as the size of the file system, number of free blocks, size of i-list, etc.
* Then, there are several blocks that hold a bitmap of free blocks.
* Then, there is the i-list.
* Finally, all other blocks are ==file blocks== for actual data.

---

**Next:** [[Unix Processes and Images]]