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
