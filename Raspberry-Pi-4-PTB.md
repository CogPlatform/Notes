# Raspberry Pi 4 setup #

We are currently testing the RPi4 to check its viability for PTB useage.

## OS Options ##
The standard OS is 32bit Raspian based off of Debian Buster. It is tiny and light, but the major issue is getting a recent Octave installed. Ubuntu 20.10 is also available and has a more recent Octave available.

## Installing PTB ##
The best way to install PTB is to use Neurodebian (supports Raspian and Ubuntu). From USTC for Ubuntu 20.10:

```
get -O- http://neuro.debian.net/lists/groovy.cn-hf.full | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list
sudo apt-key adv --recv-keys --keyserver hkp://pool.sks-keyservers.net:80 0xA5D32F012649A5A9
sudo apt-get update
```

Tsinghua:

```
wget -O- http://neuro.debian.net/lists/groovy.cn-bj1.full | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list
sudo apt-key adv --recv-keys --keyserver hkp://pool.sks-keyservers.net:80 0xA5D32F012649A5A9
sudo apt-get update
```
The apt install `octave-psychtoolbox-3` which will resolve all dependencies for you. 

## Problems

Currently font enumeration is not working, and you must not use a 32bit buffer using `PsychImaging`.


## Backing up the SD card

Backing up can be dome simply using `dd`. First insert a USB backup disk, then check the disk names `lsblk -p`:

```
cogpi@cogpi-desktop:~$ lsblk -p
NAME             MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
/dev/loop0         7:0    0  48.8M  1 loop /snap/core18/1949
/dev/loop1         7:1    0  57.2M  1 loop /snap/core20/907
/dev/loop2         7:2    0  48.8M  1 loop /snap/core18/1888
/dev/loop3         7:3    0 214.1M  1 loop /snap/gnome-3-34-1804/61
/dev/loop4         7:4    0 197.7M  1 loop /snap/gnome-3-34-1804/68
/dev/loop5         7:5    0  64.8M  1 loop /snap/gtk-common-themes/1514
/dev/loop6         7:6    0  62.1M  1 loop /snap/gtk-common-themes/1506
/dev/loop7         7:7    0  48.3M  1 loop /snap/snap-store/499
/dev/loop8         7:8    0    27M  1 loop /snap/snapd/10709
/dev/loop9         7:9    0 108.5M  1 loop /snap/qv2ray/3910
/dev/loop10        7:10   0  26.9M  1 loop /snap/snapd/9730
/dev/loop11        7:11   0  47.3M  1 loop /snap/snap-store/476
/dev/sda           8:0    1 119.3G  0 disk 
├─/dev/sda1        8:1    1   256M  0 part /media/cogpi/boot
└─/dev/sda2        8:2    1   119G  0 part /media/cogpi/rootfs
/dev/sdb           8:16   0 931.5G  0 disk 
├─/dev/sdb1        8:17   0   489M  0 part /media/cogpi/CLONEZILLA
└─/dev/sdb2        8:18   0   931G  0 part /media/cogpi/Clones
/dev/mmcblk0     179:0    0  14.8G  0 disk 
├─/dev/mmcblk0p1 179:1    0   256M  0 part /boot/firmware
└─/dev/mmcblk0p2 179:2    0  14.6G  0 part /
```

`/dev/mmcblk0` is the booted SD card, `/dev/sdb2` mounted as `/media/cogpi/Clones` is our backup destination.

```
sudo dd bs=4M if=/dev/mmcblk0 of=/media/cogpi/Clones conv=fsync status=progress
```

Once completed, shrink the image using [PiShrink](https://github.com/Drewsif/PiShrink). This can be compressed. 

To restore it read https://www.pragmaticlinux.com/2020/12/how-to-clone-your-raspberry-pi-sd-card-in-linux/




