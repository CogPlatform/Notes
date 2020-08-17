# Software + Hardware Notes
Various notes on setting up our software + hardware environment. In general, all software development should take place under Ubuntu Linux, the default OS for our workstations.

# Setting up Ubuntu 20.04
For Ubuntu, we are using the LTS version of Ubuntu as the default. We already have a fresh 20.04 install cloned and available on the NAS, you can use Clonezilla to restore. For new installs the first thing to do is install Chinese text entry and language support: https://www.pinyinjoe.com/linux/ubuntu-18-gnome-chinese-setup.htm — next you need to install the minimum number of tools for development. I always install at least: `sudo apt-get install build-essential vim curl file zsh git figlet jq mesa-utils libusb-1.0.0 freeglut3 libraw1394-11`. Once you have installed MATLAB, you can then try 

```sudo apt-get install matlab-support``` 

for MATLAB + PTB compatibility.

# Installing Github repos
```
mkdir -p ~/Code
cd ~/Code
git clone --depth 1 https://github.com/iandol/Psychtoolbox-3.git
git clone https://github.com/iandol/opticka.git
git clone https://github.com/CogPlatform/Training.git
git clone https://github.com/CogPlatform/Titta.git
git clone https://github.com/CogPlatform/Mymou.git
```

## Major Software to Install:
1. MATLAB — latest version kept up-to-date.
1. PTB — use my custom fork and and install it using Git; then in MATLAB, `cd` to the install folder and run `SetupPsychtoolbox.m` directly.
1. For 18.04 install gamemode from https://launchpad.net/~samoilov-lex/+archive/ubuntu/gamemode -- for 20.04 it is available in apt already...
1. Tobii Pro Eye Tracker Manager – https://www.tobiipro.com/downloads/ 
1. Visual Studio Code — great general purpose text editor, great Python support — https://code.visualstudio.com [download](https://code.visualstudio.com/docs/?dv=linux64_deb). Built in Git support etc. But other IDEs like [PyCharm](https://www.jetbrains.com/pycharm/) are also great.
1. Android Studio V3.3+ — https://developer.android.com/studio/ — used for the Mymou system. We will use both Java and Kotlin language support, but all new code will use Kotlin alone.
1. Python 3 — I prefer to use [Miniconda](https://conda.io/docs/user-guide/install/index.html) to install then use [conda](https://conda.io/docs/user-guide/tasks/manage-conda.html) to install packages as needed. It is better for each project to use different [environments](https://conda.io/docs/user-guide/tasks/manage-environments.html). APT installed Python + PIP can break very easily, not sure why...
1. Ultimaker Cura — for our Ultimaker 3 3D printer — https://ultimaker.com/en/products/ultimaker-cura-software 

## Problems
1. Android Studio cannot always update SDK and other compnenets properly without a VPN, though sometimes it works OK.

# Languages used for Platform Software

## MATLAB + PTB
For desktop computers, [PTB](http://psychtoolbox.org) is still the best software for dedicated testing, especially for visual cognition tasks and all tasks requiring precise timing. To install PTB, you should use `git` to clone this fork: https://github.com/iandol/Psychtoolbox-3 — for Ubuntu AMD GPUs are better using the open source drivers. We will base new experiment code on this toolbox: https://github.com/iandol/opticka

## Kotlin / Java
We will aim to use Kotlin for all new code for Mymou, the automated behavioural test system. Currently Mymou is written in Java, but Kotlin and Java work well together. We may also use Kotlin for our main backend database system.

### Micronaut / KTor
If we use Kotlin for our backend, then lets investigate http://micronaut.io/ or https://ktor.io

### PostgreSQL or MongoDB
Each database should be suitable for our needs, and possibly if we use Micronaut, each service can use its own database.

## Python 3
We will use Python for general purpose development, and for the Neural network training for Mymou and for [DeepLabCut](https://github.com/AlexEMG/DeepLabCut).


# Error solving:

# Fix GDM login failures
[CTRL]+[ALT]+[F3] - Get log (NanoL [ALT]+[arrow] changes buffer in nano), vim: check what is up in /var/log/ `vim /var/log/`. tips: [enter] to see file and [ctrl]6 to get back (or `:Explore`), [ctrl]z pauses vim and `fg` brings it back to play around while navigating the logs. Or `:term` opens a split terminal and use [ctrl]w to switch between two.
```
journalctl -b-0 > boot.log
nano ./boot.log /var/log/Xorg.0.log
```
Restart GDM:
```
sudo systemctl restart gdm
```
or
```
sudo systemctl stop gdm.service
sudo systemctl start gdm.service
```

Other possibilities:
```
sudo chown username:username .Xauthority
sudo chmod 1777 /tmp
```

# Stop MATLAB OpenGL Errors
```
sudo apt-get purge matlab-support
sudo apt-get install -f matlab-support
```
Say yes to replace C++ libraries.

# Stop CUPS network printers
Two options:

```
sudo vim /etc/cups/cups-browsed.conf
EDIT to: BrowseProtocols none
```

```
sudo systemctl stop cups-browsed 
sudo systemctl disable cups-browsed
```

# WSL and git line endings:
https://www.scivision.co/git-line-endings-windows-cygwin-wsl/
make a `.gitattributes` file then `git config --global core.autocrlf input` & `git config --global core.eol lf`


# Install SpectroCal
Make file /etc/udev/rules.d/99-ftdi.rules with this contents:
```
ACTION=="add", ATTRS{idVendor}=="0861", ATTRS{idProduct}=="1003", RUN+="/sbin/modprobe ftdi_sio" RUN+="/bin/sh -c 'echo 0861 1003 > /sys/bus/usb-serial/drivers/ftdi_sio/new_id'"
```
```
sudo udevadm control --reload
```
Unplug amd replug.

# Run ETM on a second X display
```shell
xsetroot -display :1.1 -solid gray 
DISPLAY=:1.1 xrandr --query
DISPLAY=:1.1 /opt/TobiiProEyeTrackerManager/tobiiproeyetrackermanager
# get screenshot
maim --xdisplay=:1.1 ~/Pictures/$(date +%s).png 
```

# Change Refresh rate
```shell
DISPLAY=:0.1 xrandr -r 120
DISPLAY=:0.1 xrandr --query
```

# Tweak APT Repo
```shell
wget -qO - mirrors.ubuntu.com/mirrors.txt 
sudo sed -i -e 's/archive\.ubuntu\.com/mirrors\.cn99\.com/' /etc/apt/sources.list
```

# Eyelink on Linux
```shell
wget -O - "http://download.sr-support.com/software/dists/SRResearch/SRResearch_key" | sudo apt-key add -
sudo add-apt-repository "deb http://download.sr-support.com/software SRResearch main"
sudo apt-get update
# broken on Ubuntu 20.04:
sudo apt-get install eyelink-display-software
# Should work:
sudo apt-get install eyelinkcore edfapi edf2asc edfconverter
```

# Disable Nouveau if you want to install NVidia driver
```shell
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo update-initramfs -u
sudo reboot
```

