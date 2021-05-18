# How to install Gentoo Linux (Macbook Air, 2017 model) (i5/1.8GHz/8GB/128G or i5/1.8GHz/256G)
This document show you how to install Gentoo Linux on a Macbook Air 2017

Alright, let's go!
I'm gonna split this installation into 15 parts (follow all of them)


### **I/ Installation media preparation**
You need an USB with at least 500MB in order to create the installation media, download the iso at https://gentoo.org (grab the AMD64 edition)

#### If you're using Linux, do this

* Plug in the USB (of course backup your data first)
* Open terminal and run su - root (or sudo -i if you have sudo), then type in the password (su - root - enter root password; sudo -i - enter your password)
* Run lsblk and find the USB device name, in my case it's /dev/sdc
* Run dd if=<"ISO path"> of=<"USB device /dev path"> bs=1M

#### If you're using Windows, use Rufus to create the installation media
* Do it yourself, because Rufus have a GUI (but remember to choose disk type: GPT)

#### If you're using Mac, use BalenaEtcher to create the installation media
* Download balenaEtcher at https://etcher.io
* Locate the iso file at the source path
* Choose the USB you want to create the installation media
* Click on Flash
* Enter your password to begin flashing

### II/ Boot from the installation media

* Step 1: First of all, power off your Macbook
* Step 2: Press the Power button to power your Mac on
* Step 3: When you hear the boot chime, press and hold the Option key (Alt key)
* Step 4: Choose the EFI Boot partition (yellow ext disk logo)
* Step 5: Press enter twice to boot to Gentoo install media

### III/ Connect to network (If you're using Thunderbolt to Ethernet adapter)

* Step 1: Connect the adapter to your Mac (make sure the ethernet cable is connected)
* Step 2: Run command ``` ls /sys/class/net``` to find out if the Ethernet is connected
* Step 3: If it is recogized, run command ``` dhcpcd eth0 ``` to init the internet connection using DHCP

### III/ Connect to network (If you're using an Android Phone to share the Internet connection via the USB port)

* Step 1: Connect the Android Phone to your Mac using the USB Cable first
* Step 2: On your Android Phone, enable USB network sharing (for how to turn on, please search the internet)
* Step 3: Run command ``` ls /sys/class/net``` to find out if the USB internet connection is recognized
* Step 4: Run command ``` dhcpcd enp0s20u1u1```  to init the connection to Internet via DHCP

### IV/ Begin the installation
* Step 1: Run command `` fdisk /dev/sda`` then type `g` to create the new GPT partition table
* Step 2: Press `w` to write the changes to disk
* Step 3: Enter `cfdisk /dev/sda` to create the partition layout
* Step 4: Partition like this (if you'd like to follow me):


Disk type | Space
--------- | -----
EFI System and kernel | 500MB
Swap space | 4GB
System | amount of space left

* Step 5: Write the changes and exit cfdisk

### V/ Format the disk
For EFI System and Kernel, format it as FAT32 (type command `mkfs.vfat -F 32 /dev/sda<your EFI system and kernel partition>`
For the swap space, format it as the swap type (type command `mkswap /dev/sda<your swap partition>`)
For the system space, format it as ext4 (type command `mkfs.ext4 /dev/sda<your system partition>`)

### VI/ Download and unzip the stage3 tarball (systemd)
* Step 1: Mount the partition first, using mount `/dev/sda<your system partition> /mnt/gentoo`. After that, run `cd /mnt/gentoo` to get to the directory 
* Step 2: Type in command `links gentoo.org/downloads` to load the webpage 
* Step 3: Choose the latest stage3 tarball archive (Use systemd)
* Step 4: Unzip the tarball: `tar -xpvf sta(press <Tab> key) --numeric-owner --xattrs-include='*.*'`

### VII/ Changing setting for make.conf
* Step 1: Enter the file by `nano /etc/portage/make.conf`
* Step 2: Add `MAKEOPTS=-j3` to the bottom of the file
* Step 3: Add `-march=native` to the `COMMON_FLAGS` line
* Step 4: Write the change (Ctrl + O) and exit (Ctrl + X)

### IX/ Selecting mirror
* Step 1: Type in command `mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf`
* Step 2: Use the arrow keys to choose the mirror, Space bar to select the one you want
* Step 3: Press <TAB> key, then press <Enter> to write the changes to the make.conf file
  
### X/ Configuring Gentoo ebuild repo & copy the DNS info
* Step 1: Run command `cp -R /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf`
* Step 2: Run command `cp --dereference /etc/resolv.conf /mnt/gentoo/etc`

### XI/ Mounting the filesystem (fs)
* Step 1: Run command `mount --types proc /proc /mnt/gentoo/proc`
* Step 2: Run command `mount --rbind /sys /mnt/gentoo/sys`
* Step 3: Run command `mount --rbind /dev /mnt/gentoo/dev`
* Step 4: Run command `mount --make-rslave /mnt/gentoo/sys`
* Step 5: Run command `mount --make-rslave /mnt/gentoo/dev`

### XII/ Entering the gentoo environment
* Step 1: Run command `chroot /mnt/gentoo`
* Step 2: Run command `source /etc/profile`
* Step 3: Run command `export PS1="(chroot) ${PS1}"
* Step 4: Run command `mkdir /boot && mount /dev/sda<your EFI partition name> /boot`

### XIII/ Configuring portage
* Step 1: Run command `emerge-webrsync`
* Step 2: Run command `eselect profile list` to get a list of profile
* Step 3: Run command `eselect profile set <profile number>` to set the profile (remember to choose one with systemd at the end)
* Step 4: (This will take a long time) Run command `emerge --ask --verbose --update --deep --newuse @world` to update the @world set

### XIV/ Setting up timezone and locale
* Step 1: Type `ln -sf ../usr/share/zoneinfo/<your country> /etc/localtime`
* Step 2: Type `nano -w /etc/locale.gen`
* Step 3: Insert `en_US.UTF-8` to the file and save it
* Step 4: Run `locale-gen`
* Step 5: Run `eselect locale list`
* Step 6: Run `eselect locale set <the one that contain en_US.UTF-8>`
* Step 7: Reload the env: `env-update && source /etc/profile && export PS1="(chroot) ${PS1}"`

### XV/ Configuring the Linux Kernel
* Step 1: Run command `emerge --ask gentoo-sources`
* Step 2: Run command `emerge --ask genkernel`
* Step 3: Edit the fstab file (Run command `nano -w /etc/fstab`)
* Step 4: Insert the following line (if you use /dev/sda1 as the EFI and kernel system):
`/dev/sda1 <press Tab twice> /boot <press Tab twice> ext2 <press Tab twice> defaults <press Tab twice> 0 2`
* Step 5: Save and exit the file
* Step 6: Run command `genkernel --menuconfig all`
* Step 7: Get the following option: `Device Driver ->  Network device support -> Wireless LAN -> Intel PRO/Wireless 2100 Network Connection`
* Step 8: Press <Esc> until you get the save option, press Yes to finish the kernel
* Step 9: Wait...
  
### XVI/ Configuring the fstab file
Follow the instruction --> [here](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System#Creating_the_fstab_file) <--

### XVII/ Set the hostname and root password
* To set the hostname, run echo <your hostname> > /etc/hostname
* To set the root password, run passwd, then enter the new password twice

### XVIII/ Install the bootloader
* To install the bootloader, install grub first: `emerge --ask grub`
* Run command `grub-install --efi-directory=/boot`

### XIV/ Install the Wifi card driver (Thanks to [aesophor](https://github.com/aesophor) for this)
* To install the Wifi card driver, I'm gonna use the broadcom-sta driver
* Step 1: Run command `emerge -av net-wireless/broadcom-sta`
* Step 2: Run these comamnd
`depmod –a
rmmod b43
rmmod ssb
rmmod wl
modprobe wl`
* Step 3: Create the /etc/modprobe.d/blacklist.conf file containing:
`blacklist b43
blacklist sbb
`
### XV/ Unmount all volumes, boot the OS
* Step 1: Run command `exit`
* Step 2: Run command `cd`
* Step 3: Run command `umount -l /mnt/gentoo/dev{/shm,/pts,}`
* Step 4: Run command `umount -R /mnt/gentoo`
* Step 5: Run command `reboot`
* Step 6: Leave your computer alone to boot, do not hold the <Option> key
* Step 7: The moment of truth :)


