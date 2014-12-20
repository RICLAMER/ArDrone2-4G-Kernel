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
under folder /usr/projkernek, apply the command: sudo wget https://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.32.9.tar.bz2

5 - After the sucess in download; 100%[======================================>] 64.374.628  1,04MB/s   in 65s
2014-12-20 04:33:24 (964 KB/s) - ‘linux-2.6.32.9.tar.bz2’ saved [64374628/64374628]
We need to descompress the content.  it can be with the command:  
  :~$ tar xjvf linux-2.6.32.9.tar.bz2

6 - Now, open the diziped folder and open the file Makefile
: cd ./usr/projkernel/linux-2.6.32.9$
to list all files, enter the command
:~$ ls
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





