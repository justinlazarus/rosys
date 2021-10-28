# rosys
Rigid Operating System

## boot sequence (x86)

- CPU executing in real mode (addrs correspond to actual physical addrs)
- CPU looks at reset vector (`FFFFFF0h` for 32/64 bit) for firmware entry point
- Usually this location is a jump instruction that moves execution to location of firmware start-up program
- check and initialize required devices: memory, PCI bus, PCI devices
- firmware goes through list of non-volatile storage devices looking for bootable device
- bootable device is one that can be read from and where last 2 bytes of the first sector contain the little-endian word `AA55h` (MBR boot signature)
- once a bootable device is found, it loads boot sector to linear address `7C00h` and transfers execution to boot code.
- for a hard disk, this address is the master boot record - conventionally the code here checks the partition table for a partition set as *bootable* i.e. the one with the *active* flag set.
- if an active partition is found, mbr loads and executes boot sector code from that partition - this code is called volume boot record
- boot sector code is first-stage boot loader - must fit into first 446 bytes of the MBR to leave room for the 64 byte partition table with 4 partition entries + the 2 byte boot signature
- this configuration differs for different first-stage boot loaders
- vbr loads and executes operating system boot loader - this is the second-stage boot loader


### firmware (power-on self-test)

- detect available RAM
- pre-initialize CPU and hardware
- look for bootable disk
- boot operating system kernel

### bootstrap loader
