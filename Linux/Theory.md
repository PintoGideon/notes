A Linux system has three main levels.

The hardware is at the base. Hardware includes the memory
as well as one or more central processing units.
to perform computation and to read from and write to memory

Devices such as disks and network interfaces are also part of the hardware.

The next level up is the kernel which is the core of the OS. The kernel is software residing in memory that thells the CPU what to do. The kernel manages the hardware and acts primarily as an interface between the hardware and any running program.

Processes- THe running programs that the kernel manages collectively make up the system's upper level called user space.

1. Hardware- Processor, Main Memory, Disks, Network POrts
2. Kernel- System calls, process management, memory management, device drivers
3. User Processes- GUI, Servers, Shell.

There is a critical difference in the way the kernel and user processes run. The kernel runs in kernel mode and the user processes run in user mode.

### Hardware- Main Memory

Of all the hardware on a computer system, main memory is perhaps the most important. In it's raw form,
main memory is just a storage area for a bunch of 0s and 1s
This is where the running kernel and the processes reside. All inputs and outputs from peripheral devices flow through the main memory, also as a bunch of bits.

A CPU is just an operator on memory, it reads its instructions and data from the memory and writes data back out to the memory.

### The Kernel

Nearly everything that the kernel does revolves around the main memory. One of the kernel's tasks is to split memory into many subdivisions and it must maintain state information about these subdivisions at all times.

The kernel is in charge of managing tasks in four general system areas:

1. Processes: The kernel is responsible for determining which processes are allowed to use the CPU.

2. Memory: The kernel needs to keep track of all the memory- what is currently allocated to a particular process, what might be shared between processes and what is free.

3. Device Drivers: It is usually the kernel's job to operate on the hardware.

4. System calls and support- Processes usually use system calls to communicate with the kernel.

### Process Management

Process Management describes the starting, pausing , resuming and terminating of processes.

The concepts behind starting and terminating processes are fairly straightforward but describing how a process uses the CPU in it's normal course of operation is a bit more complex.

In a system with a one-core CPU, Many processes may be able to use the CPU, but only one process may actually use the CPU at any given time. In practice, each process use the CPU for a small fraction of a second, then another process uses the CPU for another small fraction of a second; then another process takes a turn and so on. The act of one process giving up control of the CPU to another process is called context switch.

### THe magical workflow

1. The CPU interrups the current process based on an internal timer, then switches into kernel mode and hands control back to the kernel

2. The kernel records the current state of the CPU memory, which will be essential to resuming the process that was just interrupted.

3. The kernel performs any tasks that might have come up during the preceeding time.

4. The kernel is now ready to let another process run. THe kernel analyzes the list of processes that are ready to run and chooses one.

5. The kernel prepare the memory for this new process.

6) The kernel tells the CPU how long the time slice for the new process will last.

7) The kernel switches the CPU into user mode and hands control of the CPU to the process.

### User

THe Linux kernel supports the traditional concept of a Unix user. A user is an entity that can run processes and own files. The user is assosciated with a username. For example, a system could have a user named **_gideonn_**.

User exist primarily to support permissions and boundaries. Every user-space process has a user owner and processes are said to run as the owner. A user may terminate or modify the behavior of it's own processes but it cannot interfere with other user's processes. In addition, users may own files and choose whether they share them with other users.

The most important user to know about is root. The root user is an exception to the preceeding rules because root may terminate and alter another user's processes and read any file on the local system.

### Basic commands and Directory Hierarchy.

**_Let's go_**

### The Bourne Shell: /bin/sh

The shell is a program that runs commands like the ones users enter. The shell also serves as a programming environment. Unix programmers often break common tasks into little components and use the shell to manage tasks and piece things together.

The anatomy of a command looks like this

```
$ commandname options inputs

```

1. cat

The cat command simply outputs the contents of one or more files. The general syntax of the cat command is as follows.

```
$ cat file1 file2

```

2. Standard input and Standard output

Unix processes use I/O streams to read and wirte data. Processes read data from input streams and write data to output streams. Streams are very flexible. When you do not specify an input to **_cat_**, it reads from the standard input stream provided by the Linux kernel rather than a stream connected to a file. In this case, the standard input was connected to the terminal in which you ran cat.

### Basic Commands.

**_ Gaining some confidence yeah !!!!! _**

1. ls

The `ls` command lists the contents of a irectory.

```
$ ls
```

2. touch

The `touch` command creates a file. You should see the output like the following

```
$ ls -l file

```

3. rm

To delete(remove) a file, use rm. After you remove a file, it's gone from your system and generally cannot be undeleted.

```
$ rm file

```

### Navigating Directories

UNIX has a directory hierarchy that starts at / sometimes called as the root directory. The directory
seperator is the slash (/).

There are several standard subdirectories in the root directory such as /usr.

```

$ cd dir

$ mkdir dir

$ rmdir dir

rm -rf dir
```

### Shell globbing

THe shell can match simple patterns to file and directory names, a process known as globbing.

This is similar to the concept of wildcards in other systems.

### The absolute essentials

The **_grep_** command prints the lines from the file or input stream that match an expression.

For example, to print the lines in the /etc/passwd file that contain the text root, enter this:

```
$ grep root etc/passwd

```

If you want to check every file in /etc that contains the word root, you could use

```
$ grep root /etc/*


```

2. less

The less command comes in a handy when a file is really big or when a command's output is long and scrolls off the top of the screen.

Send the standard output of nearly one program directl to another program's standard input.

Sending the output of grep command to less to view the output.

```
$ grep ie /usr/share/dict/words | less

```

3. pwd

The pwd program simply outputs the name of the current working directory. You may be wondering why you need this when most Linux distributions set up accounts with the current working directory in the prompt.

4. Find and locate

```
$ find dir -name file -print

```

5. Changing your password and shell

Use the `passwd` command to change your password. You'll be asked for your old password and hen prompted for your new password twice.

6. Environment and Shell variables

The shell can store temporary variales called shell variables. An environment variable is like a shell variable but it's not specific to the shell.

Environment variables are useful because many programs read them for configuration and options.

7. The command Path

PATH is a special environment variable that contains the command path. A command path is a list of system directories that the shell searches when trying to locate a command. For example when you run ls, the shell searches the directories listed in PATH for the ls program.

### Listing and Manipulating Processes

A process is a running program. Each process on the system has a numeric process ID. For a quick list of running processes, just run ps on the command line.

```
$ ps
```

### File modes and permissions

Running `ls -l` displays the permissions.

One such display could look like this

```
-rw-r--r--1 juser somegroup

```

The first character of the mode is the file type.
A dash (-) in this position denotes a regular file meaning that there is nothing special about the file.

```

r // Means that the file is readable

w // Means that the file is writable

x // Means that the file is executable

- // means nothing

```

### Modifying permissions

To change permissions, use the chmod command. First, pick the set of permissions that you want to change, and then pick the bit to change.

```
$ chmod g+r file
$ chmod o+r file

```

The above commands add group(g) and world (o for "other") read(r) permissions. In file, you could run these two commands.

### Linux Directory Hierarchy Essentials

The most important subdirectories in root:

- /bin contains ready-to-run programs including most of the basic Unix commands such as ls and cp. Most of the programs in /bin are in binary format, having been created by a C compiler , but some are shell scripts in modern systems.

- /dev contains device files.

- /etc This core system config directory contains the user password, boot, device, networking, and other setup files. Many files in /etc are specific to the machines hardware.

- /home Holds personal directories for regular users.

- /lib holds library files containing code that executables can use.

- /proc provides system statistics through a browsable directory and file-interface. Most of the /proc subdirectory structure on Linux is unique. The /proc directory contains info about currently running processes as well as some kernel parameters.

- /sys This directory is similar to /proc in that it provides a device and system interface.

- /usr A storage area for smaller , temp files that you don't care much about. Any user may read to and write from /tmp.

- /usr It contains a large directory hierarchy including the bulk of the Linux system.

- /var The variable subdirectory where programs record runtime info.

### THe /usr Directory

THe /usr directory may look relatively clean at first glance but a quick look at /usr/bin and /usr/lib reveals that there's a lot here. /usr is where most of the user-space programs and data reside. In addition to /usr/bin, /usr/lib ,/usr contains:

- /include Holds header files used by C compiler
- /info Contains GNU info manuals
- /local is where admins can install their own software.
- /man contains manual pages
- /share contains files that work on other kinds of UNix machines with no loss of functionality.

### Kernel Location

On Linux, the kernel is normally in /vmlinuz or /boot/vmlinux. The boot loader loads this file in memory and sets it in motion when the system boots.

It is easy to manipulate most devices on a UNIX system because the kernel presents many of the device I/O interfaces to user processes as files. These device files are sometimes called device nodes.

Device files are in the /dev directory and running `ls /dev`
reveals more than a few files in /dev.

Most hard disks attached to current Linux systems correspond to device names with an sd prefix such as /dev/sda, /dev/sdb and so on. These devices represent entire disks ; the kernel makes seperate device files, such as /dev/sda1 and /dev/sda2 for the partitions on a disk.
