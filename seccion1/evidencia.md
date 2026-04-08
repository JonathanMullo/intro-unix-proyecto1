Research questions:
1. What is LUKS and what is its relationship to dm-crypt in the Linux kernel? Explain the role of the device mapper in the encryption process.
1. LUKS, dm-crypt, and the Device Mapper
LUKS: The LUKS standard handles passwords and disk header metadata, but it doesn't perform encryption itself. (GitLab, s.f.)
dm-crypt: The Linux kernel subsystem that does all the heavy lifting: it encrypts and decrypts data block by block mathematically. (dm-cryp, s.f.).

The Device Mapper: The Device Mapper acts as a bridge. It creates a virtual disk (e.g., /dev/mapper/cryptroot). When the system attempts to save a file to this disk, the Device Mapper intercepts the attempt, has dm-crypt encrypt it, and finally writes it to the physical disk. (dm-cryp, s.f.).
2. What is the difference between full disk encryption (FDE) and filesystem-level 
The fundamental difference lies in the layer at which they operate: FDE operates at the hardware level (block layer) and at the operating system level (file layer). (Data-at-rest encryption, s.f.).
FDE (e.g., LUKS)
•	Advantages: Encrypts absolutely everything (temporary files, swap partition, etc.); it is 100% transparent to applications. (Data-at-rest encryption, s.f.).
•	Limitations: It is all or nothing (when unlocked at boot, the entire operating system has access); it does not isolate data between different users on the same machine. (Data-at-rest encryption, s.f.).
File System (e.g., fscrypt)
•	Advantages: Granularity (allows each user's /home directory to be protected with different passwords); it is more efficient since it only encrypts specific files. (Data-at-rest encryption, s.f.).
•	Limitations: Metadata leakage (an attacker can see the folder hierarchy, file sizes, and number of files); it results from leaks in the swap partition. (Data-at-rest encryption, s.f.).
3. What is LVM and what are its three layers of abstraction (PV, VG, LV)? What advantages does LVM offer over traditional partitioning in a server environment?
LVM is a tool that abstracts physical disks to provide flexible virtual storage. (Documentation, s.f.).
Layers:
•	PV (Physical Volume): For example, the physical hard drive or the raw partition (in your case, the block encrypted by LUKS) (Documentation, s.f.).
•	VG (Volume Group): A kind of giant storage pool where you can group one or more PVs (Documentation, s.f.).
•	LV (Logical Volume): The pieces you carve out of this storage pool to use as traditional partitions (your root /, swap, and /home) (Documentation, s.f.).
Server advantages: It allows you to resize partitions on the fly (without shutting down the server), add multiple physical disks to a single volume, and create snapshots for live backups without stopping services. (Documentation, s.f.).
4. Explain what happens during the boot process of a system with LUKS+LVM: at what point is the decryption password requested and what role does GRUB play in this process?
1.	The boot process: The /boot partition was not encrypted. The firmware was able to execute GRUB. GRUB simply reads the kernel and the temporary filesystem (initramfs) from /boot and loads it into RAM (ArchWiki, s.f.).
2.	The prompt: The system boots from the initramfs loaded into RAM. It detects that the root partition is encrypted and pauses, prompting you to enter the password on the screen (ArchWiki, s.f.).
3.	The unlock: If the password is correct, cryptsetup unlocks the volume. At that moment, the LVM service starts, detects the VG of the decrypted disk, initializes your logical volumes (LVs), and the system transfers control to the actual operating system to continue the boot process. (ArchWiki, s.f.).
