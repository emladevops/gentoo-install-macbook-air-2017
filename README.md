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

### **II/ Boot from the installation media**

***** Step 1: Power off your Mac


