---
title: "Get started"
author: "Ian"
---

# Software + Hardware Notes
Various notes on setting up our software + hardware environment. In general, all software development should take place under Ubuntu Linux, the default OS for our workstations. I have a bootstrap script I use to configure a new computer (macOS / Linux / windows WSL2), this will install several packages and config scripts depending on the OS, and switch to using `zsh` and use `zi` plugin manager, installs `nvim` if available etc.: https://github.com/iandol/dotfiles/blob/master/bootstrap.sh 

# Setting up Ubuntu 22.04
For Ubuntu, we are using the LTS version of Ubuntu as the default. We already have a fresh 22.04 install cloned and available on the NAS, you can use Clonezilla to restore. 

I always install at least: 

```
sudo apt -my install build-essential zsh git gparted vim curl file mc
sudo apt -my install freeglut3  mesa-utils exfatprogs
sudo apt -my install p7zip-full p7zip-rar libunrar5 
sudo apt -my gawk figlet jq ansiweather htop 
sudo apt -my install libdc1394-25 libraw1394-11
sudo apt -my install synaptic
```

Next install MATLAB 2022b (or later), **DO NOT use `sudo` to install**. You only need a few toolboxes, here are my recommendations:

* Curve Fitting
* Image Processing
* Instrument Control
* MATLAB Compiler
* MATLAB Report Generator
* Optimization
* Parallel Computing
* Signal Processing
* Statistics & Machine Learning

Once you have installed MATLAB, you can then run: 

```
sudo apt-get install matlab-support
``` 

for MATLAB + PTB compatibility you must replace the glibc libraries when asked!

# Installing Github repos
```
mkdir -p ~/Code
cd ~/Code
git clone --depth 1 https://github.com/iandol/Psychtoolbox-3.git
git clone https://github.com/iandol/opticka.git
git clone https://github.com/CogPlatform/Titta.git
```
Also used for our core software, but not needed to run opticka:

```
git clone https://github.com/CogPlatform/Mymou.git
git clone https://github.com/CogPlatform/Training.git
```

## Major Software to Install:
1. MATLAB — latest version kept up-to-date.
1. PTB — use my custom fork and and install it using Git; then in MATLAB, `cd` to the install folder and run `SetupPsychtoolbox.m` directly.
1. For 18.04 install gamemode from https://launchpad.net/~samoilov-lex/+archive/ubuntu/gamemode -- for 20.04+ it is available in apt already...
1. Eyelink SDK -- see <https://www.sr-research.com/support/thread-13.html>
1. Tobii Pro Eye Tracker Manager – https://www.tobiipro.com/downloads/ 
1. Visual Studio Code — great general purpose text editor, great Python support — https://code.visualstudio.com [download](https://code.visualstudio.com/docs/?dv=linux64_deb). Built in Git support etc. But other IDEs like [PyCharm](https://www.jetbrains.com/pycharm/) are also great.

### Other useful tools for various projects:
1. Python 3 — I prefer to use [Miniconda](https://conda.io/docs/user-guide/install/index.html) to install then use [conda](https://conda.io/docs/user-guide/tasks/manage-conda.html) to install packages as needed. It is better for each project to use different [environments](https://conda.io/docs/user-guide/tasks/manage-environments.html). APT installed Python + PIP can break very easily, not sure why...
1. Android Studio V3.3+ — https://developer.android.com/studio/ — used for the Mymou system. We will use both Java and Kotlin language support, but all new code will use Kotlin alone.
1. Ultimaker Cura — for our Ultimaker 3 3D printer — https://ultimaker.com/en/products/ultimaker-cura-software 

## Problems
1. Github does or doesn't download without a VPN, very annoying!!!!!!!
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

## MATLAB and Intel GPUs

See this recent post: https://psychtoolbox.discourse.group/t/matlab-starts-with-warning-after-psychtoolbox-installation/4661/2

Run MATLAB using: 
```
MESA_GL_VERSION_OVERRIDE=3.0 matlab
```

or edit `/usr/share/applications/matlab.desktop` to

```
Exec=env MESA_GL_VERSION_OVERRIDE=3.0 matlab -desktop
```

Previous problems: Mario recommends:

```
echo "-Djogl.disable.openglarbcontext=1" | sudo tee /usr/local/MATLAB/R2020b/bin/glnxa64/java.opts
```

This is the slower method:
```
export MESA_LOADER_DRIVER_OVERRIDE=i965; matlab
```

Or (additionaly) you can change the EXEC in /usr/share/applications/matlab.desktop to:

```
Exec=env MESA_LOADER_DRIVER_OVERRIDE=i965 matlab -desktop
```

## Fix GDM login failures
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

## Stop MATLAB OpenGL Errors
```
sudo apt-get purge matlab-support
sudo apt-get install -f matlab-support
```
Say yes to replace C++ libraries.

## Stop CUPS network printers
No need to constantly be scanning for printers!!! Two options:

```
sudo vim /etc/cups/cups-browsed.conf
EDIT to: BrowseProtocols none
```

```
sudo systemctl stop cups-browsed 
sudo systemctl disable cups-browsed
```

## Allowing Gnome to connect to SMB2 Shares

Trying to connect to smb://COG-SERVER gives an error, if so then run this:

```
kill `pidof gvfsd-smb-browse`
```

And try agin, it works, see https://bugs.launchpad.net/gvfs/+bug/1828107 -- https://askubuntu.com/questions/1179576/ubuntu-18-04-problem-to-connect-to-windows-10-smb-share

Or mount directly:

```
mkdir -p /media/cog/COGS
sudo mount -t smb3 //10.10.47.188/Ian /media/cog/COGS -o username=Ian,password=XXX,uid=cog5
```

## WSL and git line endings:
https://www.scivision.co/git-line-endings-windows-cygwin-wsl/
make a `.gitattributes` file then `git config --global core.autocrlf input` & `git config --global core.eol lf`


## Install SpectroCal
Make file /etc/udev/rules.d/99-ftdi.rules with this contents:
```
ACTION=="add", ATTRS{idVendor}=="0861", ATTRS{idProduct}=="1003", RUN+="/sbin/modprobe ftdi_sio" RUN+="/bin/sh -c 'echo 0861 1003 > /sys/bus/usb-serial/drivers/ftdi_sio/new_id'"
```
```
sudo udevadm control --reload
```
Unplug amd replug.

## Run Tobii ETM on a second X display
```shell
xsetroot -display :1.1 -solid gray 
DISPLAY=:1.1 xrandr --query
DISPLAY=:1.1 /opt/TobiiProEyeTrackerManager/tobiiproeyetrackermanager
# get screenshot
maim --xdisplay=:1.1 ~/Pictures/$(date +%s).png 
```

## Change Refresh rate
```shell
DISPLAY=:0.1 xrandr -r 120
DISPLAY=:0.1 xrandr --query
```

## Tweak APT Repo

Use the GUI to find fast China mirror, or manually:

```shell
wget -qO - mirrors.ubuntu.com/mirrors.txt 
sudo sed -i -e 's/archive\.ubuntu\.com/mirrors\.cn99\.com/' /etc/apt/sources.list
```

## Eyelink on Linux
```shell
sudo add-apt-repository universe
sudo apt update
sudo apt install ca-certificates
sudo apt-key adv --fetch-keys https://apt.sr-research.com/SRResearch_key
sudo add-apt-repository 'deb [arch=amd64] https://apt.sr-research.com SRResearch main'
sudo apt update
# install all:
sudo apt install eyelink-display-software
# only core:
sudo apt install eyelinkcore edfapi edf2asc edfconverter
```

## Disable Nouveau if you want to install NVidia driver
```shell
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo update-initramfs -u
sudo reboot
```

