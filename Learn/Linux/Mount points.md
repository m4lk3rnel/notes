- **mount points** are directories in a Unix-like filesystem where other filesystems are **attached** (or "**mounted**") so their contents become accessible as part of the overall directory tree.

- you have a USB drive and you want to access its files:
	-  `sudo mkdir /mnt/usb`
	-  `sudo mount /dev/sdb1 /mnt/usb`
	- even though it'll probably mount automatically to `/media`