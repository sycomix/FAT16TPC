# FAT16TPC
Paste the assembly binary into the MBR of a FAT-formatted flashdrive. It finds a a file named "BOOTIA32.BIN" in the root, loads it into memory, and executes it. Good as a Stage 1 of a multi-stage custom bootloader.

**How to use**

Assemble it with NASM and paste it into the MBR, right after the "FAT16   " entry (yes the three spaces are important). It will find a BOOTIA32.BIN file (if it exists) in the root directory of the FAT, and load it to address 10000h (segment 1000h), and then jump to it to execute it. FAT16TPC will only load the first 64k of the file if it is larger than that.

As as aside, FAT16TPC should assemble to 380 bytes, which will keep it clear of any of the partition entries at the tail of the MBR if you decide to do anything with those.

**Error codes produced**

No error code - The file was loaded successfully. I felt a confirmation was excessive; print one yourself.

EC0F - Error Code: No File. This error code indicates that BOOTIA32.BIN is missing from the root directory.

ECFD - Error Code: File is Directory or Volume ID. This error code indicates that it found BOOTIA32.BIN, but the name is used for a directory or Volume ID for FAT, and is unloadable.

ECF0 - Error Code: File is Zero sized. This error code indicates that BOOTIA32.BIN is an empty file, and cannot be loaded.

DExx - Disk Error: Disk read returned the xx code. Refer to (https://en.wikipedia.org/wiki/INT_13H) for disk status codes.

**About**

I wrote this a while ago to speed up my OS devving hobby; Windows (and Linux too) kept complaining about "unformatted media" whenever I used an unfamiliar filesystem or nuked the MBR of the USB drive. I hope it is useful to other people too.
