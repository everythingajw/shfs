# shfs

A bad joke.

This is a (terrible) filesystem (questionably) implemented in Bash.

I was inspired to write this after watching [this video on filesystems](https://youtu.be/9MWeiuw8WHU).

## Features

- Slow!
- Inefficient
- Limited
- No directory structure
- No backward- nor forward-compatibility
- Questionable error handling
- Way too many I/O operations for every operation
- No atomicity
- No consistency checks
- Ability to store dates before the year 1901 and after the year 2446, [unlike ext4](https://docs.kernel.org/filesystems/ext4/dynamic.html#inode-timestamps) ([relevant kernel mailing list post](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/commit/?id=a4dad1ae24f850410c4e60f22823cba1289b8d52)).

I have not yet run any performance tests on this, but let's be real here. This will never be performant.

## How it works

Filesystem-wide metadata is stored at offset zero of the entire disk. This includes:

- 2 byte unsigned number of inodes
- 4 byte unsigned file block size

File metadata (each inode) is after the filesystem-wide metadata.

An inode comprises of several fields:

- 2 byte unsigned inode number
- 2 byte unsigned permissions (read, write, execute for user, group, other)
- 4 byte unsigned user id
- 4 byte unsigned group id
- 4 byte unsigned file length
- 35 byte last modification time
- 256 byte file name

All fields except for the file name are stored as big-endian integers. File names are just a sequence of null-terminated bytes.

Each file has a maximum size of 2048 bytes. Each file is allocated a 2048-byte block, regardless of file size.

If the first byte of the file name is /, then that inode is not considered allocated. File names cannot contain the null byte.

Execute permissions are stored, but are totally meaningless.

## License

Wow, you actually want to use this software? I'm legitimately shocked.

This project is licensed under the GNU GPLv3.
