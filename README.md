# Compiling Linux kernel with Busybox
The repository contains the steps to compile the Linux kernel 5.3 with Busybox 1.29 using arm-linux-gnueabi compiler. 

## Install required packages
For Ubuntu, install the following package:
```console
mhdzr@ubuntu:~$ sudo apt-get install libncurses5-dev qemu-system-arm gcc-arm-linux-gnueabi
```
## Download files 
To get Linux kernel visit:
*  [Linux kernel](https://www.kernel.org/).
At the moment to write this turorial the stable version is 5.3.1.
For busy box visit: 
*  [Busybox](https://busybox.net/).
At the momento to write ths tutorial the stavle vesion is 1.29.3.

After get the files, unpack the content of both files in a common foler. For this tutorial the repository name is *Compiling-Linux-kernel-with-Busybox*. 

To ease next commands, add two variables in bash:

```console
mhdzr@ubuntu:~$ ROOT=~/Compiling-Linux-kernel-with-Busybox
mhdzr@ubuntu:~$ BUILDDIR=$ROOT/build 
```
## Create the make rule
Type the following command to enter Busybox directory and create the build folder:
```console
mhdzr@ubuntu:~$ cd $ROOT/busybox-1.29.3
mhdzr@ubuntu:~$ mkdir -p $BUILDDIR/obj/busybox-arm
```
The first step to build Busybox is create the make rule, to do this type: 
```console
mhdzr@ubuntu:~$ make 0=$BUILDDIR/obj/busybox-arm ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- defconfig
```

* `0=$BUILDDIR/obj/busybox-arm`. Specify the output dictrectory for the compilation. 
* `ARCH=arm`. Specify the architecture to generate the rule. If this parameter is omitted, the compilation will fail. 
* `CROSS_COMPILE=arm-linux-gnueabi-`. Specify the compiler to use. 
* `defconfig`. The configuration to use; for this case default. You can list de full list of defconfigs using `make ARCH=arm help | grep defconfig`. For more information about defconfig visit [What exactly does Linux kernel's make defconfig do?](https://stackoverflow.com/questions/41885015/what-exactly-does-linux-kernels-make-defconfig-do/41886394).

---
** NOTE: **
There are other ways to generate the compiler like [crosstool-ng](https://crosstool-ng.github.io/). To install it, follow the instruction to fill the [requirements](https://crosstool-ng.github.io/docs/os-setup/) and  follow the instruction to [build and install](https://crosstool-ng.github.io/docs/install/) crosstool-ng from source. When it is already installed, the tool provides you with a set of pre-configured toolchains that you can list using: 

```
mhdzr@ubuntu:~$ ct-ng list-samples
```
From the list, to set and generate a toolchain:

```
mhdzr@ubuntu:~$ ct-ng arm-unknown-linux-uclibcgnueabi
mhdzr@ubuntu:~$ ct-ng build
```
The problem with crosstool-ng is that you need to fill library dependancies and source code. You can get more info for [assambling a root filesystem](https://crosstool-ng.github.io/docs/toolchain-usage/). Once you have the toolchain the next steps are the same. 

---

## Enable static linking in Busybox

To avoid external dependancies with busybox compile Busybox as static linking. To so run the command: 
```
mhdzr@ubuntu:~$ make O=$BUILDDIR/obj/busybox-arm ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- menuconfig 
```

The command will start a menu. In the menu, select **Busybox Settings**->**Build Busybox as static library**. Press enter and (y). Select exit with direction keys twice. When prompt you to save configuration select yes. 

## Build Busybox
To start the compilation type the following commands: 

```
mhdzr@ubuntu:~$ $BUILDDIR/obj/busybox-arm
mhdzr@ubuntu:~$ make -j2 ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-
```
---
** NOTE: **

If you get the error: 

```
mhdzr@ubuntu:~$ busybox-1.29.3 is not clean, please run 'make mrproper'
```

Remove the .config files and run again the menu config command: 

```
mhdzr@ubuntu:~$ cd $ROOT
mhdzr@ubuntu:~$ find . -name "*.config"
./busybox-1.29.3/.config
./build/obj/busybox-arm/.config
mhdzr@ubuntu:~$ rm ./busybox-1.29.3/.config ./build/obj/busybox-arm/.config
mhdzr@ubuntu:~$ make O=$TOP/obj/busybox-arm ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- menuconfig
mhdzr@ubuntu:~$ make -j2 ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-
```



---



