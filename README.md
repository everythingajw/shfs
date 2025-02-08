# shfs

A bad joke.

This is a (terrible) filesystem (questionably) implemented in Bash.

I was inspired to write this after watching [this video on filesystems](https://youtu.be/9MWeiuw8WHU).

## Features

- Slow!
- Inefficient
- Very limiting
- No directory structure
- No backward- nor forward-compatibility

I have not yet run any performance tests on this, but let's be real here. This will never be performant.

## How it works

File metadata is stored at the beginning of the disk.

An inode comprises of several fields:

- 2 byte unsigned inode number
- 2 byte unsigned permissions (read, write, execute for user, group, other)
- 4 byte unsigned user id
- 4 byte unsigned group id
- 2 byte unsigned file length
- 8 byte signed last modification time
- 256 byte null-terminated file name (255 "usable bytes")

All fields except for the file name are stored as big-endian integers. File names are just a sequence of null-terminated bytes.

Each file has a maximum size of 2048 bytes. Each file is allocated a 2048-byte block, regardless of file size.

If the first byte of the file name is zero, then that inode is not considered allocated.

