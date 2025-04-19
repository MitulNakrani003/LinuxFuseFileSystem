# FUSE File System Implementation

This project implements a read-write version of a Unix-like file system using the FUSE (File system in User SpacE) library. The implementation follows the specifications provided in the project description.

## Environment

This project is designed to run on an x86 Linux VM. All development and testing was done in this environment.

## Dependencies

Before running the project, you need to install the following packages:

```bash
sudo apt install check         # libcheck for testing
sudo apt install libfuse-dev   # FUSE development libraries and headers
sudo apt install python3       # Python 3
sudo apt install python-is-python3  # Makes python command point to python3
```

## Building the Project

To build the project, simply run:

```bash
make
```

This will compile the source files and create the necessary executables.

## Running Tests

There are two test suites for this project:

### Part 1 Tests (Read-only operations)

These tests verify the read-only operations of the file system:

```bash
./unittest-1
```

This will generate a predefined disk image named `test.img` and run tests for:
- `fs_init` - constructor
- `fs_getattr` - get attributes of a file/directory
- `fs_readdir` - enumerate entries in a directory
- `fs_read` - read data from a file
- `fs_statfs` - report file system statistics
- `fs_rename` - rename a file
- `fs_chmod` - change file permissions

### Part 2 Tests (Write operations)

These tests verify the write operations of the file system:

```bash
./unittest-2
```

This will generate a disk image named `test2.img` and run tests for:
- `fs_create` - create a new (empty) file
- `fs_mkdir` - create new (empty) directory
- `fs_unlink` - remove a file
- `fs_rmdir` - remove a directory
- `fs_truncate` - delete the contents of a file
- `fs_write` - write to a file
- `fs_utime` - change access and modification times

## Running as a FUSE File System

You can also mount the file system and interact with it directly:

```bash
mkdir mnt
./hw3fuse -image test.img mnt
```

This will mount the file system in the `mnt` directory. You can then navigate the file system using standard commands:

```bash
ls mnt
cd mnt
cat mnt/file.1k
```

When you're done, unmount the file system:

```bash
fusermount -u mnt
```

### Debugging with GDB

To debug with GDB, use the following command:

```bash
gdb --args ./hw3fuse -s -d -image test.img mnt
```

The `-s` flag specifies single-threaded mode, and `-d` specifies debug mode so it runs in the foreground.

## Notes

- Make sure to unmount the file system with `fusermount -u` if the program crashes.
- The test images are regenerated each time you run the tests to ensure a clean state.
- The file system is limited to 128MB in size and files cannot be larger than about 4MB due to the implementation.
