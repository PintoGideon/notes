### Linux Essentials

### The Linux Boot Process (Simplified)

- After a computer starts, the BIOS on a motherboard checks up on the hardware of all the input and output devices. Once everything checks out, the boot process on a computer can begin. The boot program will look for a section in the hard-drive that contains the data needed to boot an operating system.
- This boot loader will then load the Linux kernel.
- The linux kernel will then load an inital RAM disk
- The RAM disk consists of device drivers which will mount the filesystem from the harddisk.
- After the kernel is setup and ready to go, it kicks of the Initialization system.

### GRUB 2

GPT (GUID Partition Table)

- Supports 128 partitions
- Each partition size up in the ZB range
- Needs UEFI (Unified Extensible Firmware Interface) to boot.

### Interacting with Boot Loader

### Working with the Kernel modules

- The core framework of the operating system
- Provides a way for the rest of the system to operate with the hardware, memory, networking and itself.
- Linux kernel is monolithic (handles all memory management and hardware device interactions)
- Extra functionality can be loaded and unloaded dynamically through kernel modules
- Ensures that the system will not need to be rebooted into a different kernel image for added functionality

- Many third party linux libraries are device drivers

```bash

uname - Display info about the currently running kernel
uname -m
uname -rm
uname -a (Gives you details about the Linux kernel)

```

```bash

lsmod - List modules

```

### Internet Protocol

IP ( Internet Protocol) - an address that is assigned to a machine so that other machines can communicate with each other.

Two major versions:

- IPv4
  A standard address of four octets containing numbers between 0 and 255 for each octet.

- IPv6
  replacement for IPv6 contains a 128 but hexadecimal number for addressing.

### Transmission Control Protocol

TCP (Transmission Control Protocol) - Method with which all transactions between IP address (IPv4 and IPv6) are communicated. This protocol provides a mechanism that transmits and verifies that data traffic arrives at its destination and can be assembled in the correct order.

### UDP

It is a stateless network connection. Data packets are sent to a destination without any verification that they were received.

### ICMP

Intended for networking equipment such as routers , network switches, firewalls and other devices to send error messages between themselves.

### Network Mask

Defines a logical network that indicates the start and end of a range of IP addresses.

1. Network Gateway- Destination where network traffic goes that has no other matching route or that is not intended for the local network.

2. Broadcast Address- An IP addresss that is used to broadcast messages to all hosts on a particular network

### Network Manager

The NetworkManager command line interface. This is the command line utility for configuring network devices and their connection settings.

This is short for 'device' which is the physical hardware such as a network interface card) that we use to connect to a network.

```bash
nmcli dev show

```

```bash
nmcli con show

```

This is short for 'connection' which contains the network configuration settings assignmed to a particular device. We assign our IP address and DNS settings to a connection.

```bash
nmcli con add con-name "backup" type ethernet ip4 192.168.122.75/24 gw4 192.168.22.1 ifname ens11 autoconnect
nmcli con show

```

There are two ways an interface can have an IP address either via DHCP or via statically assigned IP address.

```bash
nmcli con mod backup ipv4.dns "192.168.122.1"
nmcli -f ipv4.dns con show backup
```

The command below will show you all the configured addresses.

```bash
ip addr show

```

You can also view the routing table with the command below

```bash
ip route show
```

If you want to add a IP address to a specific interface we can use

```bash
ip addr add 192.168.122.76/24 dev ens11
ip addr show ens11
```

### Networking tools

### Testing Connectivity

ping command is used to test a system's ability to communicate with another network device by sending ICMP packets across the network.

```bash
ping
```

If our network settings aren't properly configured, we won't receive a response.

traceroute displays a list of the 'hops' a packet will traverse to get to a destination. Used to verify network routing and to look for breaks in network communication.

```bash
traceroute 192.168.122.1
```

netstat command is used to display network connections and their state on a system. Can also be used to view the routing table.

```bash
netstat
```

### The Basics of DNS resolution from Linux

Local configuration files that Linux uses to translate IP addresses into host names.

```bash
/*Text file that will contain the localhost entry mapped to the loopback addresses (Both ipv4 and ipv6). This file can also be used map other hostnames to IP addresses.
*/

cat /etc/hosts

/* Systems will use this file for a computer's hostname. The hostnamectl file will write a system's new hostname to this file
*/

ping -c 1 centos7
/*
64 bytes from centos7 (192.168.122.41)

*/
/etc/hostname
/etc/resolv.conf
/etc/nsswitch.conf
```

### Pseudo File System

- A regular file system is a method of laying out files and folder on a physical hard disk
- A pseudo file system does not exist on a physical hard disk. Only exists in RAM while system is running

-Two primary pseudo file system locations in Linux
a. /proc - Contains information about the processes running on a system.
b. /sys - Contains information about the systems hardware and kernel modules

```bash

cd /proc/
ls
cat cpuinfo

```

**_Finding detailed information about the various components of your computer_**

```bash
cd /sys/
ls
cat kernel
ls fs
ls fs/xfs
ls fs/xfs/xvda1
ls fs/xfs/xvda1/stats
cat fs/xfs/xvda1/stats/stats
```

### Main Filesystem Locations

Primary Locations that you must know

- / Bottom of the directory tree, the 'root'
- /var The variable location, log files and dynamic content
- /home The users home directory where personal files are stored
- /boot The boot directory where the Linux kernel and supporting files are stored
- /opt

### Swap space

Swap is temporary storage that acts like RAM

- When a percentage of RAM is full, the kernel will move less used data to swap
- Swap partition
- Swap file

### Partitions

Suppose you have a single hard disk (/dev/sda).
The /dev/sda1 indicates the first partition of the sda A single hard disk may be divided into different partitions of varying sizes. Each partition can have a specific function within a system.

A mount point takes a partition or disk and mounts it to a directory.
The mount will list out every partition and show existing mounts.

```bash
mount
```

The fdisk command can be used to list out partition information on the specified disk.

```bash
fdisk -l/dev/diskname
```

### Filesystem Hierarchy Standard

Where Computer data is stored on a storage device within a certain manner

- The data is organized and easily located
- The data can be saved in a persistent manner
- Data integrity is preserved
- The data can be quickly retrieved for a user in a later point in time

### In Practice

With unix type operating systems, there is only one root. It's just that UNIX file systems handle their naming very differently.

1. /bin- This directory contains executable programs that regular users on the system can run.

2. /boot- Files necessary to boot the system up. THe kernel resides in this directory

3. /dev- Locations from where all devices are referenced from. This includes sound drives, keyboards.

4. /etc- contains config files for system confifuration

### Legacy MBR Partitions

The lsblk command used to list out block devices (such as hard drives).

```bash
lsblk
```

To run our MBR partition we can use the fdisk.

```bash
fdisk /dev/sda
```

### GPT Partitions

We can determine our partition setup using the lsblk command. We can also use the fdisk command to see the blocks.

Steps to create a GPT partitions.

```bash

gdisk /dev/sda

```

Use the menu driven commands to create a partition.

For the location of the beginning of the sector we are going to use the default. We can specify the size of the last sector. For example for a 1gb disk, we can partition for 500mb.

The Partition IDs look a bit different under the GPT partitions. We can use the `p` key to look at our new partition table. We use the `w` key to write the changes to the disk.

We can use the `lsblk` commands to check our updated devices.

### How to create swap partitions.

Swap partitions are created when your system's ram is completely full. The kernel takes the older applications that have not been used for a while and places them in the swap space.

THe common scenario is to use a swap partition which means dedicated some space for swap.

Let's go ahead and create a swap partition

```bash
lsblk
gdisk /dev/sda

Press the 'n' key.
First Sector: default (Press Enter)
Last Sector: (Press enter and let gdisk allocated the remaining space)
Partition ID: 8200
Press the 'p' key to see the partition
```

To use this swap space, We need to put a filesystem here.
The mkswap command is used to format a partition to be used as swap space. To create a label on a file system, we use the -L command

```bash
mkswap -L SWAP /dev/sda2
```

The free command helps use to monitor swap space. Now we have to tell the system that it is okay to use the swap space

```bash
free
swapon -L SWAP
```

We run the free command again to check if the swap space in our system has increased.

```bash
free
```

As soon as reboot the computer, the swap space does not come on.

```bash
vim /etc/fstab
```

The fstab is what your computer used when it boots up to locate where all your filesystems are so it knows what applications to use for your home directories and many more.

```bash
/dev/sda2
```

### Creating Linux File Systems

1. Non-Journaling

- ext2- Legacy file system , released in 1993

2. Journaling

- Uses a journal to keep track of changes that have not yet been written to the file system
- ext3- released in 2001, introduced journaling to ext2
- ext4- released in 2006, added extra features meant to be a 'stop-gap' until a better solution comes along.
- XFS

### Making File systems

Let's create a file system. To create a file system on a partition that is ready to be used, we used mkfs which creates a new file system on a partition.
We can also add a label (using -L) to the filesystem.

```bash
mkfs -t ext4 or mkfs.ext4 -L SRV /dev/sda1

lsblk -f
```

### Understanding Mount Points

How mounting works?

On a typical Linux installation, we have the /opt directory on top of the local file system. Let's say that we need more the opt directory. So we setup a seperate disk with a partition on it and a filesystem setup on it.

We can mount it to our opt directory with the mount command.

```bash
mount /dev/sdb1 /opt
```

### Mounting and Unmounting File systems

When we run the mount command, we end up with every individual mount point existent on this system.

```bash
mount -t ext4
```

When we mount a filesystem, we specify a device and a directory to which this filesystem will get mounted to.

```bash
mount /dev/vdb1 /opt

```

### APT (Advanced Package Tool)

- Installs applications (and their dependencies)
- Removes applications
- Updates and upgrades packages

### Basics of how it works

- Reads the /etc/apt/sources.list
- The sources.list contains all the url listings for all software repositories that you can install software from your particular installation.
- Directs installation and uninstallation of packages to dkpg.

Let's see some of the components and commands we can use.

```bash
cat /etc/apt/sources.list
sudo apt-get update
Enter Password for root:

```

Now we have a local cache of all the available package that we can install on our hard-drive.

Let's upgrade packages:

```bash
sudo apt-get upgrade
```

We can also use the apt command to install applications.

```bash
sudo apt-get install chromium-browser
```

To remove an application,

```bash
sudo apt-get remove chromium-browser
sudo apt autoremove
```

In order to remove the config files as well,

```bash
sudo apt-get purge chromium-browser
```

To get the distibution up to the latest level, we can use

```bash
sudo apt-get dist-upgrade
```

The dist-upgrade will bring everything on the system up to date.
We can also download a package but not install it.

```bash
apt-get download htop
```

We can use the apt-cache search command to search through our local apt cache for a package that can be installed.

### The Debian Package Utility

The deb package contains

- application or utility
- default config files
- how and where to install the files that come with the package
- listing of dependencies the package requires.

Some of the commonly used dkpg commands.

```bash
// Displays information on a package

dpkg --info htop

```

```bash
// Lists out packages that match the string provided
dpkg -l nano

```

```bash
// Installs the package
sudo dpkg -i htop

```

We can also list out all the files that were installed with a specific package

```bash
dpkg -L

```

To remove a package

```bash
sudo dpkg -r htop
sudo dpkg -P htop
```

We can also search for a package to see if have the config files for it.

```bash
dpkg -S nano

```
