# Compiling-Linux-kernel-with-busy-box
The repository contains the steps to compile the Linux kernel 5.3 with Busybox 1.29 using arm-linux-gnueabi compiler. 

## Install required packages
For Ubuntu, install the following package:
```console
mhdzr@ubuntu:~$ sudo apt-get install libncurses5-dev qemu-system-arm gcc-arm-linux-gnueabi
```
## Download files 
To get Linux kernel visit:
* https://www.kernel.org/.
At the moment to write this turorial the stable version is 5.3.1.
For busy box visit: 
* https://busybox.net/.
At the momento to write ths tutorial the stavle vesion is 1.29.3.

After get the files, unpack the content of both files in a common foler. For this tutorial the repository name is *Compiling-Linux-kernel-with-Busybox*. 

To ease next commands, add two variables in the bash:

```console
mhdzr@ubuntu:~$ ROOT=~/Compiling-Linux-kernal-with-Busybox
mhdzr@ubuntu:~$ BUILDDIR=$ROOT/build 
```


