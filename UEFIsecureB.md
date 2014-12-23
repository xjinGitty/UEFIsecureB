[UEFI wiki](http://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface)
## history:
1. 1998: intel initiative boot
2. 2005: uefi V1
3. 2007: uefi v2.1
4. current uefi v2.4 from 2013

## advantages
1. boot from large disks (addressable 16M)
2. cpu arch : 16 bit to 32/64 bit and need the bootloader have the same bit long with the firmware (means 64bit UEFI just co-work with 64bit bootloader)
3. boot service -> running service (then the kernel take over
4. co-work both with MBR partition scheme and the GPT parition scheme (here simple introduction of the differences between MBR and GPT))
5. uefi support fat32 and fat16 or fat12 for removable device for ESPs 

## disk compatibility
1. linux to support GPT could be enabled by turn on option: CONFIG_EFI_PARTITION
2. BIOS via GPT:
	1. grub2 and linux (GPT-aware): then the protective MBR could used as the original MBR
	2. grub and linux: then BIOS-BOOP-PARTITION is needed to embed the seconde stage of the grub	
	BBP only valid when BIOS-GPT: 
		- GPT did not have that partition type
		- UEFI: no such embedding for the second stage: for grub2-efi is just and efi app file
3. UEFI via GPT:
	ESP will be create for the efi app file (512M will be ok)
4. UEFI via MBR:
	CSM mode is there for supporting this (CSM mode is basically provide the BIOS-like function)

## EFI features
### services:
1. bootservice and the runtime service
2. variable service : data stored as variable will be used via firmware, os, and the efi app

### applications:
* efi could be considered as have abilities as:
	1. run os
	2. independent load standalone efi applications (means pluggable system firmware that is not provided by manufactor)
		- all of the efi app should be kept in ESP and be loaded via firmware boot manager
		- or by other efi app


## ESP
1. has efi app
2. os kernels
3. GPT or MBR 

## BOOTing
1. compared with bios use the boot sector, uefi make boot manager as firmware and when pc power up, it check configuration and then load the bootloader
2. boot configuration is set of the variables: including variable that indicate the path to the os loader
3. efi firmware is easy to detect the os loader, and this auto detection relies on a standardized file path to the os loader
4. format of the file path should be like: ESP/BOOT/BOOT<MACHINE-TYPE-SHORT-NAME>.EFI, like /efi/BOOT/BOOTX64.efi

5. EFI boot from GTP: firmware will give user interface for the boot manager to select the os loader
6. some EFI firmware will directly fall back to BIOS when detect MBR partition scheme is used, to prevent uefi booting from ESP on MBR disk. This scheme is called UEFI-MBR

## Secure boot
[secureBootKeyChain](https://www.suse.com/communities/conversations/wp-content/uploads/2012/08/mok2.png)
[details secureB of suse](https://www.suse.com/communities/conversations/uefi-secure-boot-details/)

1. from uefi v2.2: just os loaders or drivers that has been signed via the acceptable digital signature could be loaded
2. enable secure boot: system will drop to 'setup' mode, ask for the pk to be writen, after this, enter the 'user' mode. then only the driver or the os loader that has been signed with the pk could be loaded.
3. KEK can be add to DB to allow other certificates to be used.
4. and there is 'custom' mode, could let user add additional pk in

5. Intel-based systems certified for Windows 8 must allow secure boot to enter custom mode or be disabled, but not on systems using the ARM architecture
6. minimal bootloader: shim is used to be signed with win PK, and then ...

## UEFI shell
1. launching method: some firmware vendor make it available as efi app under the ESP, like /ESP/SHELLX64.efi
2. (almost depend on the manufactor and the model of the system motherboard)
3. and some other deploy the combination keys to launch the shell
4. otherwise must compile it manually and then add (bcfg)

