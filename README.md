# How to install Gentoo Linux (Macbook Air, 2017 model) (i5/1.8GHz/8GB/128G or i5/1.8GHz/256G)
This document show you how to install Gentoo Linux on a Macbook Air 2017

Alright, let's go!
I'm gonna split this installation into many parts (follow all of them): 


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

### II/ Boot from the installation media**

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

### V/ Download the stage3 tarball (systemd)
