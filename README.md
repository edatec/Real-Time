# Real-Time
How to add real-time patches to linux kernel

## install tools

```sh
sudo apt install git bc bison flex libssl-dev make libc6-dev libncurses5-dev
```

## Install the Toolchain for Kernel(Cross-Compiling)

### 32

```sh
sudo apt install crossbuild-essential-armhf
```

### 64

```sh
sudo apt install crossbuild-essential-arm64
```

## clone raspberry kernel

```sh
git clone --depth=1 https://github.com/raspberrypi/linux
```

## download patch

[Patch](https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.15/older/)

```sh
wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.15/patch-5.15.96-rt61.patch.gz
gunzip patch-5.15.96-rt61.patch.gz
cd linux
patch -p1 -i ../patch-5.15.96-rt61.patch
```

## compile


```sh
cd linux
KERNEL=kernel7l
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2711_defconfig
CONFIG_LOCALVERSION="-5.15.36-rt41-patched"
make -j6  ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage modules dtbs
```

```sh
sudo env PATH=$PATH make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=mnt/ext4 modules_install
sudo cp mnt/fat32/$KERNEL.img mnt/fat32/$KERNEL-backup.img
sudo cp arch/arm/boot/zImage mnt/fat32/$KERNEL.img
sudo cp arch/arm/boot/dts/*.dtb mnt/fat32/
sudo cp arch/arm/boot/dts/overlays/*.dtb* mnt/fat32/overlays/
sudo cp arch/arm/boot/dts/overlays/README mnt/fat32/overlays/
sudo umount mnt/fat32
sudo umount mnt/ext4
```

```sh
# add the following configuration to config.txt
kernel=kernel-myconfig.img
```