## SLED12 feature sets that I have ever did some investigation

---
### feature list:
*(features that I worked together with Ben)*
* KMS: concept, feature implementation, testing process 
* grub2: comparison with grub, and issues during test, bootloader overhaul (the compatibility)
* UEFI secure boot: simple introduction of UEFI and secureboot, current status (with the GPT disk format)/ MOK
* image-based installation

*(features that I worked together with Nick)*
* on-line migration: introduce the big change during the procedure (now is rejected, seems will be implement in sp1) 
* migration from SLE11 to SLE12 *(just DVD is supported)*

* KIWI to build the live installer: and to check that is the big chances


*(WeiHua feature: I did not help there, just did some investigation for interesting)*
* danymic X configuration

*(xudong)*
* rsyslog replace syslog and syslog-ng

*(yifan)* 
* dracut instead the mkinitrd and the initramfs (initrd)

---
### details for specific features:

#### on-line migration between SP
1. minimize the upgrade path
2. simple the upgrading steps during the procedure
3. try to make failed upgrade be recoverable
[on-line SPmigration](https://fate.suse.com/315161)

#### live installer
1. make yast could deploy the installed system(liveboot/ usb installed) to harddisk

#### danymic X
1. xset and xrandr and xmodmap is the usefull user interface 
2. xorg.conf did not exist anymore

#### OEM image install should be provided by new installer
1. the original KIWI OEMboot template is a little hardware specific
2. the new installer should first do the same thing with the OEMboot image, while with full feature set of the standard installer at hand

#### rsyslog and the syslog-ng (replaced)
1. better to have tool to convert the setting for syslog-ng to rsyslog
2. journal is a little persistence in the disk, by default "store is not on", and journal now just is the conjenctions (gathering) then forward to rsyslog
3. the final implementation: drop syslog, keep syslog-ng and write conversion tool to convert the syslog-ng to rsyslog, configuration for syslog-ng will be kept in the system
4. there is one thing that need to be investigated: rsyslog mainly did the log analysis? and the journal did not, it just record log info?

#### sle11 to sle12 upgrade
1. iprovement compared with sle10 to sle11
2. the steps
3. the packages first be downloaded and then deploy 

#### image based installation
1. tarball imaged type means here, under the /images dir with xx.tar.gz and the images.xml file in it
2. "deploying" proposal should be enabled in the control.xml file 
2. OEMboot type image is another scenario
3. UEFI / secure boot and the KMS; the new boot loader: grub2
