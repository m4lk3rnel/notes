- https://www.youtube.com/watch?v=KQjRnuwb7is
### The x86 Boot Process
- UEFI (**U**nified **E**xtensible **F**irmware **I**nterface) -> bootloader  -> kernel -> initramfs -> PID 1 (e.g. systemd - [[Systemd Explained]])
	- the kernel contains the initramfs

- really cool: 
	- [Preboot Execution Environment](https://en.wikipedia.org/wiki/Preboot_Execution_Environment), booting process that uses a DHCP server or a TFTP server. A client-server-based protocol that allows computers to boot up using software that is downloaded from a network instead of a local disk.