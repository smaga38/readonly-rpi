# readonly-rpi
Read-only Root-FS with overlayfs for Raspian
Raspbian, the standard Debian-based distribution that runs on most Raspberry PIs, has one major problem when Rpi is used as a stand-alone system that will simply shut down and not shut down regularly: it starts ext4 as the root file system. Ext4 has a journal that can be easily damaged when power is lost during a write operation, and it is not particularly convenient or optimized for flash devices.

There is a small init script that can be run before init itself, which will mount the root file system read-only, and then mount the temporary file system located in ram on top of it using overlayfs. With this setting, all applications can simply read and write to their directories in the root file system, but all changes simply go to RAM, nothing is written to disk. This means that there will be no more write operations on the SD card, and therefore it should last much longer. Of course, this also means that all changes made between reboots are lost after reloading a raspberry or turning off the power. In headless environments, this is often the desired scenario when the Raspberry Pi is just to perform a simple task and the data should not be saved.

If, on the other hand, the data should be stored (say, you want to create a device that registers data), you can simply create an additional partition on the SD card or use an external USB flash drive or similar and mount it as read / write. would normally do through fstab. This script will only associate with the root partition, it does not affect all other mounts.

if you need a writable partition, consider using f2fs on it, which is optimized for flash memory and should be more resilient to power loss than ext4.
This script was created by Pascal Suter, modified by Tony McBirdley and me ... along with contributions from community members on these topics and some of our own changes and additions. A discussion about this can be read here: https://www.raspberrypi.org/forums/viewtopic.php?f=66&t=173063
