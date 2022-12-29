# Raspberry Pi 4 setup #

## OS Options ##
The standard OS is 32bit (armhf) Raspian based off of Debian Bullseye 11. It is tiny and light, but Octave is at V6.2. There is also a 64bit build, which works well. Ubuntu 22.04 64bit (aarch64) is also available. Both support Neurodebian, but a custom PTB install require a 32bit OS (Mario only builds 32bit mex files), so Raspian 32bit is preferred for this. At the moment there are **no** 64bit builds of PTB for RPi, so for PTB we must build ourselves:

## Building on 64bit (work-in-progress)

see https://github.com/iandol/Psychtoolbox-3/tree/arm64

Dependencies:

```
sudo apt install -y freeglut3-dev libglfw3-dev libglu1-mesa-dev libxi-dev freenect \
  libpciaccess-dev libxxf86vm-dev libxcb-dri3-dev libxcb-present-dev \
  libxcomposite-dev libxml2-dev libasound2-dev liboctave-dev
```

To build Screen need to add `-I/usr/lib/aarch64-linux-gnu/glib-2.0/include/` to mex or:

```
sudo ln -s /usr/lib/aarch64-linux-gnu/glib-2.0/include/glibconfig.h /usr/include/glib-2.0/
```

To build/use GPIO need this package not available anymore in apt: https://github.com/WiringPi/WiringPi/releases

With most mex files rebuilt, performance is at least equivalent to 32bit builds!

## Installing PTB ##

See https://github.com/kleinerm/Psychtoolbox-3/blob/master/Psychtoolbox/PsychDocumentation/RaspberryPiSetup.m for the official up-to-date details

With 64bit mex files rebuild, use the same github repo, running `SetupPsychToolbox` etc.

### Neurodebian

You can install PTB via Neurodebian (supports Raspian and Ubuntu) which solves dependencies easily. 

Raspian:
```
wget -O- http://neuro.debian.net/lists/bullseye.cn-bj1.full | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list
sudo apt-key adv --recv-keys --keyserver hkps://keyserver.ubuntu.com 0xA5D32F012649A5A9
sudo apt-get update
```

Tsinghua Ubuntu 22.04:
```
wget -O- http://neuro.debian.net/lists/jammy.cn-bj1.full | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list
sudo apt-key adv --recv-keys --keyserver hkps://keyserver.ubuntu.com 0xA5D32F012649A5A9
sudo apt-get update
```

Then `sudo apt install octave-psychtoolbox-3` which will resolve all dependencies for you. However, this installs an ancient version of PTB. Recent version of PTB only have 32bit mex builds so cannot work with Ubuntu.  Stick to Raspian for the moment...

### Gamemode ###
This allows PTB's `Priority` command to work better on Linux. **Install from APT (from Bullseye at least)**, or for older systems from  https://github.com/FeralInteractive/gamemode like so:

```
sudo apt install meson libsystemd-dev pkg-config ninja-build git libdbus-1-dev libinih-dev
cd ~/Code
git clone https://github.com/FeralInteractive/gamemode.git
cd gamemode
git checkout 1.6.1 # omit to build the master branch
./bootstrap.sh
```

## Running PTB ##

In a terminal type: sudo raspi-config; Navigate to Advanced Options > Compositor > xcompmgr composition manager; Choose No; Reboot the Raspberry Pi.

```
sudo apt install -y liboctave-dev gamemode freeglut3
```

Download the [latest PTB as a ZIP file](https://github.com/kleinerm/Psychtoolbox-3/archive/master.zip) or use `git` and create a `~/Code/Psychtoolbox` folder for it. Create an `~/.octaverc` file with the following:

```
warning('off', 'Octave:shadowed-function');
graphics_toolkit('gnuplot');
more off;
setenv('LC_CTYPE','en_US.UTF-8');setenv('LC_ALL','en_US.UTF-8')
Screen('Preference','VisualDebugLevel',3);
```

First, it is important to run Octave from command-line via `octave --no-gui` and then `cd ~/Code/Psychtoolbox` and run `SetupPsychToolbox`. Then you can use the GUI as normal...

### libdc1394

Need a .22 linked to the current .25:

```
sudo ln -s /usr/lib/arm-linux-gnueabihf/libdc1394.so.25 /usr/lib/arm-linux-gnueabihf/libdc1394.so.22
```

## Latest MESA

You can update the GPU drivers. The easiest way is to use precompiled ones, see https://www.raspberrypi.org/forums/viewtopic.php?f=67&t=293361 and https://github.com/smartavionics/Cura/releases in particular.

```
LD_LIBRARY_PATH=/opt/mesa/lib/aarch64-linux-gnu octave --gui
```
Or set `LD_LIBRARY_PATH` in your `.zshrc` to use as the default...

You can build Mesa yourself from source: https://qengineering.eu/install-vulkan-on-raspberry-pi.html

## Problems

32-bit rendering was fixed in a recent PTB update, Mario says fonts work, the only thing broken ATM is HDMI audio.

## Interface to GPIO?

![](https://raw.githubusercontent.com/Gadgetoid/Pinout.xyz/master/resources/raspberry-pi-pinout.png)
https://pinout.xyz/

PTB has a demo: `RaspberryPiGPIODemo.m` -- this uses the WirinngPi library that uses a different pin number scheme, and I can't get the gpio command to work.

https://github.com/gnu-octave/octave-rpi-gpio

https://www.raspberrypi.org/documentation/usage/gpio/

http://abyz.me.uk/rpi/pigpio/

Octave has a pythonic python bridge and there are several libraries to call GPIO from Python. Or use pigpio and sockets. Octave has several packages that will be useful:

https://octave.sourceforge.io/instrument-control/index.html

## Older Config and Workarounds ##

There is currently a MESA bug, and a current workaround is to make an `/etc/X11/xorg.conf` file to overrride the bug: https://gitlab.freedesktop.org/mesa/mesa/-/issues/3601

```
Section "ServerFlags"
  Option "Debug" "None"
EndSection
```

You should edit `\boot\config.txt` to make sure it uses the real open-source drivers, and not fake kms one `dtoverlay=vc4-fkms-v3d`:

```
[pi4]
# Enable DRM VC4 V3D driver on top of the dispmanx display stack
dtoverlay=vc4-kms-v3d-pi4
max_framebuffers=2
gpu_mem=256

[all]
gpu_mem=256
```

## Backing up the SD card

Backing up can be dome simply using `dd`. You can do it live, but better is on a different machine.For live, frst insert a USB backup disk, then check the disk names `lsblk -p`:

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
sudo dd bs=4M if=/dev/mmcblk0 of=/media/cogpi/Clones/MyImage.img conv=fsync status=progress
```

Once completed, shrink the image using [PiShrink](https://github.com/Drewsif/PiShrink). This can be compressed:

```
sudo pishrink.sh -v -z -a -p MyImage.img
```

To restore it read https://www.pragmaticlinux.com/2020/12/how-to-clone-your-raspberry-pi-sd-card-in-linux/




