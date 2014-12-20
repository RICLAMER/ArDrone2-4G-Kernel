ArDrone2-4G-Kernel
==================

This Project are in state of DEVELOPING. Where the focus is build a kernel compatible with USB dongle 3G or 4G.

First off all, we need a clean istalation of ubuntu. I'm runing the version "ubuntu-13.10-desktop-amd64", under my Windows 8 with Hyper-V.
Yes, i created a virtual Ubuntu in my computer.
A easy explanation how to install Ubuntu under windows 8 with Hyper-V can be read here -> http://www.howtogeek.com/120246/how-to-install-a-virtualized-copy-of-ubuntu-in-windows-8/
  
Next we need to install some dependence package in Ubuntu.
 Create a folder to contain all initial files.
 cd ./usr
 sudo mkdir projkernel
 
 Install SSH Server, to by possible connect using putty in ubuntu.
 sudo apt-get install openssh-server
 
1 - We need to isntall the ARM compiler. This is necessary becouse the processor of Ar Drone is a ARM.
:~$ sudo apt-get install  gcc-arm-linux-gnueabi

2 - Next gcc compiler, in my case, is already installed.
:~$ sudo apt-get install binutils-arm-linux-gnueabi

3 - We will need to compile the kernel with the same version kernel of Ar Drone 2.0.  To dyscover the Ardrone Kernel version, we need to connect in Ar Drone WiFI
and connect using telnet to shell. I'm using the Putty to connect.  The IP adress is 192.168.1.1, port 23. User and Password is not necessary.

Connected in Ar Drone in telnet, run this command: uname -a
In my case, i received the line: Linux uclibc 2.6.32.9-g980dab2 #1 PREEMPT Mon Sep 16 11:50:23 CEST 2013 armv7l GNU/Linux

4 - OK, now in ubuntu, lest make donwload of this version of Kernel 2.6.32-9.
under folder /usr/projkernek, apply the command: 

sudo wget https://devzone.parrot.com/repositories/entry/oss-ardrone2/trunk/linux.tar.gz?format=raw

Now Rename this file to a apropriate name

: sudo mv linux.tar.gz?format=raw linux.tar.gz

5 - We need to descompress the content.  it can be with the command:  

: sudo tar -xzvf linux.tar.gz

6 - Now, open the diziped folder and open the file Makefile
: cd ./usr/projkernel
to list all files, enter the command
: ls
Result -->
riclamer@HyperUbuntu:/usr/projkernel/linux-2.6.32.9$ ls
arch     crypto         fs       Kbuild       Makefile  REPORTING-BUGS  sound
block    Documentation  include  kernel       mm        samples         tools
COPYING  drivers        init     lib          net       scripts         usr
CREDITS  firmware       ipc      MAINTAINERS  README    security        virt

Now we need to edit the Makefile, let's do that with nano.  Enter the command
: sudo nano Makefile
Result fisrt lines -->
  GNU nano 2.2.6              File: Makefile

VERSION = 2
PATCHLEVEL = 6
SUBLEVEL = 32
EXTRAVERSION = .9
NAME = Man-Eating Seals of Antiquity

 We need to set the line EXTRAVERSION with the version that we get in the pass "3" with command uname -a.
 in my case tha change will be:
 
 GNU nano 2.2.6              File: Makefile

VERSION = 2
PATCHLEVEL = 6
SUBLEVEL = 32
EXTRAVERSION = .9-g980dab2
NAME = Man-Eating Seals of Antiquity

 Now we need to save the change.  Press CTRL + O folowwed with ENtER, then CTRL + X to exit.
 
7 - Let's configure the Kernel, to inform what processor will be used.
:~$ export ARCH=arm
:~$ export CROSS_COMPILE=arm-linux-gnueabi-

7 - Lets to save a copy of kerner, after apply make.

sudo cp -rp /usr/projkernel /usr/kernel-build

8 - Now we need to open the kernel.config file in folder /usr/projkernel to enable the drivers and modules.

: sudo nano make.config

Now, in the line 813, change to:
CONFIG_SCSI_NETLINK=y
line 875:
CONFIG_WLAN=y
line 888:
CONFIG_USB_USBNET=y
line 889:
CONFIG_WAN=y

 Now we need to save the change.  Press CTRL + O folowwed with ENtER, then CTRL + X to exit.
 
 9 - Now we need to make a copy of kernel.config in folder /usr/projkernel to arch/arm/config folder.
 
 : sudo cp -rp kernel.config /usr/projkernel/arch/arm/configs/ardrone_defconfig
 
 and make the .config file to be processed with make
 
 : sudo cp -rp kernel.config /usr/projkernel/.config
 
 10 - All right, now is the time to  MAKE the kernel. in folder /usr/projkernel/, apply this command:
 
 : sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-

now very question are displayerd to you confirm. in my teste, you can just set "y" to necessary drivers. Some drivers WiFI, are not necessary. 


---ERROR COMPILER----
If you received this error:

make[1]: *** No rule to make target `n/n', needed by `firmware/n.gen.o'.  Stop.
make: *** [firmware] Error 2

... you kernel not compiled. you need to cleam your build and compiler again, só apply this command:
: make clean
: make distclean

Now Run make again:

: sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-

Now be alert with this 2 option:

  "Include in-kernel firmware blobs in kernel binary (FIRMWARE_IN_KERNEL) [Y/n/?] (NEW) "
I selected no.

  "External firmware blobs to build into the kernel binary (EXTRA_FIRMWARE) [] (NEW) "
Just hit enter after this one. Continue normally on all other questions.









