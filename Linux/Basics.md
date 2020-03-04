### An Operation System

A Linux distribution is a Linux Kernel
plus software.

Each Distribution can have a different focus.

At the heart of the OS is the Linux Kernel.
Linux is the layer between applications
and hardware.

### Linux Distributions

- The kernel is the core.
- Linux Kernel + Apps=Distro

When you log into a Linux system , you interact with a shell program that runs programs in the same way that the window command prompt dies. The directories to search are stored in a shell variable , PATH.

System programs are stored in some standard places:

```
/bin
/usr/bin
/usr/local/bin

```

### Directories

The /home directory is itself a subdirectory of the root directory which sits at the top of the hierarchy and contains all the system's files in subdirectores.
The root dir normally includes bin for system programs (.binaries), /etc/ for system config files and /lib for system libraries.

### File and Directory Maintenance

You can change the permissions on a file or directory using the chmod system call.

```bash
#include <sys/stat.h>

int chmod(const char *path, mode_t mode)

```

An example to provide a user with permissions to execute looks like this: chmod 755 demo1.sh
r=4.
w=2.
x=1.

bluevalentine!2

A superuser can change the owner of a file using the chown system call.

```bash
#include <sys/types.h>
#include <unistd.h>

int chown(const char *path, uid_t owner, gid_t group)
```

### User information

All Linux Programs, with notable exception of init, are started by other programs or users. Users most often start programs from a shell that respondes to their commands. When a user logs in to a linux system, he/she has a username and password. Once these have been validated, user is presented with a shell.

Internally, a user has a unique user identifier known as a UID. Each program that Linux runs is run on behalf of a user and an assosciated UID.

### Basename

basename- strip directory and suffix from filenames.
dirname- strip last component from filename.



