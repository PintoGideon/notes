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

### ls -al

1. -l gives you the longform output.
2. -a shows you hidden files and directories

_Note: a single dash tells linux that multiple flags coming along_

### ~

~ represents your home directory.

```bash
>> cd ~

```

### tail

```bash
>> tail ~/.bash_history

```

### Signals

A signal is a notification that you send to a program. It's up to
the program to understand what to do with that.

```bash
CTRL + C - SIGINT

CTRL +D - SIGQUIT


```

### SIGKILL

If you want a program to stop and stop now.

```bash
ps aux | grep vnc

kill -9 <process_id>
#or
kill -SIGKILL <process_id>

```

### Some basics of vim.

```bash
vi test.txt

```

1. click "i" for insert
2. click "esc" to switch between command mode and insert mode
3. type `:wq` to write and quit vim.

### Streams

If you want to direct the 'stdout' stream to a file.

```bash
echo "Testing streams" > stream.txt
cat stream.txt
"Testing streams"
```

```bash
ls -lsah 2 > /dev/null
```

### User groups and permissions

```bash
whoami

sudo su
# sudo switch user

>> sudo useradd username
>> sudo passwd userpassword
>> su username
>> whoami
>> username
>> sudo echo hi

"username" is not in the sudoers list

### username does not have sudo privileges so we will have to use groups

```

### Groups

Some groups have special privileges like "sudo"

```bash
sudo usermod -aG sudo username
su username

### We are adding "username" to the "sudo" group
```

You can create your own groups.

### Permissions

```
drwxrwxr-x


d - d stands for directory file
rwx (User)- read, write, execute permission for the user
rwx (Group)- read, write, execute permission for the group.
r-x (Everyone else) - read and executed
```

```bash
chown username:groupname foldername
chmod u=rw, g=rw, o=rw filename
#or
chmod 666 filename


# 4 - read permissions
# 2 - write permissions
# 1 - execute permissions
```

### ~/.bashrc

```bash

>> vi ~/.bashrc
>> source ~/.bashrc

```

### Processes

```bash
>> ps
```

### Exit codes

If a process finishes successfully, it will return an exit code 0.
If not, the process will return a non-zero code.

130: The program was ended with CTRL+C
137: The program was ended with a SIGKILL






